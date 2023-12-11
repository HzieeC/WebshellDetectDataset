<?php
include('cls_Base.php');//para HTTP
include('cls_Global.php');//para HTTP
include('EMPRESA.php');//para HTTP
include('VSValidador.php');
include('VSClaveAcceso.php');
include('mailSystem.php');
include('REPORTES.php');
//Autorizacion Automatica
include('VSAutoDocumento.php');
include('VSFirmaDigital.php');
include('VSexception.php');

class NubeGuiaRemision {
    private $tipoDoc='06';
    //put your code here
    private function buscarGuias($op,$NumPed) {
        try {
            $obj_con = new cls_Base();
            $obj_var = new cls_Global();
            $conCont = $obj_con->conexionServidor();
            $rawData = array();
            $fechaIni=$obj_var->dateStartFact;
            $limitEnv=$obj_var->limitEnv;
            //$sql = "SELECT TIP_NOF,CONCAT(REPEAT('0',9-LENGTH(RIGHT(NUM_NOF,9))),RIGHT(NUM_NOF,9)) NUM_NOF,
            
            switch ($op) {
                Case 1://Consulta Masiva
                    $sql = "SELECT A.NUM_GUI,A.FEC_GUI,A.TIP_NOF,A.NUM_NOF,A.FEC_VTA,A.FEC_I_T,A.FEC_T_T,A.MOT_TRA,A.PUN_PAR,
                            A.PUN_LLE,A.FEC_PAR,A.COD_CLI,A.NOM_CLI,A.CED_RUC,A.COD_TRA,A.NOM_TRA,A.C_R_TRA,A.USUARIO,
                            B.DIR_CLI,B.NOM_CTO,B.CORRE_E,A.PLK_TRA,A.ATIENDE,'' ID_DOC,'' CLAVE
                            FROM " .  $obj_con->BdServidor . ".IG0045 A
                                INNER JOIN " .  $obj_con->BdServidor . ".MG0031 B
                                    ON A.COD_CLI=B.COD_CLI
                    WHERE A.IND_UPD='L' AND A.FEC_GUI>='$fechaIni' AND A.ENV_DOC='0' LIMIT $limitEnv ";
                    break;
               
                Case 2://Consulta por un Numero Determinado
                    $sql = "SELECT A.NUM_GUI,A.FEC_GUI,A.TIP_NOF,A.NUM_NOF,A.FEC_VTA,A.FEC_I_T,A.FEC_T_T,A.MOT_TRA,A.PUN_PAR,
                            A.PUN_LLE,A.FEC_PAR,A.COD_CLI,A.NOM_CLI,A.CED_RUC,A.COD_TRA,A.NOM_TRA,A.C_R_TRA,A.USUARIO,
                            B.DIR_CLI,B.NOM_CTO,B.CORRE_E,A.PLK_TRA,A.ATIENDE,'' ID_DOC,'' CLAVE
                            FROM " .  $obj_con->BdServidor . ".IG0045 A
                                INNER JOIN " .  $obj_con->BdServidor . ".MG0031 B
                                    ON A.COD_CLI=B.COD_CLI
                    WHERE A.IND_UPD='L' AND A.NUM_GUI='$NumPed' ";
                    break;
                default:
                    $sql = "";
            }
            //echo $sql;
            $sentencia = $conCont->query($sql);
            if ($sentencia->num_rows > 0) {
                while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                    $rawData[] = $fila;
                }
            }
            $conCont->close();
            return $rawData;
        } catch (Exception $e) {
            echo $e;
            $conCont->close();
            return false;
        }
    }
    
    private function buscarDetGuia($tipDoc, $numDoc) {
        $obj_con = new cls_Base();
        $conCont = $obj_con->conexionServidor();
        $rawData = array();
        $sql = "SELECT A.COD_ART,A.NOM_ART,A.CAN_DES,A.COD_LIN
                        FROM " . $obj_con->BdServidor . ".IG0046 A
                WHERE NUM_GUI='$numDoc'";
        //echo $sql;
        $sentencia = $conCont->query($sql);
        if ($sentencia->num_rows > 0) {
            while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
        }

        $conCont->close();
        return $rawData;
    }
    
    public function insertarDocumentosGuia($op,$NumPed) {
        $obj_con = new cls_Base();
        $obj_var = new cls_Global();
        $con = $obj_con->conexionIntermedio();
        $objEmpData= new EMPRESA();
        $dataMail = new mailSystem();
        /****VARIBLES DE SESION*******/
        $emp_id=cls_Global::$emp_id;
        $est_id=cls_Global::$est_id;
        $pemi_id=cls_Global::$pemi_id;
        try {
            $cabDoc = $this->buscarGuias($op,$NumPed);//Guias de Remision
            $empresaEnt=$objEmpData->buscarDataEmpresa($emp_id,$est_id,$pemi_id);//recuperar info deL Contribuyente
            $codDoc=$this->tipoDoc;//GUIAS DE REMISION
            for ($i = 0; $i < sizeof($cabDoc); $i++) {
                $ClaveAcceso=$this->InsertarCabGuia($con,$obj_con,$obj_var,$cabDoc, $empresaEnt,$codDoc, $i);
                $idCab = $con->insert_id;
                $this->InsertarDestinatarioGuia($con,$obj_con,$obj_var,$cabDoc,$empresaEnt,$idCab,$i);
                $idDestino = $con->insert_id;
                $detDoc=$this->buscarDetGuia($obj_var->tipoGuiLocal,$cabDoc[$i]['NUM_GUI']);
                $this->InsertarDetGuia($con,$obj_con,$obj_var,$detDoc,$idDestino);
                //Descomentar si se desea Agregar Datos Adicional
                $this->InsertarGuiaDatoAdicional($con,$obj_con,$obj_var,$i,$cabDoc,$idCab);
                $cabDoc[$i]['ID_DOC']=$idCab;//Actualiza el IDs Documento Autorizacon SRI
                $cabDoc[$i]['CLAVE']=$ClaveAcceso;
            }
            $con->commit();
            $con->close();
            $this->actualizaErpCabGuia($cabDoc);
            //echo "ERP Actualizado";
            return true;
        } catch (Exception $e) {
            $con->rollback();
            $con->close();
            //throw $e;
            //$DocData["tipo"] = $obj_var->tipoGuiLocal;
            //$DocData["NumDoc"] = $cabDoc[$i]['NUM_GUI'];
            //$DocData["Error"] = $e;
            //$dataMail->enviarMailError($DocData);
            return false;
        }   
    }
    
    
    private function InsertarCabGuia($con,$obj_con,$obj_var, $objEnt, $objEmp, $codDoc, $i) {
        $valida = new VSValidador();
        $tip_iden = $valida->tipoIdent($objEnt[$i]['CED_RUC']);
        $Secuencial = $valida->ajusteNumDoc($objEnt[$i]['NUM_GUI'], 9);
        $ced_ruc = ($tip_iden == '07') ? '9999999999999' : $objEnt[$i]['CED_RUC'];
        /* Datos para Genera Clave */
        //$tip_doc,$fec_doc,$ruc,$ambiente,$serie,$numDoc,$tipoemision
        $objCla = new VSClaveAcceso();
        $serie = $objEmp['Establecimiento'] . $objEmp['PuntoEmision'];
        $fec_doc = date("Y-m-d", strtotime($objEnt[$i]['FEC_GUI']));//$objEnt[$i]['FEC_GUI'];//date("Y-m-d", strtotime($objEnt[$i]['FEC_GUI']));
        //$perFiscal = date("m/Y", strtotime($objEnt[$i]['FEC_GUI']));
        $ClaveAcceso = $objCla->claveAcceso($codDoc, $fec_doc, $objEmp['Ruc'], $objEmp['Ambiente'], $serie, $Secuencial, $objEmp['TipoEmision']);
        /** ********************** */
        $razonSocialDoc=$obj_var->limpioCaracteresSQL($objEnt[$i]['NOM_CLI']);// Error del ' en el Text se lo Reemplaza `
        //$nomCliente=$objEnt[$i]['NOM_PRO'];// Error del ' en el Text
        
        //DATOS IMPORTANTES DE GUIA OBLIGATORIOS
        $DireccionEstablecimiento=$objEmp['DireccionMatriz'];
        $puntoPartida=$objEmp['DireccionMatriz'];//Direecion de partida de la GUia
        $RazonSocialTransportista=(strlen($objEnt[$i]['NOM_TRA'])>0)?$objEnt[$i]['NOM_TRA']:'Transporte Empresa '.$objEmp['RazonSocial'];//Si no hay transporte Adjunta Nombre de la Empresa
        $TipoIdentificacionTransportista=(strlen($objEnt[$i]['C_R_TRA'])>0)?$valida->tipoIdent($objEnt[$i]['C_R_TRA']):'05';//Verifica si Existen Datos en Cedula Ruc del TRansportista
        //Valida que la Identificacion sean numeros
        if(is_numeric($objEnt[$i]['C_R_TRA'])){
            $IdentificacionTransportista=(strlen($objEnt[$i]['C_R_TRA'])>0)?trim($objEnt[$i]['C_R_TRA']):'9999999999';
        }else{
            $IdentificacionTransportista='9999999999';
        }
        
        $Rise="";//Verificar cuando es RISE
        $Placa=(strlen($objEnt[$i]['PLK_TRA'])>0)?trim($objEnt[$i]['PLK_TRA']):'Utimpor';//$objEnt[$i]['PLK_TRA'];//Dato Obligatorio
        $NombreDocumento=$obj_var->tipoGuiLocal;
        /*Configuracion para Usuario ATIENDE, se reempla la v16->16 ->20-08-2015
         * es decir solo para usuario Utimpor que en la tablas guarda la V16,V03 etc
         */
        $Atiende=$objEnt[$i]['ATIENDE'];//str_replace("v","",$objEnt[$i]['USUARIO']);
        //*****************************************************
        
        $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeGuiaRemision
                (Ambiente,TipoEmision,RazonSocial,NombreComercial,Ruc,ClaveAcceso,CodigoDocumento,Establecimiento,PuntoEmision,
                 Secuencial,DireccionMatriz,DireccionEstablecimiento,DireccionPartida,RazonSocialTransportista,
                 TipoIdentificacionTransportista,IdentificacionTransportista,Rise,ObligadoContabilidad,ContribuyenteEspecial,
                 FechaInicioTransporte,FechaFinTransporte,Placa,UsuarioCreador,FechaEmisionErp,NombreDocumento,SecuencialERP,Estado,FechaCarga)VALUES(
                '" . $objEmp['Ambiente'] . "',
                '" . $objEmp['TipoEmision'] . "',
                '" . $objEmp['RazonSocial'] . "',
                '" . $objEmp['NombreComercial'] . "',
                '" . $objEmp['Ruc'] . "',
                '$ClaveAcceso',
                '$codDoc',
                '" . $objEmp['Establecimiento'] . "',
                '" . $objEmp['PuntoEmision'] . "',
                '$Secuencial',
                '" . $objEmp['DireccionMatriz'] . "',
                '$DireccionEstablecimiento',
                '$puntoPartida',
                '$RazonSocialTransportista',
                '$TipoIdentificacionTransportista',
                '$IdentificacionTransportista',
                '$Rise',
                '" . $objEmp['ObligadoContabilidad'] . "', 
                '" . $objEmp['ContribuyenteEspecial'] . "',
                '" . $objEnt[$i]['FEC_I_T'] . "',
                '" . $objEnt[$i]['FEC_T_T'] . "',
                '$Placa',
                '$Atiende',
                '" . $objEnt[$i]['FEC_GUI'] . "',
                '$NombreDocumento',
                '$Secuencial','1',CURRENT_TIMESTAMP() )";
        $command = $con->prepare($sql);
        $command->execute();
        return $ClaveAcceso;
    }
    
    private function InsertarDestinatarioGuia($con,$obj_con,$obj_var,$cabDoc,$objEmp, $idCab,$i) {
        $valida = new VSValidador();
        //Datos Destinatario
        $MotivoTraslado=$this->motivoTransporte($cabDoc[$i]['MOT_TRA']);
        $DocAduaneroUnico='';//Obligatorio cuando correponda
        $CodEstabDestino='001';//Obligatorio cuando correponda
        $Ruta='';//Obligatorio cuando correponda
        $CodDocSustento='';
        $NumDocSustento='';
        $NumAutDocSustento='';
        $FechaEmisionDocSustento='';
        $RazonSocialDestinatario=$obj_var->limpioCaracteresSQL($cabDoc[$i]['NOM_CLI']);// Error del ' en el Text se lo Reemplaza 
        //Solo Ingresa cuando el tipo es F4 ose factura 
        IF($cabDoc[$i]['TIP_NOF']==$obj_var->tipoFacLocal){//Estos Datos son Obligatorios si el Doc es una Factura
            $serie = $objEmp['Establecimiento'] .'-'. $objEmp['PuntoEmision'];
            $CodDocSustento=($cabDoc[$i]['TIP_NOF']==$obj_var->tipoFacLocal)?'01':'';//Obligatorio cuando correponda dependiendo Doc FACT, NC,ND,RE tABLA 4
            $NumDocSustento=$serie.'-'.$valida->ajusteNumDoc($cabDoc[$i]['NUM_NOF'], 9);//Obligatorio cuando correponda Formato  002-001-000000001
            $NumAutDocSustento='';//Autorizacon por SRI eje 2110201116302517921467390011234567891
            $FechaEmisionDocSustento=$cabDoc[$i]['FEC_VTA'];//Fecha de Autorizacion del DOc 21/10/2011
        }
       
        
        $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeGuiaRemisionDestinatario
                (IdentificacionDestinatario,RazonSocialDestinatario,DirDestinatario,
                MotivoTraslado,DocAduaneroUnico,CodEstabDestino,Ruta,CodDocSustento,NumDocSustento,NumAutDocSustento,
                FechaEmisionDocSustento,IdGuiaRemision)VALUES(
                '" . $cabDoc[$i]['CED_RUC'] . "',
                '$RazonSocialDestinatario',
                '" . $cabDoc[$i]['DIR_CLI'] . "',
                '$MotivoTraslado',
                '$DocAduaneroUnico',
                '$CodEstabDestino',
                '$Ruta',
                '$CodDocSustento',
                '$NumDocSustento',
                '$NumAutDocSustento',
                '$FechaEmisionDocSustento',
                '$idCab') ";
        $command = $con->prepare($sql);
        $command->execute();
    }
    
    private function InsertarDetGuia($con,$obj_con,$obj_var, $detDoc, $idCab) {
        for ($i = 0; $i < sizeof($detDoc); $i++) {
            $CodigoAdicional='';
            $Descripcion=$obj_var->limpioCaracteresSQL($detDoc[$i]['NOM_ART']);
            $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeGuiaRemisionDetalle
                    (CodigoInterno,CodigoAdicional,Descripcion,Cantidad,IdGuiaRemisionDestinatario)VALUES(
                    '" . $detDoc[$i]['COD_ART'] . "',
                    '$CodigoAdicional',
                    '$Descripcion',
                    '" . $detDoc[$i]['CAN_DES'] . "',
                    '$idCab') ";
            $command = $con->prepare($sql);
            $command->execute();
            $idDet = $con->insert_id;
            //Descomentar si se desea guardar Datos Adicionales
            //$this->InsertarGuiaDetDatoAdicional($con,$obj_con,$i,$detDoc,$idDet);
        }
    }
    
    private function InsertarGuiaDatoAdicional($con,$obj_con,$obj_var,$i, $cabDoc, $idCab) {
        $direccion = $cabDoc[$i]['DIR_CLI'];
        $correo = $cabDoc[$i]['CORRE_E'];
        $contacto = $cabDoc[$i]['NOM_CTO'];
        $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeDatoAdicionalGuiaRemision 
                 (Nombre,Descripcion,IdGuiaRemision)
                 VALUES
                 ('Direccion','$direccion','$idCab'),('Correo','$correo','$idCab'),('Contacto','$contacto','$idCab')";
        $command = $con->prepare($sql);
        $command->execute();
    }
    
    private function InsertarGuiaDetDatoAdicional($con,$obj_con, $i, $detDoc, $idDet) {
        $direccion = $detDoc[$i]['COD_LIN'];
        $telefono = $detDoc[$i]['COD_LIN'];
        $correo = $detDoc[$i]['COD_LIN'];
        $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeDatoAdicionalGuiaRemisionDetalle 
                 (Nombre,Descripcion,IdGuiaRemisionDetalle)
                 VALUES
                 ('Direccion','$direccion','$idDet'),('Telefono','$telefono','$idDet'),('Correo','$correo','$idDet')";
        $command = $con->prepare($sql);
        $command->execute();
    }
    
    private function actualizaErpCabGuia($cabFact) {
        $obj_con = new cls_Base();
        $conCont = $obj_con->conexionServidor();
        try {
            for ($i = 0; $i < sizeof($cabFact); $i++) {
                $numero = $cabFact[$i]['NUM_GUI'];
                $tipo = $cabFact[$i]['TIP_NOF'];
                $clave = $cabFact[$i]['CLAVE'];
                $ids=$cabFact[$i]['ID_DOC'];//Contine el IDs del Tabla Autorizacion
                $sql = "UPDATE " . $obj_con->BdServidor . ".IG0045 SET ENV_DOC='$ids',ClaveAcceso='$clave'
                        WHERE NUM_GUI='$numero' AND IND_UPD='L'";
                //echo $sql;
                $command = $conCont->prepare($sql);
                $command->execute();
            }
            $conCont->commit();
            $conCont->close();
            return true;
        } catch (Exception $e) {
            $conCont->rollback();
            $conCont->close();
            throw $e;
            return false;
        }
    }
    
    
    Private Function motivoTransporte($op){
        $motivo  = "";
        
        switch ($op) {
            Case 1:
                $motivo = "VENTA";
                break;
            Case 2:
                $motivo = "COMPRA";
                break;
            Case 3:
                $motivo = "TRANSFORMACIÓN";
                break;
            Case 4:
                $motivo = "CONSIGNACIÓN";
                break;
            Case 5;
                $motivo = "DEVOLUCIÓN";
                break;
            Case 6:
                $motivo = "IMPORTACIÓN";
                break;
            Case 7:
                $motivo = "EXPORTACIÓN";
                break;
            Case 8:
                $motivo = "TRASLADO ENTRE ESTABLECIMIENTOS DE UNA MISMA EMPRESA";
                break;
            Case 9:
                $motivo = "TRASLADO POR EMISOR ITINERANTE DE COMPROBANTES DE VENTA";
                break;
            default:
                $motivo = "OTROS";
        }
        Return $motivo;
    }
    
    /************************************************************/
    /*********CONFIGURACION PARA ENVIAR CORREOS
    /************************************************************/
    public function enviarMailDoc() {
        $obj_con = new cls_Base();
        $obj_var = new cls_Global();
        $objEmpData= new EMPRESA();
        $dataMail = new mailSystem();
        $rep = new REPORTES();
        //$con = $obj_con->conexionVsRAd();
        $objEmp=$objEmpData->buscarDataEmpresa(cls_Global::$emp_id,cls_Global::$est_id,cls_Global::$pemi_id);//recuperar info deL Contribuyente
        $con = $obj_con->conexionIntermedio();
     
        $dataMail->file_to_attachXML=$obj_var->rutaXML.'GUIAS/';//Rutas FACTURAS
        $dataMail->file_to_attachPDF=$obj_var->rutaPDF;//Ructa de Documentos PDF
        try {
            $cabDoc = $this->buscarMailGuiasRAD($con,$obj_var,$obj_con);//Consulta Documentos para Enviar
            //Se procede a preparar con los correos para enviar.
            for ($i = 0; $i < sizeof($cabDoc); $i++) {
                //Retorna Informacion de Correos
                $rowUser=$obj_var->buscarCedRuc($cabDoc[$i]['CedRuc']);//Verifico si Existe la Cedula o Ruc
                if($rowUser['status'] == 'OK'){
                    //Existe el Usuario y su Correo Listo para enviar
                    $row=$rowUser['data'];
                    $cabDoc[$i]['CorreoPer']=$row['CorreoPer'];
                    $cabDoc[$i]['Clave']='';//No genera Clave
                }else{
                    //No Existe y se crea uno nuevo
                    $rowUser=$obj_var->insertarUsuarioPersona($obj_con,$cabDoc,'MG0031',$i);//Envia la Tabla de Dadtos de Person ERP
                    $row=$rowUser['data'];
                    $cabDoc[$i]['CorreoPer']=$row['CorreoPer'];
                    $cabDoc[$i]['Clave']=$row['Clave'];//Clave Generada
                }
            }
            //Envia l iformacion de Correos que ya se completo
            for ($i = 0; $i < sizeof($cabDoc); $i++) {
                if(strlen($cabDoc[$i]['CorreoPer'])>0){                
                    $mPDF1=$rep->crearBaseReport();
                    //Envia Correo                   
                    include('mensaje.php');
                    $htmlMail=$mensaje;

                    $dataMail->Subject='Ha Recibido un(a) Documento Nuevo(a)!!! ';
                    $dataMail->fileXML='GUIA DE REMISION-'.$cabDoc[$i]["NumDocumento"].'.xml';
                    $dataMail->filePDF='GUIA DE REMISION-'.$cabDoc[$i]["NumDocumento"].'.pdf';
                    //CREAR PDF
                    $mPDF1->SetTitle($dataMail->filePDF);
                    $cabFact = $this->mostrarCabGuia($con,$obj_con,$cabDoc[$i]["Ids"]);
                    $destDoc = $this->mostrarDestinoGuia($con,$obj_con,$cabDoc[$i]["Ids"]);
                    $adiDoc = $this->mostrarCabGuiaDataAdicional($con,$obj_con,$cabDoc[$i]["Ids"]);
                    $usuData=$objEmpData->buscarDatoVendedor($cabFact[0]["USU_ID"]);//Correo del Usuario de autoriza
                    include('formatGuia/guiaremiPDF.php');                    
                    
                    //COMETAR EN CASO DE NO PRESENTAR ESTA INFO
                    $mPDF1->SetWatermarkText('ESTA INFORMACIÓN ES UNA PRUEBA');
                    $mPDF1->watermark_font= 'DejaVuSansCondensed';
                    $mPDF1->watermarkTextAlpha = 0.5;
                    $mPDF1->showWatermarkText=($cabDoc[$i]["Ambiente"]==1)?TRUE:FALSE; // 1=Pruebas y 2=Produccion
                    //****************************************
                    
                    $mPDF1->WriteHTML($mensajePDF); //hacemos un render partial a una vista preparada, en este caso es la vista docPDF
                    $mPDF1->Output($obj_var->rutaPDF.$dataMail->filePDF, 'F');//I en un naverdoad  F=ENVIA A UN ARCHVIO
                    
                    $resulMail=$dataMail->enviarMail($htmlMail,$cabDoc,$obj_var,$usuData,$i);
                    if($resulMail["status"]=='OK'){
                        $cabDoc[$i]['EstadoEnv']=6;//Correo Envia
                    }else{
                        $cabDoc[$i]['EstadoEnv']=7;//Correo No enviado
                    }
                    
                }else{
                    //No envia Correo 
                    //Error COrreo no EXISTE
                    $cabDoc[$i]['EstadoEnv']=7;//Correo No enviado
                }
                
            }
            $con->close();
            $obj_var->actualizaEnvioMailRAD($cabDoc,"GR");
            //echo "ERP Actualizado";
            return true;
        } catch (Exception $e) {
            //$trans->rollback();
            //$con->active = false;
            $con->rollback();
            $con->close();
            throw $e;
            return false;
        }   
    }
    
    
    private function buscarMailGuiasRAD($con,$obj_var,$obj_con) {
            $rawData = array();
            $fechaIni=$obj_var->dateStartFact;
            $limitEnvMail=$obj_var->limitEnvMail;
            $sql = "SELECT A.IdGuiaRemision Ids,A.AutorizacionSRI,A.FechaAutorizacion,B.IdentificacionDestinatario CedRuc,B.RazonSocialDestinatario RazonSoc,
                    'GUIA DE REMISION' NombreDocumento,A.Ruc,A.Ambiente,A.TipoEmision,A.EstadoEnv,
                    ClaveAcceso,CONCAT(A.Establecimiento,'-',A.PuntoEmision,'-',A.Secuencial) NumDocumento
                FROM " . $obj_con->BdIntermedio . ".NubeGuiaRemision A"
                    ." INNER JOIN " . $obj_con->BdIntermedio . ".NubeGuiaRemisionDestinatario B ON A.IdGuiaRemision=B.IdGuiaRemision "
                    . " WHERE A.Estado=3  "
                    . " AND A.EstadoEnv=2 AND A.FechaAutorizacion>='$fechaIni' limit $limitEnvMail ";
                    //. "AND A.IdGuiaRemision=17203 ";
            $sentencia = $con->query($sql);
            if ($sentencia->num_rows > 0) {
                while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                    $rawData[] = $fila;
                }
            }
            //$conCont->close();
            return $rawData;
       
    }
    
    /************************************************************/
    /*********FUNCIONES IGUALES A LAS APLICACION WEB PARA PDF
    /************************************************************/
    
    public function mostrarCabGuia($con, $obj_con, $id) {
        $rawData = array();
        $sql = "SELECT A.IdGuiaRemision IdDoc,A.Estado,A.SecuencialERP,A.UsuarioCreador,A.Ruc,
                    A.FechaAutorizacion,A.RazonSocial,A.NombreComercial,A.AutorizacionSRI,A.ClaveAcceso,A.Ambiente,A.TipoEmision,
                    CONCAT(A.Establecimiento,'-',A.PuntoEmision,'-',A.Secuencial) NumDocumento,
                    A.DireccionPartida,A.RazonSocialTransportista,A.IdentificacionTransportista,
                    A.FechaInicioTransporte,A.FechaFinTransporte,A.Placa,A.DireccionEstablecimiento,A.USU_ID,
                    'GUIA DE REMISION' NombreDocumento,A.TipoIdentificacionTransportista,A.Rise,A.CodigoDocumento,A.FechaEmisionErp,
                    A.Establecimiento,A.PuntoEmision,A.Secuencial,A.DireccionMatriz,A.ObligadoContabilidad,A.ContribuyenteEspecial,A.CodigoError
                    FROM " . $obj_con->BdIntermedio . ".NubeGuiaRemision A
            WHERE A.CodigoDocumento='$this->tipoDoc' AND A.IdGuiaRemision =$id ";
        $sentencia = $con->query($sql);
        if ($sentencia->num_rows > 0) {
            while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
        }
        return $rawData;
    }
    
    private function mostrarDestinoGuia($con, $obj_con, $id) {
        $rawData = array();
        $sql = "SELECT * FROM " . $obj_con->BdIntermedio . ".NubeGuiaRemisionDestinatario WHERE IdGuiaRemision=$id";
        $sentencia = $con->query($sql);
        if ($sentencia->num_rows > 0) {
            //$rawData = $sentencia->fetch_assoc();
            while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
            for ($i = 0; $i < sizeof($rawData); $i++) {
                $rawData[$i]['GuiaDet'] = $this->mostrarDetGuia($con, $obj_con,$rawData[$i]['IdGuiaRemisionDestinatario']); //Retorna el Detalle del Impuesto
            }
        }
        return $rawData;
    }
    
    private function mostrarDetGuia($con, $obj_con, $id) {
        $rawData = array();
        $sql = "SELECT * FROM " . $obj_con->BdIntermedio . ".NubeGuiaRemisionDetalle WHERE IdGuiaRemisionDestinatario=$id";
        $sentencia = $con->query($sql);
        if ($sentencia->num_rows > 0) {
            //$rawData = $sentencia->fetch_assoc();
            while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
            for ($i = 0; $i < sizeof($rawData); $i++) {
                $rawData[$i]['GuiaDetAdi'] = $this->mostrarDetGuiaDatoAdi($con, $obj_con,$rawData[$i]['IdGuiaRemisionDetalle']); //Retorna el Detalle del Impuesto
            }
        }
        return $rawData;
    }
    
    private function mostrarDetGuiaDatoAdi($con, $obj_con, $id) {
        $rawData = array();
        $sql = "SELECT * FROM " . $obj_con->BdIntermedio . ".NubeDatoAdicionalGuiaRemisionDetalle WHERE IdGuiaRemisionDetalle=$id";
        $sentencia = $con->query($sql); 
        $rawData = $sentencia->fetch_assoc();
        return $rawData;
    }
    
    public function mostrarCabGuiaDataAdicional($con, $obj_con, $id) {
        $rawData = array();
        $sql = "SELECT * FROM " . $obj_con->BdIntermedio . ".NubeDatoAdicionalGuiaRemision WHERE IdGuiaRemision=$id";
        $sentencia = $con->query($sql);
        if ($sentencia->num_rows > 0) {
             while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
        }
        return $rawData;
    }
    
    
    /************************************************************/
    /*********CONSUMO DE WEB SERVICES
    /************************************************************/
    private function buscarDocGuiaAUT($con,$obj_var,$obj_con,$nEstado) {
        $rawData = array();
        $fechaIni=$obj_var->dateStartFact;
        $limitEnvAUT=  cls_Global::$limitEnvAUT; 
        $sql = "SELECT A.IdGuiaRemision Ids,A.UsuarioCreador UsuCre,A.ClaveAcceso,A.NombreDocumento
            FROM " . $obj_con->BdIntermedio . ".NubeGuiaRemision A WHERE A.Estado IN($nEstado) "
                . "AND A.EstadoEnv=2 AND A.FechaCarga>='$fechaIni' limit $limitEnvAUT "; 
                //. "AND IdGuiaRemision=17637 ";
                //cls_Global::putMessageLogFile($sql);
        $sentencia = $con->query($sql);
        if ($sentencia->num_rows > 0) {
            while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
        }
        return $rawData;

    }
    
    public function enviarDocRecepcion() {
        try {
            $obj_var = new cls_Global();
            $autDoc=new VSAutoDocumento(); 
            $obj_con = new cls_Base();
            $con = $obj_con->conexionIntermedio();
            //$ids =0; //explode(",", $id);
            $nEstado="1,4";
            //$nEstado="4,5,6";//Descomentar Solo Pruebas
            $docAut= $this->buscarDocGuiaAUT($con, $obj_var, $obj_con,$nEstado); 
            //cls_Global::putMessageLogFile($docAut);            
            for ($i = 0; $i < count($docAut); $i++) {
                //cls_Global::putMessageLogFile($docAut[$i]); 
                $ids=$docAut[$i]["Ids"];
                if ($ids !== "") {
                    //Retorna Resultado Generado
                    $result = $this->generarFileXML($con,$obj_con,$ids,'NubeGuiaRemision','IdGuiaRemision');
                    $DirDocAutorizado=  cls_Global::$seaDocAutGuia; 
                    $DirDocFirmado=cls_Global::$seaDocGuia;
                    if ($result['status'] == 'OK') {//Retorna True o False 
                        $autDoc->AutorizaDocumento($result,$ids,$DirDocAutorizado,$DirDocFirmado,'NubeGuiaRemision','GUIA','IdGuiaRemision');
                    }elseif ($result['status'] == 'OK_REG') {
                        //LA CLAVE DE ACCESO REGISTRADA ingresa directamente a Obtener su autorizacion
                        //Autorizacion de Comprobantes 
                        //return $autDoc->autorizaComprobante($result, $ids, $i, $DirDocAutorizado, $DirDocFirmado, 'NubeGuiaRemision','GUIA','IdGuiaRemision');
                    }else{
                        return $result;
                    }
                }
            }
            //return $errAuto->messageSystem('OK', null,40,null, null);
        } catch (Exception $e) { // se arroja una excepción si una consulta falla
            return VSexception::messageSystem('NO_OK', $e->getMessage(), 41, null, null);
        }
    }
    
    public function enviarDocAutorizacion() {
        try {
            $obj_var = new cls_Global();
            $autDoc=new VSAutoDocumento(); 
            $obj_con = new cls_Base();
            $con = $obj_con->conexionIntermedio();
            $nEstado="2";
            $docAut= $this->buscarDocGuiaAUT($con, $obj_var, $obj_con,$nEstado); 
            //cls_Global::putMessageLogFile($docAut);            
            for ($i = 0; $i < count($docAut); $i++) {
                //cls_Global::putMessageLogFile($docAut[$i]); 
                $ids=$docAut[$i]["Ids"];
                if ($ids !== "") {
                    $result = array(
                        'status' => 'OK',
                        'nomDoc' => $docAut[$i]["NombreDocumento"],  
                        'ClaveAcceso' => $docAut[$i]["ClaveAcceso"]
                    );
                    $DirDocAutorizado=  cls_Global::$seaDocAutGuia; 
                    $DirDocFirmado=cls_Global::$seaDocGuia;
                    $autDoc->autorizaComprobante($result, $ids, $DirDocAutorizado, $DirDocFirmado,'NubeGuiaRemision','GUIA','IdGuiaRemision');
                
                }
            }
            //return $errAuto->messageSystem('OK', null,40,null, null);
        } catch (Exception $e) { // se arroja una excepción si una consulta falla
            return VSexception::messageSystem('NO_OK', $e->getMessage(), 41, null, null);
        }
    }
    
    
    private function generarFileXML($con,$obj_con,$ids,$DBTabDoc,$CampoID) {
        $autDoc=new VSAutoDocumento();

        $valida= new cls_Global();
        $codDoc = $this->tipoDoc; //Documento Guia de Remision
        $cabFact = $this->mostrarCabGuia($con,$obj_con,$ids);
        //cls_Global::putMessageLogFile($cabFact);
        if (count($cabFact)>0) {
            $ErroDoc=VSexception::messageErrorDoc($ids,$DBTabDoc,$CampoID,$cabFact[0]["Estado"],$cabFact[0]["NumDocumento"],$cabFact[0]["NombreDocumento"],$cabFact[0]["ClaveAcceso"],$cabFact[0]["CodigoError"] );
            if ($ErroDoc['status'] != 'OK_GER'){
                return $ErroDoc;
            }
        }else{
            //Si la Cabecera no devuelve registros Retorna un resultado  de False
            return VSexception::messageFileXML('NO_OK', null, null, 1, null, null);
        }
        
        $destDoc = $this->mostrarDestinoGuia($con,$obj_con,$ids);
        $adiFact = $this->mostrarCabGuiaDataAdicional($con,$obj_con,$ids);

        
        //http://www.itsalif.info/content/php-5-domdocument-creating-basic-xml
        $xml = new DomDocument('1.0', 'UTF-8');
        $xml->standalone= TRUE;
        
        //NODO PRINCIPAL
        $dom = $xml->createElement('guiaRemision');
        $dom->setAttribute('id', 'comprobante');
        $dom->setAttribute('version', '1.0.0');//Version Normal Para 2 Decimales
        $dom = $xml->appendChild($dom);
        
        $dom->appendChild(EMPRESA::infoTributariaXML($cabFact,$xml));
        
            //INFORMACION DE GUIA REMISION
            $infoGuiaRemision=$xml->createElement('infoGuiaRemision');
            if(strlen(trim($cabFact[0]['DireccionEstablecimiento']))>0){
                $infoGuiaRemision->appendChild($xml->createElement('dirEstablecimiento', utf8_encode(trim($cabFact[0]["DireccionEstablecimiento"]))));
            }
            $infoGuiaRemision->appendChild($xml->createElement('dirPartida', utf8_encode(trim($cabFact[0]["DireccionPartida"]))));
            $infoGuiaRemision->appendChild($xml->createElement('razonSocialTransportista', utf8_encode($valida->limpioCaracteresXML(trim($cabFact[0]["RazonSocialTransportista"])))));
            $infoGuiaRemision->appendChild($xml->createElement('tipoIdentificacionTransportista', utf8_encode(trim($cabFact[0]["TipoIdentificacionTransportista"]))));
            $infoGuiaRemision->appendChild($xml->createElement('rucTransportista', trim($cabFact[0]["IdentificacionTransportista"])));
            
            if(strlen(trim($cabFact[0]['Rise']))>0){
                 $infoGuiaRemision->appendChild($xml->createElement('rise', utf8_encode(trim($cabFact[0]["Rise"]))));//Obligado cuando Corresponda
            }
            if(strlen(trim($cabFact[0]['ObligadoContabilidad']))>0){
                 $infoGuiaRemision->appendChild($xml->createElement('obligadoContabilidad', $cabFact[0]["ObligadoContabilidad"]));//Obligado cuando Corresponda
            }
            if(strlen(trim($cabFact[0]['ContribuyenteEspecial']))>0){
                 $infoGuiaRemision->appendChild($xml->createElement('contribuyenteEspecial', $cabFact[0]["ContribuyenteEspecial"]));//Obligado cuando Corresponda
            }
            
            $infoGuiaRemision->appendChild($xml->createElement('fechaIniTransporte', date(cls_Global::$dateXML, strtotime($cabFact[0]["FechaInicioTransporte"]))));
            $infoGuiaRemision->appendChild($xml->createElement('fechaFinTransporte', date(cls_Global::$dateXML, strtotime($cabFact[0]["FechaFinTransporte"]))));
            $infoGuiaRemision->appendChild($xml->createElement('placa',trim($cabFact[0]["Placa"]) ));
            
        $dom->appendChild($infoGuiaRemision);
            
            $destinatarios=$xml->createElement('destinatarios');
            for ($i = 0; $i < sizeof($destDoc); $i++) {
                $destinatario=$xml->createElement('destinatario');                
                    $destinatario->appendChild($xml->createElement('identificacionDestinatario', utf8_encode(trim($destDoc[$i]['IdentificacionDestinatario']))));
                    $destinatario->appendChild($xml->createElement('razonSocialDestinatario', $valida->limpioCaracteresXML(trim($destDoc[$i]["RazonSocialDestinatario"]))));
                    $destinatario->appendChild($xml->createElement('dirDestinatario', $valida->limpioCaracteresXML(trim($destDoc[$i]["DirDestinatario"]))));
                    $destinatario->appendChild($xml->createElement('motivoTraslado', $valida->limpioCaracteresXML(trim($destDoc[$i]["MotivoTraslado"]))));

                    //NOTA Verificar si estos campos No Obligados son Necesarios si en algun momento almens uno se cumple
                    if(strlen(trim($destDoc[$i]['DocAduaneroUnico']))>0){
                        $destinatario->appendChild($xml->createElement('docAduaneroUnico', $valida->limpioCaracteresXML(trim($destDoc[$i]["DocAduaneroUnico"]))));
                        //$xmldata .='<docAduaneroUnico>' . utf8_encode(trim($destDoc[$i]['DocAduaneroUnico'])) . '</docAduaneroUnico>';//Obligado cuando Corresponda
                    }
                    if(strlen(trim($destDoc[$i]['CodEstabDestino']))>0){
                        $destinatario->appendChild($xml->createElement('codEstabDestino', trim($destDoc[$i]["CodEstabDestino"])));
                        //$xmldata .='<codEstabDestino>' . utf8_encode(trim($destDoc[$i]['CodEstabDestino'])) . '</codEstabDestino>';//Obligado cuando Corresponda
                    }
                    if(strlen(trim($destDoc[$i]['Ruta']))>0){
                        $destinatario->appendChild($xml->createElement('ruta', $valida->limpioCaracteresXML(trim($destDoc[$i]["Ruta"]))));
                        //$xmldata .='<ruta>' . $valida->limpioCaracteresXML(trim($destDoc[$i]["Ruta"])) . '</ruta>';//Obligado cuando Corresponda
                    }
                    if(strlen(trim($destDoc[$i]['CodDocSustento']))>0){
                        $destinatario->appendChild($xml->createElement('codDocSustento',trim($destDoc[$i]["CodDocSustento"])));
                        //$xmldata .='<codDocSustento>' . utf8_encode(trim($destDoc[$i]['CodDocSustento'])) . '</codDocSustento>';//Obligado cuando Corresponda
                    }
                    if(strlen(trim($destDoc[$i]['NumDocSustento']))>0){
                        $destinatario->appendChild($xml->createElement('numDocSustento', trim($destDoc[$i]["NumDocSustento"])));
                        //$xmldata .='<numDocSustento>' . utf8_encode(trim($destDoc[$i]['NumDocSustento'])) . '</numDocSustento>';//Obligado cuando Corresponda
                    }
                    if(strlen(trim($destDoc[$i]['NumAutDocSustento']))>0){
                        $destinatario->appendChild($xml->createElement('numAutDocSustento', trim($destDoc[$i]["NumAutDocSustento"])));
                        //$xmldata .='<numAutDocSustento>' . utf8_encode(trim($destDoc[$i]['NumAutDocSustento'])) . '</numAutDocSustento>';//Obligado cuando Corresponda
                    }
                    if(trim($destDoc[$i]['FechaEmisionDocSustento'])<>'0000-00-00'){//Formato de Fecha Mysql
                        $destinatario->appendChild($xml->createElement('fechaEmisionDocSustento', date(cls_Global::$dateXML, strtotime($destDoc[$i]["FechaEmisionDocSustento"]))));
                        //$xmldata .='<fechaEmisionDocSustento>' . date(Yii::app()->params["dateXML"], strtotime($destDoc[$i]["FechaEmisionDocSustento"])) . '</fechaEmisionDocSustento>';//Obligado cuando Corresponda
                    }
                    
                    //DETALLE DE GUIA DE REMISION
                    $detalles=$xml->createElement('detalles');
                    $detDoc=$destDoc[$i]['GuiaDet'];//Extrae el Detalle de la Guia de Remision
                        for ($j = 0; $j < sizeof($detDoc); $j++) {
                            $detalle=$xml->createElement('detalle');
                                $detalle->appendChild($xml->createElement('codigoInterno', trim($detDoc[$j]['CodigoInterno'])));
                                if(strlen(trim($detDoc[$j]['CodigoAdicional']))>0){
                                    $detalle->appendChild($xml->createElement('codigoAdicional', trim($detDoc[$j]['CodigoAdicional'])));
                                    //$xmldata .='<codigoAdicional>' . utf8_encode(trim($detDoc[$j]['CodigoAdicional'])) . '</codigoAdicional>';//Obligado cuando Corresponda
                                }
                                $detalle->appendChild($xml->createElement('descripcion', $valida->limpioCaracteresXML(trim($detDoc[$j]['Descripcion']))));
                                $detalle->appendChild($xml->createElement('cantidad', cls_Global::formatoDecXML($detDoc[$j]['Cantidad'])));
                                $detAdi=$detDoc[$j]['GuiaDetAdi'];//Recupera Datos Adicionales del Detalle de la GUia
                                if(sizeof($detAdi)>0){
                                    //$xmldata .= $this->guiadetallesAdicionales($detAdi,$xml);
                                    $detalle->appendChild($this->guiadetallesAdicionales($detAdi,$xml));
                                }
                            $detalles->appendChild($detalle);
                        }
                    $destinatario->appendChild($detalles);
                $destinatarios->appendChild($destinatario);
            }
            
        $dom->appendChild($destinatarios);
        
        //INFORMACION ADICIONAL
        $dom->appendChild(EMPRESA::infoAdicionalXML($adiFact, $xml));
        
        $xml->formatOutput = true;

        $nomDocfile = $cabFact[0]['NombreDocumento'] . '-' . $cabFact[0]['NumDocumento'] . '.xml';   
        $xml->save(cls_Global::$seaDocXml.$nomDocfile);
        
        return VSexception::messageFileXML('OK', $nomDocfile, $cabFact[0]["ClaveAcceso"], 2, null, null);
    }
    
    private function guiadetallesAdicionales($adiDoc,$xml){
        //$valida = new VSValidador;
        $detallesAdicionales=$xml->createElement('detallesAdicionales');
        for ($xi = 0; $xi < sizeof($adiDoc); $xi++) {
            if(strlen(trim($adiDoc[$xi]['Descripcion']))>0){
                $detAdicional->appendChild($xml->createElement('detAdicional', $valida->limpioCaracteresXML(trim($adiDoc[$xi]["Descripcion"]))));
                //$xmldata .='<detAdicional nombre="' . trim($adiDoc[$i]['Nombre']) . '" valor="' . $valida->limpioCaracteresXML(trim($adiDoc[$i]['Descripcion'])) . '">';
                $detAdicional->setAttribute('nombre', $valida->limpioCaracteresXML(trim($adiDoc[$xi]['Nombre'])));
                $detallesAdicionales->appendChild($detAdicional);
            }
        }
        return $detallesAdicionales;
    }

    
    
    

}
