<?php

/*
 * Control de AUTORIZACION de Documentos 
 *  */
//include('VSFirmaDigital.php');
//include('VSexception.php');
class VSAutoDocumento {

    public function AutorizaDocumento($result,$ids,$DirDocAutorizado,$DirDocFirmado,$DBTabDoc,$DocErr,$CampoID) {
        $firmaDig = new VSFirmaDigital();
        $firma = $firmaDig->firmaXAdES_BES($result['nomDoc'],$DirDocFirmado);
        //Verifica Errores del Firmado
        if ($firma['status'] == 'OK') {
            //Validad COmprobante
            $valComp = $firmaDig->validarComprobanteWS($result['nomDoc'],$DirDocFirmado); //Envio NOmbre Documento
            if ($valComp['status'] == 'OK') {//Retorna Datos del Comprobacion
                //Verifica si el Doc Fue Recibido Correctamente...
                $Rac = $valComp['data']['RespuestaRecepcionComprobante'];
                $estadoRac = $Rac['estado'];
                if ($estadoRac == 'RECIBIDA') {
                    //Continua con el Proceso
                    //Autorizacion de Comprobantes                     
                    //CAMBIO METODO OFFLINE 29-11-2016
                    //return $this->autorizaComprobante($result, $ids, $DirDocAutorizado, $DirDocFirmado, $DBTabDoc, $DocErr, $CampoID);
                    return $this->actualizaDocRecibidoSri($Rac, $ids, $result['nomDoc'], $DirDocAutorizado, $DirDocFirmado, $DBTabDoc, $DocErr, $CampoID);
                } else {
                    //Verifica si la Clave esta en Proceso de Validacion
                    if ($estadoRac == 'DEVUELTA') {
                        //Actualiza Errores de Documento Devuelto...
                        $AdrSri= $this->recibeDocSriDevuelto($Rac, $ids, $result['nomDoc'], $DirDocFirmado,$DBTabDoc,$DocErr,$CampoID);
                        if (count($ids) == 1) {//Sale directamente si solo tiene un Domento para validadr
                            return $AdrSri; //Si la autorizacion es uno a uno.
                        }
                    }
                }
            } else {
                //Si Existe un error al realizar la peticion al Web Servicies
                return VSexception::messageSystem('NO_OK', $valComp["error"], 4, "Ids=>$ids", null);
            }
        } else {
            //Sin No hay firma Automaticamente Hay que Parar el Envio
            return VSexception::messageSystem('NO_OK', $firma["error"], 3, "Ids=>$ids", null);
        }
        return VSexception::messageSystem('OK', null,40,null, null);//Si nunka tuvo un Error Devuelve OK
    }
    
    public function autorizaComprobante($result, $ids, $DirDocAutorizado, $DirDocFirmado, $DBTabDoc, $DocErr, $CampoID) {
        $firmaDig = new VSFirmaDigital();
        //Continua con el Proceso
        //Autorizacion de Comprobantes
        $autComp = $firmaDig->autorizacionComprobanteWS($result['ClaveAcceso']); //Envio CLave de Acceso
        //cls_Global::putMessageLogFile($autComp);
        if ($autComp['status'] == 'OK') {
            //Validamos el Numero de Autorizacin que debe ser Mayor a 0
            //Nota verificar si en el metodo Offline devuelve la autorizacion 
            $numeroAutorizacion = (int) $autComp['data']['RespuestaAutorizacionComprobante']['numeroComprobantes'];
            /******************************************************* */
            //Operacion de Stop, si no hay ningun Documento AUtorizado sale automaticamente de la funcion Autoriza
            //Su finalidad es que no siga realizado el resto de las operaciones y continuar con la siguiente.
            if ($numeroAutorizacion == 0) {
                //$mError="No podemos encontrar la información que está solicitando.";
                if ($autComp['error'] == 0) {                    
                    $autorizacion = $autComp['data']['RespuestaAutorizacionComprobante']['autorizaciones']['autorizacion'];
                    return $this->actualizaDocRecibidoSri($autorizacion, $ids, $result['nomDoc'], $DirDocAutorizado, 
                                                            $DirDocFirmado, $DBTabDoc, $DocErr, $CampoID);
                }else{
                    return VSexception::messageSystem('NO_OK', "$DBTabDoc=>> $CampoID=$ids =>>".$autComp["error"], 22, null, null);
                }
            }
            //Por favor volver a Intentar en unos minutos
            /******************************************************* */
            $autorizacion = $autComp['data']['RespuestaAutorizacionComprobante']['autorizaciones']['autorizacion'];
            if ($numeroAutorizacion == 1) {//Verifica si Autorizo algun Comprobante Apesar de recibirlo Autorizo Comprobante
                $AdrSri = $this->actualizaDocRecibidoSri($autorizacion, $ids, $result['nomDoc'], $DirDocAutorizado, $DirDocFirmado, $DBTabDoc, $DocErr, $CampoID);
                //Guarda Documento Autorizado
                $this->newXMLDocRecibidoSri($autorizacion, $result['nomDoc'], $DirDocAutorizado);
                if (count($ids) == 1) {//Sale directamente si solo tiene un Domento para validadr
                    return $AdrSri; //Si la autorizacion es uno a uno.
                }
            } else {
                //Ingresa si el Documento a intentado Varias AUTORIZACIONES
                if ($numeroAutorizacion > 1) {
                    for ($c = 0; $c < sizeof($autorizacion); $c++) {
                        $this->actualizaDocRecibidoSri($autorizacion[$c], $ids, $result['nomDoc'], $DirDocAutorizado, $DirDocFirmado, $DBTabDoc, $DocErr, $CampoID);
                        if ($autorizacion[$c]['estado'] == 'AUTORIZADO') {
                            $this->newXMLDocRecibidoSri($autorizacion[$c], $result['nomDoc'], $DirDocAutorizado);
                            break; //Si de todo el Recorrido Existe un Autorizado 
                        }
                    }
                }
            }
        } else {
            //$mError="(Error al Realizar la Autorizacion del Documento)";
            return VSexception::messageSystem('NO_OK', "Ids=$ids =>>".$autComp["error"], 22, null, null);
        }
    }

    private function actualizaDocRecibidoSri($response,$ids,$NombreDocumento,$DirDocAutorizado,$DirDocFirmado,$DBTabDoc,$DocErr,$CampoID) {
        $obj_con = new cls_Base();
        $con = $obj_con->conexionIntermedio();
        $msg= new VSexception();
        $status="";
        try {
            //$UsuId=1;//Usuario Admin //Yii::app()->getSession()->get('user_id', FALSE);
            $estado = $response['estado'];
            $fecha = ($response['fechaAutorizacion']!=null)?date("Y-m-d H:i:s", strtotime($response['fechaAutorizacion'])):date('Y-m-d H:i:s');
            $numeroAutorizacion='';
            $CodigoError='';
            $DescripcionError='';
            if($estado=='RECIBIDA'){
                $codEstado='2';
                $DirectorioDocumento=$DirDocAutorizado;            
            }elseif($estado=='AUTORIZADO'){
                //$fecha = date("Y-m-d H:i:s", strtotime($response['fechaAutorizacion']));//date(Yii::app()->params["datebytime"], strtotime($response['fechaAutorizacion']));
                $numeroAutorizacion = ($response['numeroAutorizacion']!=null)?$response['numeroAutorizacion']:'';
                $codEstado='3';
                $DirectorioDocumento=$DirDocAutorizado;
                $op=15;//Su documento fue autorizado correctamente
                $status="OK";
            }elseif($estado=='EN PROCESO'){
                //DOCUMENTO RECIBIDO Y EN PROCESO DE AUTORIZACION
                $codEstado='9';
                $DirectorioDocumento=$DirDocAutorizado;
            }else{
                //NO AUTORIZADOS,RECHAZADO, NEGADO O DEVUELTAS
                $codEstado='6';
                $op=16;//Su documento fue rechazado o negado
                $status="NO_OK";
                $mensaje=$response['mensajes']['mensaje'];//Array de Errores Sri
                $this->mensajeErrorDocumentos($con,$mensaje,$ids,$DocErr);
                //$CodigoError=$mensaje[0]['identificador'];
                //$InformacionAdicional=(!empty($mensaje[0]['informacionAdicional']))?$mensaje[0]['informacionAdicional']:'';
                $CodigoError=$mensaje['identificador'];
                $InformacionAdicional=(!empty($mensaje['informacionAdicional']))?$mensaje['informacionAdicional']:'';
                $DescripcionError=utf8_encode("$CampoID=>$ids ID=>$CodigoError Error=> $InformacionAdicional");
                $DirectorioDocumento=$DirDocFirmado;
            }
            //,USU_ID="'.$UsuId.'"
            $sql = 'UPDATE ' . $obj_con->BdIntermedio . '.'.$DBTabDoc.' SET 
                                AutorizacionSRI="'.$numeroAutorizacion.'",FechaAutorizacion="'.$fecha.'",
                                DirectorioDocumento="'.$DirectorioDocumento.'",NombreDocumento="'.$NombreDocumento.'",
                                EstadoDocumento="'.$estado.'",Estado="'.$codEstado.'",
                                DescripcionError="'.$DescripcionError.'",CodigoError="'.$CodigoError.'"
                            WHERE '.$CampoID.'='.$ids ;
            //echo $sql;
            $command = $con->prepare($sql);
            $command->execute();

            $con->commit();
            $con->close();
            //$mError="No podemos encontrar la información que está solicitando.";
            return VSexception::messageSystem($status, $DescripcionError, $op, $DescripcionError, null);
        } catch (Exception $e) {
            $con->rollback();
            $con->close();
            //throw $e;
            return VSexception::messageSystem('NO_OK', $e->getMessage(), 41, null, null);
        }
    }
    
    private function recibeDocSriDevuelto($response, $ids, $NombreDocumento, $DirDocFirmado,$DBTabDoc,$DocErr,$CampoID) {
        $valida= new cls_Global();
        $obj_con = new cls_Base();
        $con = $obj_con->conexionIntermedio();
        try {
            //$UsuId=1;//Usuario Admin //Yii::app()->getSession()->get('user_id', FALSE);
            $estado = $response['estado'];
            $CodigoError = '';
            $DescripcionError = '';
            $comprobanteRac = $response['comprobantes']['comprobante'];            
            $mensaje = $comprobanteRac['mensajes']['mensaje']; //Array de Errores Sri
            $this->mensajeErrorDocumentos($con,$mensaje,$ids,$DocErr);
            if (sizeof($mensaje)>0){
                //Cuando es Varios Mensajes Guarda solo el ultimo.
                $nEnd=sizeof($mensaje)-1;//El ultimo Items
                if($mensaje[$nEnd]['identificador']<>''){
                    $CodigoError = $mensaje[$nEnd]['identificador'];                
                    //$MensajeSRI = $mensaje[$nEnd]['mensaje'];
                    //$InformacionAdicional = (!empty($mensaje[$nEnd]['informacionAdicional'])) ? $mensaje[$nEnd]['informacionAdicional'] : ''; 
                    $MensajeSRI=utf8_encode($valida->limpioCaracteresXML(trim($mensaje[$nEnd]['mensaje'])));
                    $InformacionAdicional=(!empty($mensaje[$nEnd]['informacionAdicional']))?utf8_encode($valida->limpioCaracteresXML(trim($mensaje[$nEnd]['informacionAdicional']))):'';
                }else{
                    //En Caso de que sea 1SOLO mensaje
                    $CodigoError = $mensaje['identificador'];
                    $MensajeSRI=utf8_encode($valida->limpioCaracteresXML(trim($mensaje['mensaje'])));
                    $InformacionAdicional=(!empty($mensaje['informacionAdicional']))?utf8_encode($valida->limpioCaracteresXML(trim($mensaje['informacionAdicional']))):'';
                }              
            }else{
                //Para 1 solo mensaje
                $CodigoError = $mensaje['identificador'];
                //$MensajeSRI = $mensaje['mensaje'];
                $MensajeSRI=utf8_encode($valida->limpioCaracteresXML(trim($mensaje['mensaje'])));
                //$InformacionAdicional = (!empty($mensaje['informacionAdicional'])) ? $mensaje['informacionAdicional'] : '';
                $InformacionAdicional=(!empty($mensaje['informacionAdicional']))?utf8_encode($valida->limpioCaracteresXML(trim($mensaje['informacionAdicional']))):'';
            }
            $DescripcionError = utf8_encode("$CampoID=>$ids ID=>$CodigoError MensSri=>($MensajeSRI) InfAdicional=>($InformacionAdicional)");
            $DirectorioDocumento = $DirDocFirmado;
            
            //Verificar Codigo Error para Actualizar Estado
            //$codEstado = '4';
            $codEstado = $this->estadoError($CodigoError);
            

            //,USU_ID="'.$UsuId.'"
            $sql = 'UPDATE ' . $obj_con->BdIntermedio . '.'.$DBTabDoc.' SET 
                                DirectorioDocumento="'.$DirectorioDocumento.'",NombreDocumento="'.$NombreDocumento.'",
                                EstadoDocumento="'.$estado.'",Estado="'.$codEstado.'",
                                DescripcionError="'.$DescripcionError.'",CodigoError="'.$CodigoError.'"
                            WHERE '.$CampoID.'='.$ids ;
            //echo $sql;
            $command = $con->prepare($sql);
            $command->execute();

            $con->commit();
            $con->close();
            //Su documento fue devuelto por errores en el comprobante
            //Dependiendo del Error arrojado por el SRI
            return VSexception::messageWSSRI('OK', null, $CodigoError, $DescripcionError, null);//Web service Sri
        } catch (Exception $e) {
            $con->rollback();
            $con->close();
            //throw $e;
            return VSexception::messageSystem('NO_OK', $e->getMessage(), 41, null, null);
        }
    }
    
    private function estadoError($Numero) {
        /*  0 = NO ENVIADO
            1 = ENVIADO BASE INTERMEDIA
            2 = RECIBIDO SRI 
            3 = AUTORIZADO SRI 
            4 = EN PROCESO (CLAVE DE ACCESO) Recibir
            5 = ELIMINADO DEL SISTEMA
            6 = RECHAZADO (NO AUTORIZADOS, NEGADO o DEVUELTAS) =>SOLUC VOLVER ENVIAR CON ESTADO 1
            8 = DOCUMENTO ANULADO
            9 = EN PROCESO (AUTORIZACION)
         */
         switch ($Numero) {
             case 58://Error en la estructura de clave acceso 
                return 6;
                break;
            case 70://Clave de acceso en procesamiento 
                return 4;
                break;            
            case 65://Fecha de emisión  extemporánea 
                return 6;
                break;
            default:
                return 6;
        }
    }


    private function mensajeErrorDocumentos($con, $mensaje, $ids, $tipDoc) {
        $valida= new cls_Global();
        $IdFactura='';$IdRetencion='';$IdNotaCredito='';$IdNotaDebito='';$IdGuiaRemision='';
        switch ($tipDoc) {
            case 'FACTURA':
                $IdFactura=$ids;
                break;
            case 'RETENCION':
                $IdRetencion=$ids;
                break;
            case 'NOTA_CREDITO':
                $IdNotaCredito=$ids;
                break;
            case 'NOTA_DEBITO':
                $IdNotaDebito=$ids;
                break;
            default:
                $IdGuiaRemision=$ids;
        }
        if (sizeof($mensaje)>0){
            //Descomentar cuando existan varios Mensajes.
            //Se Guardan Varios Mensajes.
            /*for ($i = 0; $i < sizeof($mensaje); $i++) {
                $Identificador = $mensaje[$i]['identificador'];
                $TipoMensaje = $mensaje[$i]['tipo'];
                $Mensaje = $mensaje[$i]['mensaje'];
                $InformacionAdicional = (!empty($mensaje[$i]['informacionAdicional'])) ? $mensaje[$i]['informacionAdicional'] : '';
                $sql = "INSERT INTO " . $con->BdIntermedio . ".NubeMensajeError 
                 (IdFactura,IdRetencion,IdNotaCredito,IdNotaDebito,IdGuiaRemision,Identificador,TipoMensaje,Mensaje,InformacionAdicional)
                 VALUES
                 ('$IdFactura','$IdRetencion','$IdNotaCredito','$IdNotaDebito','$IdGuiaRemision','$Identificador','$TipoMensaje','$Mensaje','$InformacionAdicional')";
                $command = $con->prepare($sql);
                $command->execute();
                //$status, $error, $op, $message, $data
                $DescripcionError=utf8_encode("$tipDoc Ids=>$ids ID=>$Identificador Error=> $InformacionAdicional");
                VSexception::messageSystem("NO_OK", $TipoMensaje, 0, $Mensaje,$DescripcionError);//Print Error
            }
        }  else {*/
            //Solo para 1 solo Mensaje
            $Identificador=$mensaje['identificador'];
            $TipoMensaje=$mensaje['tipo'];
            //$Mensaje=$mensaje['mensaje']; //Error por Problema de Caracteres Especiales           
            $Mensaje=utf8_encode($valida->limpioCaracteresXML(trim($mensaje['mensaje'])));            
            //$InformacionAdicional=(!empty($mensaje['informacionAdicional']))?$mensaje['informacionAdicional']:'';
            $InformacionAdicional=(!empty($mensaje['informacionAdicional']))?utf8_encode($valida->limpioCaracteresXML(trim($mensaje['informacionAdicional']))):'';
            
            $sql = "INSERT INTO " . $con->BdIntermedio . ".NubeMensajeError 
                 (IdFactura,IdRetencion,IdNotaCredito,IdNotaDebito,IdGuiaRemision,Identificador,TipoMensaje,Mensaje,InformacionAdicional)
                 VALUES
                 ('$IdFactura','$IdRetencion','$IdNotaCredito','$IdNotaDebito','$IdGuiaRemision','$Identificador','$TipoMensaje','$Mensaje','$InformacionAdicional')";
            $command = $con->prepare($sql);
            $command->execute();
            $DescripcionError=utf8_encode("$tipDoc Ids=>$ids ID=>$Identificador Error=> $InformacionAdicional");
            VSexception::messageSystem("NO_OK", $TipoMensaje, 0, $Mensaje,$DescripcionError);//Print Error
        }
        
    }
    
    private function newXMLDocRecibidoSri($response,$NombreDocumento,$DirDocAutorizado) {
        $estado = $response['estado'];
        if ($estado == 'AUTORIZADO') {
            $xmldata=$this->xmlAutoSri($response);
            file_put_contents($DirDocAutorizado . $NombreDocumento, $xmldata); //Escribo el Archivo Xml
            $arroout["status"] = "OK";
            $arroout["error"] = null;
            $arroout["message"] = 'El Xml se recibio correctamente';
            $arroout["data"] = null;
        } else {
            $arroout["status"] = "NO";
            $arroout["error"] = null;
            $arroout["message"] = $NombreDocumento .'El Xml no se Genero';
            $arroout["data"] = null;
        }
        if($arroout["status"]=="NO"){
            cls_Global::putMessageLogFile($arroout);//Imprime el Error en Logs
        }
        return $arroout;
    }
    private function xmlAutoSri($response) {
        $xmldata = '<?php xml version="1.0" encoding="UTF-8"?>';
        $xmldata .= '<autorizacion>';
            $xmldata .= '<estado>' . $response["estado"] . '</estado>';
            $xmldata .= '<numeroAutorizacion>' . $response["numeroAutorizacion"] . '</numeroAutorizacion>';
            $xmldata .= '<fechaAutorizacion class="fechaAutorizacion">' . $this->getFechaAuto($response["fechaAutorizacion"]) . '</fechaAutorizacion>';
            $xmldata .= '<comprobante><![CDATA[' . $response["comprobante"] . ']]></comprobante>';
        $xmldata .= '</autorizacion>';
        return $xmldata;
    }
    
    private function getFechaAuto($fecha) {
         //formato de Fecha Autorizacion=>2014-12-02T21:34:15.637-05:00 =>   02/10/2014 18:59:27 
        $aux = explode(".", trim($fecha));
        $aux = explode("T", trim($aux[0]));//Separamos por medio de la T
        $fecha=date('d/m/Y', strtotime($aux[0])).' '.$aux[1];
        return $fecha;
    }
    

}
