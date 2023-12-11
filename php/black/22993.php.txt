<?php
//include('whebshell/Mod/Base/cls_Base.php');
include('cls_Base.php');//para HTTP
include('cls_Global.php');//para HTTP
include('EMPRESA.php');//para HTTP
include('VSValidador.php');
include('VSClaveAcceso.php');
include('mailSystem.php');
include('REPORTES.php');

//Autorizacin Automatica
include('VSAutoDocumento.php');
include('VSFirmaDigital.php');
include('VSexception.php');

class NubeNotasCredito {
    private $tipDev="DV";
    private $tipoDoc='04';
    
    private function buscarNC($op,$NumPed) {
        try {
            $obj_con = new cls_Base();
            $obj_var = new cls_Global();
            $conCont = $obj_con->conexionServidor();
            $rawData = array();
            $fechaIni=$obj_var->dateStartFact;
            $limitEnv=$obj_var->limitEnv;
            
            switch ($op) {
                Case 1://Devoluciones Cabecera
                    $sql = "SELECT A.TIP_DEV,A.NUM_DEV,A.FEC_DEV,A.COD_MOT,C.NOM_MOT,A.TIP_NOF,A.NUM_NOF,A.FEC_NOF,
                                    A.COD_CLI,B.CED_RUC,B.NOM_CLI,B.DIR_CLI,B.TEL_N01,B.CORRE_E,A.VAL_BRU,A.POR_DES,A.VAL_DES,
                                    A.BAS_IVA,A.BAS_IV0,A.POR_IVA,A.POR_IVA,A.VAL_IVA,A.VAL_NET,A.POR_RET,
                                    A.VAL_RET,A.T_I_DEV,A.T_P_DEV,A.LIN_N01,A.ATIENDE,A.USUARIO,'' ID_DOC,'' CLAVE
                                    FROM " .  $obj_con->BdServidor . ".IG0060 A
                                            INNER JOIN " .  $obj_con->BdServidor . ".MG0031 B ON A.COD_CLI=B.COD_CLI 
                                            INNER JOIN " .  $obj_con->BdServidor . ".MG0041 C ON A.COD_MOT=C.COD_MOT
                            WHERE A.TIP_DEV='".$this->tipDev."' AND A.FEC_DEV>='$fechaIni' AND A.IND_UPD='L' AND A.ENV_DOC='0' LIMIT $limitEnv";
                    
                    break;
                Case 2://Compras provisiones de pasivos por NUmero
                    
                    $sql = "SELECT A.TIP_DEV,A.NUM_DEV,A.FEC_DEV,A.COD_MOT,C.NOM_MOT,A.TIP_NOF,A.NUM_NOF,A.FEC_NOF,
                                    A.COD_CLI,B.CED_RUC,B.NOM_CLI,B.DIR_CLI,B.TEL_N01,B.CORRE_E,A.VAL_BRU,A.POR_DES,A.VAL_DES,
                                    A.BAS_IVA,A.BAS_IV0,A.POR_IVA,A.POR_IVA,A.VAL_IVA,A.VAL_NET,A.POR_RET,
                                    A.VAL_RET,A.T_I_DEV,A.T_P_DEV,A.LIN_N01,A.ATIENDE,A.USUARIO,'' ID_DOC,'' CLAVE
                                    FROM " .  $obj_con->BdServidor . ".IG0060 A
                                            INNER JOIN " .  $obj_con->BdServidor . ".MG0031 B ON A.COD_CLI=B.COD_CLI 
                                            INNER JOIN " .  $obj_con->BdServidor . ".MG0041 C ON A.COD_MOT=C.COD_MOT
                            WHERE A.TIP_DEV='".$this->tipDev."' AND A.IND_UPD='L' AND A.NUM_DEV='$NumPed'  ";
                    
                    break;
                default:
                    $sql = "";
            }
            
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

    private function buscarDetNC($tipDoc, $numDoc) {
        $obj_con = new cls_Base();
        $conCont = $obj_con->conexionServidor();
        $rawData = array();//IF(A.I_M_IVA=1,A.T_VENTA*0.12,0)        
        $sql = "SELECT A.COD_ART,A.NOM_ART,A.CAN_DEV,A.P_VENTA,A.T_VENTA,A.I_M_IVA,A.VAL_DES,0 VAL_IVA
                    FROM " . $obj_con->BdServidor . ".IG0061 A
                WHERE A.TIP_DEV='$tipDoc' AND A.NUM_DEV='$numDoc' AND A.IND_EST='L';";
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

    public function insertarDocumentosNC($op,$NumPed) {
        $obj_con = new cls_Base();
        $obj_var = new cls_Global();
        $con = $obj_con->conexionIntermedio();
        $objEmpData= new EMPRESA();
        /****VARIBLES DE SESION*******/
        $emp_id=cls_Global::$emp_id;
        $est_id=cls_Global::$est_id;
        $pemi_id=cls_Global::$pemi_id;
        try {
            $cabFact = $this->buscarNC($op,$NumPed);
            $empresaEnt=$objEmpData->buscarDataEmpresa($emp_id,$est_id,$pemi_id);//recuperar info deL Contribuyente

            $codDoc=$this->tipoDoc;//Documento Notas de Credito
            for ($i = 0; $i < sizeof($cabFact); $i++) {
                $ClaveAcceso=$this->InsertarCabNC($con,$obj_con,$cabFact, $empresaEnt,$codDoc, $i);
                $idCab = $con->insert_id;
                $detFact=$this->buscarDetNC($cabFact[$i]['TIP_DEV'],$cabFact[$i]['NUM_DEV']);
                $this->InsertarDetNC($con,$obj_con,$cabFact[$i]['POR_IVA'],$detFact,$idCab);
                $this->InsertarNcDatoAdicional($con,$obj_con,$i,$cabFact,$idCab);
                $cabFact[$i]['ID_DOC']=$idCab;//Actualiza el IDs Documento Autorizacon SRI
                $cabFact[$i]['CLAVE']=$ClaveAcceso;
            }
            $con->commit();
            $con->close();
            $this->actualizaErpCabNc($cabFact);
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
    
    private function InsertarCabNC($con,$obj_con, $objEnt, $objEmp, $codDoc, $i) {
        $valida = new VSValidador();
        $tip_iden = $valida->tipoIdent($objEnt[$i]['CED_RUC']);
        $Secuencial = $valida->ajusteNumDoc($objEnt[$i]['NUM_DEV'], 9);
        
        //$GuiaRemi = (strlen($objEnt[$i]['GUI_REM']) > 0) ? $objEmp['Establecimiento'] . '-' . $objEmp['PuntoEmision'] . '-' . $valida->ajusteNumDoc($objEnt[$i]['GUI_REM'], 9) : '';
        $ced_ruc = ($tip_iden == '07') ? '9999999999999' : $objEnt[$i]['CED_RUC'];
        /* Datos para Genera Clave */
        //$tip_doc,$fec_doc,$ruc,$ambiente,$serie,$numDoc,$tipoemision
        $objCla = new VSClaveAcceso();
        $serie = $objEmp['Establecimiento'] . $objEmp['PuntoEmision'];
        $fec_doc = date("Y-m-d", strtotime($objEnt[$i]['FEC_DEV']));
        $ClaveAcceso = $objCla->claveAcceso($codDoc, $fec_doc, $objEmp['Ruc'], $objEmp['Ambiente'], $serie, $Secuencial, $objEmp['TipoEmision']);
        /** ********************** */
        $nomCliente=str_replace("'","`",$objEnt[$i]['NOM_CLI']);// Error del ' en el Text se lo Reemplaza `
        //$nomCliente=$objEnt[$i]['NOM_CLI'];// Error del ' en el Text
        $TotalSinImpuesto=floatval($objEnt[$i]['BAS_IVA'])+floatval($objEnt[$i]['BAS_IV0']);//Cambio por Ajuste de Valores Byron Diferencias
        $Rise="";//Verificar cuando es RISE
        $CodDocMod="01";//Aplica a documentos de Facturas =>f4
        $NumDocMod = $objEmp['Establecimiento'] ."-" .$objEmp['PuntoEmision']."-".$valida->ajusteNumDoc($objEnt[$i]['NUM_NOF'], 9);
        $FecEmiDocMod=date("Y-m-d", strtotime($objEnt[$i]['FEC_NOF']));

        
        $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeNotaCredito
                    (Ambiente,TipoEmision,RazonSocial,NombreComercial,Ruc,ClaveAcceso,CodigoDocumento,Establecimiento,
                     PuntoEmision,Secuencial,DireccionMatriz,FechaEmision,DireccionEstablecimiento,ContribuyenteEspecial,
                     ObligadoContabilidad,TipoIdentificacionComprador,RazonSocialComprador,IdentificacionComprador,Rise,
                     CodDocModificado,NumDocModificado,FechaEmisionDocModificado,TotalSinImpuesto,ValorModificacion,MotivoModificacion,
                     Moneda,SecuencialERP,CodigoTransaccionERP,UsuarioCreador,Estado,FechaCarga) VALUES (
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
                            '$fec_doc', 
                            '" . $objEmp['DireccionMatriz'] . "', 
                            '" . $objEmp['ContribuyenteEspecial'] . "', 
                            '" . $objEmp['ObligadoContabilidad'] . "', 
                            '$tip_iden',          
                            '$nomCliente', 
                            '$ced_ruc','$Rise',
                            '$CodDocMod','$NumDocMod','$FecEmiDocMod', 
                            '" . $TotalSinImpuesto . "',
                            '" . $objEnt[$i]['VAL_NET'] . "',
                            '" . $objEnt[$i]['NOM_MOT'] . "', 
                            '" . $objEmp['Moneda'] . "', 
                            '$Secuencial', 
                            '" . $objEnt[$i]['TIP_DEV'] . "',
                            '" . $objEnt[$i]['ATIENDE'] . "',
                            '1',CURRENT_TIMESTAMP() )";

        //echo $sql;
        $command = $con->prepare($sql);
        $command->execute();
        //$command = $con->query($sql);
        return $ClaveAcceso;

    }

    private function InsertarDetNC($con,$obj_con,$por_iva ,$detFact, $idCab) {
        //$obj_var = new cls_Global();
        $valSinImp = 0;
        $val_iva12 = 0;
        $vet_iva12 = 0;
        $val_iva0 = 0;//Valor de Iva
        $vet_iva0 = 0;//Venta total con Iva
        $VAL_IVA=0;//Calculo de Iva Genrado
        //TIP_NOF,NUM_NOF,FEC_VTA,COD_ART,NOM_ART,CAN_DES,P_VENTA,T_VENTA,VAL_DES,I_M_IVA,VAL_IVA
        for ($i = 0; $i < sizeof($detFact); $i++) {
            $valSinImp = floatval($detFact[$i]['T_VENTA']) - floatval($detFact[$i]['VAL_DES']);
            if ($detFact[$i]['I_M_IVA'] == '1') {
                //$val_iva12 = $val_iva12 + floatval($detFact[$i]['VAL_IVA']);
                //MOdificacion por que iva no cuadra con los totales
                $VAL_IVA=(floatval($detFact[$i]['CAN_DEV'])*floatval($detFact[$i]['P_VENTA'])* (floatval($por_iva)/100));
                $val_iva12 = $val_iva12 + $VAL_IVA;
                $vet_iva12 = $vet_iva12 + $valSinImp;
            } else {
                $VAL_IVA=0;//Cuando el Valor del Iva es 0
                $val_iva0 = 0;
                $vet_iva0 = $vet_iva0 + $valSinImp;
            }
          
            $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeDetalleNotaCredito
                    (CodigoPrincipal,CodigoAuxiliar,Descripcion,Cantidad,PrecioUnitario,Descuento,PrecioTotalSinImpuesto,IdNotaCredito) VALUES (
                    '" . $detFact[$i]['COD_ART'] . "', 
                    '1',
                    '" . $detFact[$i]['NOM_ART'] . "', 
                    '" . $detFact[$i]['CAN_DEV'] . "',
                    '" . $detFact[$i]['P_VENTA'] . "',
                    '" . $detFact[$i]['VAL_DES'] . "',
                    '$valSinImp',
                    '$idCab')";

            
            $command = $con->prepare($sql);
            $command->execute();
            $idDet = $con->insert_id;
            //Inserta el IVA de cada Item devuelto
            if ($detFact[$i]['I_M_IVA'] == '1') {//Verifico si el ITEM tiene Impuesto
                //Segun Datos Sri
                $this->InsertarDetImpNC($con,$obj_con, $idDet, '2', $por_iva, $valSinImp, $VAL_IVA); //12%
            } else {//Caso Contrario no Genera Impuesto
                $this->InsertarDetImpNC($con,$obj_con, $idDet, '2', '0', $valSinImp, $VAL_IVA); //0%
            }
        }
        //Inserta el Total del Iva Acumulado en el detalle
        //Insertar Datos de Iva 0%
        If ($vet_iva0 > 0) {
            $this->InsertarNcImpuesto($con,$obj_con, $idCab, '2','0', $vet_iva0, $val_iva0);
        }
        //Inserta Datos de Iva 12
        If ($vet_iva12 > 0) {
            $this->InsertarNcImpuesto($con,$obj_con, $idCab, '2', $por_iva, $vet_iva12, $val_iva12);
        }
    }

    private function InsertarDetImpNC($con,$obj_con, $idDet, $codigo,  $Tarifa, $t_venta, $val_iva) {
        $CodigoPor=cls_Global::retornaTarifaDelIva($Tarifa);
        $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeDetalleNotaCreditoImpuesto 
                 (Codigo,CodigoPorcentaje,BaseImponible,Tarifa,Valor,IdDetalleNotaCredito)VALUES(
                 '$codigo','$CodigoPor','$t_venta','$Tarifa','$val_iva','$idDet')";
        $command = $con->prepare($sql);
        $command->execute();
        //$command = $con->query($sql);
    }

    private function InsertarNcImpuesto($con,$obj_con, $idCab, $codigo, $Tarifa, $t_venta, $val_iva) {
        $CodigoPor=cls_Global::retornaTarifaDelIva($Tarifa);
        $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeNotaCreditoImpuesto 
                 (Codigo,CodigoPorcentaje,BaseImponible,Tarifa,Valor,IdNotaCredito)VALUES(
                 '$codigo','$CodigoPor','$t_venta','$Tarifa','$val_iva','$idCab')";
        $command = $con->prepare($sql);
        $command->execute();
        //$command = $con->query($sql);
    }

    private function InsertarNcDatoAdicional($con,$obj_con, $i, $cabFact, $idCab) {
        $direccion = $cabFact[$i]['DIR_CLI'];
        $telefono = $cabFact[$i]['TEL_N01'];
        $correo = $cabFact[$i]['CORRE_E'];
        $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeDatoAdicionalNotaCredito 
                 (Nombre,Descripcion,IdNotaCredito)
                 VALUES
                 ('Direccion','$direccion','$idCab'),('Telefono','$telefono','$idCab'),('Correo','$correo','$idCab')";
        $command = $con->prepare($sql);
        $command->execute();
        //$command = $con->query($sql);
    }
    
    private function actualizaErpCabNc($cabFact) {
        $obj_con = new cls_Base();
        $conCont = $obj_con->conexionServidor();
        try {
            for ($i = 0; $i < sizeof($cabFact); $i++) {
                $numero = $cabFact[$i]['NUM_DEV'];
                $tipo = $cabFact[$i]['TIP_DEV'];
                $clave = $cabFact[$i]['CLAVE'];
                $ids=$cabFact[$i]['ID_DOC'];//Contine el IDs del Tabla Autorizacion
                $sql = "UPDATE " . $obj_con->BdServidor . ".IG0060 SET ENV_DOC='$ids',ClaveAcceso='$clave'
                        WHERE TIP_DEV='$tipo' AND NUM_DEV='$numero' AND IND_UPD='L'";
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
    
    
    /************************************************************/
    /*********CONFIGURACION PARA DATOS DE USUARIOS
    /************************************************************/
    
    private function buscarFacturasRAD() {
        try {
            $obj_con = new cls_Base();
            $obj_var = new cls_Global();
            $conCont = $obj_con->conexionIntermedio();
            $rawData = array();
            $fechaIni=$obj_var->dateStartFact;
            $limitEnv=$obj_var->limitEnv;
            
            $sql = "SELECT IdFactura,AutorizacionSRI,FechaAutorizacion,Ambiente,TipoEmision,RazonSocial,NombreComercial,
                    Ruc,ClaveAcceso,CodigoDocumento,Establecimiento,PuntoEmision,Secuencial,DireccionMatriz,
                    FechaEmision,DireccionEstablecimiento,ContribuyenteEspecial,ObligadoContabilidad,TipoIdentificacionComprador,
                    GuiaRemision,RazonSocialComprador,IdentificacionComprador,TotalSinImpuesto,TotalDescuento,Propina,
                    ImporteTotal,Moneda,'1','',Estado
                        FROM " . $obj_con->BdIntermedio . ".NubeFactura 
                    WHERE Estado=2 AND IdRad='0' AND FechaCarga>'$fechaIni' limit $limitEnv ";
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
    
    private function buscarDetFacturasRAD($Ids) {
        $obj_con = new cls_Base();
        $conCont = $obj_con->conexionIntermedio();
        $rawData = array();
        $sql = "SELECT * FROM " . $obj_con->BdIntermedio . ".NubeDetalleFactura WHERE IdFactura='$Ids' ";
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
    
    public function insertarFacturasRAD() {
        
        $obj_con = new cls_Base();
        $obj_var = new cls_Global();
        $con = $obj_con->conexionVsRAd();
        try {
            $cabFact = $this->buscarFacturasRAD();
            for ($i = 0; $i < sizeof($cabFact); $i++) {
                $this->InsertarCabFacturaRAD($con,$obj_con,$cabFact, $i);
                $idCab = $con->insert_id;
                $detFact=$this->buscarDetFacturasRAD($cabFact[$i]['IdFactura']);
                $this->InsertarDetFacturaRAD($con,$obj_con,$detFact,$idCab);
                $this->InsertarFacturaImpuestoRAD($con,$obj_con, $idCab,$cabFact[$i]['IdFactura']);
                $this->InsertarFacturaDatoAdicionalRAD($con,$obj_con,$idCab,$cabFact[$i]['IdFactura']);
                $cabFact[$i]['ID_DOC']=$idCab;//Actualiza el IDs Documento Autorizacon SRI
            }
            $con->commit();
            $con->close();
            $this->actualizaRAD($cabFact);
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
    
    private function InsertarCabFacturaRAD($con,$obj_con, $objEnt, $i) {
        $Iva=0;
        $Ice=0;
        $IdUsuarioCreador=1;//Numero de Cedula del CLiente
        $ArchivoXml='';
        $sql = "INSERT INTO " . $obj_con->BdRad . ".VSFactura
                    (AutorizacionSRI,FechaAutorizacion,Ambiente,TipoEmision,RazonSocial,NombreComercial,
                    Ruc,ClaveAcceso,CodDoc,Estab,PtoEmi,Secuencial,DirMatriz,FechaEmision,DirEstablecimiento,
                    ContribuyenteEspecial,ObligadoContabilidad,TipoIdentificacionComprador,GuiaRemision,RazonSocialComprador,
                    IdentificacionComprador,TotalSinImpuesto,TotalDescuento,Propina,Iva,Ice,ImporteTotal,Moneda,IdUsuarioCreador,
                    FechaCreacion,ArchivoXml,Estado)VALUES(
                    '" . $objEnt[$i]['AutorizacionSRI'] . "',
                    '" . $objEnt[$i]['FechaAutorizacion'] . "',
                    '" . $objEnt[$i]['Ambiente'] . "',
                    '" . $objEnt[$i]['TipoEmision'] . "',
                    '" . $objEnt[$i]['RazonSocial'] . "',
                    '" . $objEnt[$i]['NombreComercial'] . "',
                    '" . $objEnt[$i]['Ruc'] . "',
                    '" . $objEnt[$i]['ClaveAcceso'] . "',
                    '" . $objEnt[$i]['CodigoDocumento'] . "',
                    '" . $objEnt[$i]['Establecimiento'] . "',
                    '" . $objEnt[$i]['PuntoEmision'] . "',
                    '" . $objEnt[$i]['Secuencial'] . "',
                    '" . $objEnt[$i]['DireccionMatriz'] . "',
                    '" . $objEnt[$i]['FechaEmision'] . "',
                    '" . $objEnt[$i]['DireccionEstablecimiento'] . "',
                    '" . $objEnt[$i]['ContribuyenteEspecial'] . "',
                    '" . $objEnt[$i]['ObligadoContabilidad'] . "',
                    '" . $objEnt[$i]['TipoIdentificacionComprador'] . "',
                    '" . $objEnt[$i]['GuiaRemision'] . "',
                    '" . $objEnt[$i]['RazonSocialComprador'] . "',
                    '" . $objEnt[$i]['IdentificacionComprador'] . "',
                    '" . $objEnt[$i]['TotalSinImpuesto'] . "',
                    '" . $objEnt[$i]['TotalDescuento'] . "',
                    '" . $objEnt[$i]['Propina'] . "',
                    '" .$Iva. "','" .$Ice. "',
                    '" . $objEnt[$i]['ImporteTotal'] . "',
                    '" . $objEnt[$i]['Moneda'] . "',
                    '" . $IdUsuarioCreador . "',
                    CURRENT_TIMESTAMP(),
                    '" . $ArchivoXml . "',
                    '" . $objEnt[$i]['Estado'] . "');";        
        $command = $con->prepare($sql);
        $command->execute();
    }
    
    private function InsertarDetFacturaRAD($con,$obj_con, $detFact, $idCab) {    
        for ($i = 0; $i < sizeof($detFact); $i++) {
            $sql = "INSERT INTO " . $obj_con->BdRad . ".VSDetalleFactura
                        (CodigoPrincipal,CodigoAuxiliar,Descripcion,Cantidad,Descuento,
                        PrecioUnitario,PrecioTotalSinImpuesto,IdFactura)VALUES(
                        '" . $detFact[$i]['CodigoPrincipal'] . "',
                        '" . $detFact[$i]['CodigoAuxiliar'] . "',
                        '" . $detFact[$i]['Descripcion'] . "',
                        '" . $detFact[$i]['Cantidad'] . "',
                        '" . $detFact[$i]['Descuento'] . "',
                        '" . $detFact[$i]['PrecioUnitario'] . "',
                        '" . $detFact[$i]['PrecioTotalSinImpuesto'] . "',
                        '$idCab')";

            $command = $con->prepare($sql);
            $command->execute();
            $idDet = $con->insert_id;
            $this->InsertarDetImpFacturaRAD($con,$obj_con, $idDet,$detFact[$i]['IdDetalleFactura']); 
        }
        
    }
    
    private function InsertarDetImpFacturaRAD($con,$obj_con, $idDet,$Ids) {        
        $sql = "INSERT INTO " . $obj_con->BdRad . ".VSDetalleFacturaImpuesto
                    (Codigo,CodigoPorcentaje,BaseImponible,Tarifa,Valor,IdDetalleFactura)
                SELECT Codigo,CodigoPorcentaje,BaseImponible,Tarifa,Valor,$idDet
                    FROM " . $obj_con->BdIntermedio . ".NubeDetalleFacturaImpuesto
                WHERE IdDetalleFactura='$Ids'";
        $command = $con->prepare($sql);
        $command->execute();
    }
    
    private function InsertarFacturaImpuestoRAD($con,$obj_con, $idCab,$Ids) {        
        $sql = "INSERT INTO " . $obj_con->BdRad . ".VSFacturaImpuesto
                    (Codigo,CodigoPorcentaje,BaseImponible,Tarifa,Valor,IdFactura)
                    SELECT Codigo,CodigoPorcentaje,BaseImponible,Tarifa,Valor,'$idCab'
                            FROM " . $obj_con->BdIntermedio . ".NubeFacturaImpuesto
                    WHERE IdFactura='$Ids' ";
        
        $command = $con->prepare($sql);
        $command->execute();
    }
    
    private function InsertarFacturaDatoAdicionalRAD($con,$obj_con,$idCab,$Ids) {
        $sql = "INSERT INTO " . $obj_con->BdRad . ".VSFacturaAdicionales
                    (Nombre,Descripcion,IdFactura)
                SELECT Nombre,Descripcion,'$idCab' FROM " . $obj_con->BdIntermedio . ".NubeDatoAdicionalFactura
                WHERE IdFactura='$Ids' "; 
        $command = $con->prepare($sql);
        $command->execute();
    }
    
    private function actualizaRAD($cabFact) {
        $obj_con = new cls_Base();
        $conCont = $obj_con->conexionIntermedio();
        try {
            for ($i = 0; $i < sizeof($cabFact); $i++) {
                $ids=$cabFact[$i]['ID_DOC'];//Contine el IDs del Tabla Autorizacion
                $IdFactura=$cabFact[$i]['IdFactura'];
                $sql = "UPDATE " . $obj_con->BdIntermedio . ".NubeFactura SET IdRad='$ids' WHERE IdFactura='$IdFactura';";
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
        
        $dataMail->file_to_attachXML=$obj_var->rutaXML.'NC/';//Rutas FACTURAS
        $dataMail->file_to_attachPDF=$obj_var->rutaPDF;//Ructa de Documentos PDF
        try {
            $cabDoc = $this->buscarMailNcRAD($con,$obj_var,$obj_con);//Consulta Documentos para Enviar
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
                //if(1>0){   
                    $mPDF1=$rep->crearBaseReport();
                    //Envia Correo                   
                    include('mensaje.php');
                    $htmlMail=$mensaje;

                    $dataMail->Subject='Ha Recibido un(a) Documento Nuevo(a)!!! ';
                    $dataMail->fileXML='NOTA DE CREDITO-'.$cabDoc[$i]["NumDocumento"].'.xml';
                    $dataMail->filePDF='NOTA DE CREDITO-'.$cabDoc[$i]["NumDocumento"].'.pdf';
                    //CREAR PDF
                    $mPDF1->SetTitle($dataMail->filePDF);
                    $cabFact = $this->mostrarCabNc($con,$obj_con,$cabDoc[$i]["Ids"]);
                    $detDoc = $this->mostrarDetNc($con,$obj_con,$cabDoc[$i]["Ids"]);
                    $impDoc = $this->mostrarNcImp($con,$obj_con,$cabDoc[$i]["Ids"]);
                    $adiDoc = $this->mostrarNcDataAdicional($con,$obj_con,$cabDoc[$i]["Ids"]);
                    $usuData=$objEmpData->buscarDatoVendedor($cabFact[0]["USU_ID"]);//Correo del Usuario que Autoriza
                    include('formatNc/ncPDF.php');
                    
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
            $obj_var->actualizaEnvioMailRAD($cabDoc,"NC");
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
    
    private function buscarMailNcRAD($con,$obj_var,$obj_con) {
            $rawData = array();
            $fechaIni=$obj_var->dateStartFact;
            $limitEnvMail=$obj_var->limitEnvMail;            
            $sql = "SELECT A.IdNotaCredito Ids,A.AutorizacionSRI,A.FechaAutorizacion,A.IdentificacionComprador CedRuc,A.RazonSocialComprador RazonSoc,
                    'NOTA DE CREDITO' NombreDocumento,A.Ruc,A.Ambiente,A.TipoEmision,A.EstadoEnv,
                    A.ClaveAcceso,CONCAT(A.Establecimiento,'-',A.PuntoEmision,'-',A.Secuencial) NumDocumento
                FROM " . $obj_con->BdIntermedio . ".NubeNotaCredito A "
                    . " WHERE A.Estado=3 "
                    . " AND A.EstadoEnv=2 AND A.FechaAutorizacion>='$fechaIni' limit $limitEnvMail ";             
            $sentencia = $con->query($sql);
            if ($sentencia->num_rows > 0) {
                while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                    $rawData[] = $fila;
                }
            }
            return $rawData;
       
    }
    
    public function mostrarCabNc($con,$obj_con,$id) {
        $rawData = array();        
        $sql = "SELECT A.IdNotaCredito IdDoc,A.Estado,A.CodigoTransaccionERP,A.SecuencialERP,A.UsuarioCreador,
                        A.FechaAutorizacion,A.RazonSocial,A.NombreComercial,A.AutorizacionSRI,A.DireccionMatriz,A.DireccionEstablecimiento,
                        CONCAT(A.Establecimiento,'-',A.PuntoEmision,'-',A.Secuencial) NumDocumento,
                        A.ContribuyenteEspecial,A.ObligadoContabilidad,A.TipoIdentificacionComprador,
                        A.CodigoDocumento,A.Establecimiento,A.PuntoEmision,A.Secuencial,
                        A.FechaEmision,A.IdentificacionComprador,A.RazonSocialComprador,
                        A.CodDocModificado,A.NumDocModificado,A.FechaEmisionDocModificado,
                        A.TotalSinImpuesto,A.ValorModificacion,A.MotivoModificacion,A.USU_ID,
                        'NOTA DE CREDITO' NombreDocumento,A.AutorizacionSri,A.ClaveAcceso,A.FechaAutorizacion,
                        A.Ambiente,A.TipoEmision,A.Moneda,A.Ruc,A.CodigoError
                        FROM " . $obj_con->BdIntermedio . ".NubeNotaCredito A
                WHERE A.CodigoDocumento='$this->tipoDoc' AND A.IdNotaCredito =$id ";
        //echo $sql;
        $sentencia = $con->query($sql);
        if ($sentencia->num_rows > 0) {
            while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
        }
        return $rawData;
    }
    
    public function mostrarDetNc($con,$obj_con,$id) {
        $rawData = array();
        $sql = "SELECT * FROM " . $obj_con->BdIntermedio . ".NubeDetalleNotaCredito WHERE IdNotaCredito=$id";
        $sentencia = $con->query($sql);
        if ($sentencia->num_rows > 0) {
            while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
        }
        for ($i = 0; $i < sizeof($rawData); $i++) {
            $rawData[$i]['impuestos'] = $this->mostrarDetNcImp($con,$obj_con,$rawData[$i]['IdDetalleNotaCredito']); //Retorna el Detalle del Impuesto
        }
        return $rawData;
    }

    private function mostrarDetNcImp($con,$obj_con,$id) {
        $rawData = array();
        $sql = "SELECT * FROM " . $obj_con->BdIntermedio . ".NubeDetalleNotaCreditoImpuesto WHERE IdDetalleNotaCredito=$id";
        $sentencia = $con->query($sql); 
        if ($sentencia->num_rows > 0) {
             while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
        }
        return $rawData;
    }

    public function mostrarNcImp($con,$obj_con,$id) {
        $rawData = array();
        $sql = "SELECT * FROM " . $obj_con->BdIntermedio . ".NubeNotaCreditoImpuesto WHERE IdNotaCredito=$id";
        $sentencia = $con->query($sql); 
        if ($sentencia->num_rows > 0) {
             while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
        }
        return $rawData;
    }

    public function mostrarNcDataAdicional($con,$obj_con,$id) {
        $rawData = array();
        $sql = "SELECT * FROM " . $obj_con->BdIntermedio . ".NubeDatoAdicionalNotaCredito WHERE IdNotaCredito=$id";
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
    private function buscarDocNcAUT($con,$obj_var,$obj_con,$nEstado) {
        $rawData = array();
        $fechaIni=$obj_var->dateStartFact;
        $limitEnvAUT=  cls_Global::$limitEnvAUT; 
        $sql = "SELECT A.IdNotaCredito Ids,A.UsuarioCreador UsuCre,A.ClaveAcceso,A.NombreDocumento
            FROM " . $obj_con->BdIntermedio . ".NubeNotaCredito A WHERE A.Estado IN($nEstado) "
                . "AND A.EstadoEnv=2 AND A.FechaCarga>='$fechaIni' limit $limitEnvAUT "; 
                //. "AND IdNotaCredito=241 ";
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
            $docAut= $this->buscarDocNcAUT($con, $obj_var, $obj_con,$nEstado); 
            //cls_Global::putMessageLogFile($docAut);            
            for ($i = 0; $i < count($docAut); $i++) {
                //cls_Global::putMessageLogFile($docAut[$i]); 
                $ids=$docAut[$i]["Ids"];
                if ($ids !== "") {
                    //Retorna Resultado Generado
                    $result = $this->generarFileXML($con,$obj_con,$ids,'NubeNotaCredito','IdNotaCredito');
                    $DirDocAutorizado=  cls_Global::$seaDocAutNc; 
                    $DirDocFirmado=cls_Global::$seaDocNc;
                    if ($result['status'] == 'OK') {//Retorna True o False 
                        //return $autDoc->AutorizaDocumento($result,$ids,$DirDocAutorizado,$DirDocFirmado,'NubeFactura','FACTURA','IdFactura');
                        $autDoc->AutorizaDocumento($result,$ids,$DirDocAutorizado,$DirDocFirmado,'NubeNotaCredito','NOTA_CREDITO','IdNotaCredito');
                    }elseif ($result['status'] == 'OK_REG') {
                        //LA CLAVE DE ACCESO REGISTRADA ingresa directamente a Obtener su autorizacion
                        //Autorizacion de Comprobantes 
                        //return $autDoc->autorizaComprobante($result, $ids, $DirDocAutorizado, $DirDocFirmado, 'NubeFactura','FACTURA','IdFactura');
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
            $docAut= $this->buscarDocNcAUT($con, $obj_var, $obj_con,$nEstado); 
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
                    $DirDocAutorizado=cls_Global::$seaDocAutNc; 
                    $DirDocFirmado=cls_Global::$seaDocNc;
                    $autDoc->autorizaComprobante($result, $ids, $DirDocAutorizado, $DirDocFirmado,'NubeNotaCredito','NOTA DE CREDITO','IdNotaCredito');
                
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
        //$xmlGen=new VSXmlGenerador();
        $codDoc = $this->tipoDoc; //Documento Factura
        $cabFact = $this->mostrarCabNc($con,$obj_con,$ids);
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
        
        $detFact = $this->mostrarDetNc($con,$obj_con,$ids);
        $impFact = $this->mostrarNcImp($con,$obj_con,$ids);
        $adiFact = $this->mostrarNcDataAdicional($con,$obj_con,$ids);
        //$pagFact = $this->mostrarFormaPago($con,$obj_con,$ids);//Agregar forma de pago

        
        //http://www.itsalif.info/content/php-5-domdocument-creating-basic-xml
        $xml = new DomDocument('1.0', 'UTF-8');
        //$xml->version='1.0';
        //$xml->encoding='UTF-8';
        $xml->standalone= TRUE;
        
        //NODO PRINCIPAL
        $dom = $xml->createElement('notaCredito');
        $dom->setAttribute('id', 'comprobante');
        $dom->setAttribute('version', '1.1.0');
        $dom = $xml->appendChild($dom);
        
        $dom->appendChild(EMPRESA::infoTributariaXML($cabFact,$xml));
        
            //INFORMACION DE NOTA CREDITO
            $infoNotaCredito=$xml->createElement('infoNotaCredito');
            $infoNotaCredito->appendChild($xml->createElement('fechaEmision', date(cls_Global::$dateXML, strtotime($cabFact[0]["FechaEmision"]))));
            $infoNotaCredito->appendChild($xml->createElement('dirEstablecimiento', utf8_encode(trim($cabFact[0]["DireccionEstablecimiento"]))));
            $infoNotaCredito->appendChild($xml->createElement('tipoIdentificacionComprador', $cabFact[0]["TipoIdentificacionComprador"]));
            $infoNotaCredito->appendChild($xml->createElement('razonSocialComprador', utf8_encode($valida->limpioCaracteresXML(trim($cabFact[0]["RazonSocialComprador"])))));
            $infoNotaCredito->appendChild($xml->createElement('identificacionComprador', $cabFact[0]["IdentificacionComprador"]));
            if(strlen(trim($cabFact[0]['ContribuyenteEspecial']))>0){                
                $infoNotaCredito->appendChild($xml->createElement('contribuyenteEspecial', $cabFact[0]["ContribuyenteEspecial"]));
            }
            if(strlen(trim($cabFact[0]['ObligadoContabilidad']))>0){                
                $infoNotaCredito->appendChild($xml->createElement('obligadoContabilidad', $cabFact[0]["ObligadoContabilidad"]));
            }            
            $infoNotaCredito->appendChild($xml->createElement('codDocModificado', $cabFact[0]["CodDocModificado"]));
            $infoNotaCredito->appendChild($xml->createElement('numDocModificado', $cabFact[0]["NumDocModificado"]));
            $infoNotaCredito->appendChild($xml->createElement('fechaEmisionDocSustento', date(cls_Global::$dateXML, strtotime($cabFact[0]["FechaEmisionDocModificado"]))));
            $infoNotaCredito->appendChild($xml->createElement('totalSinImpuestos', cls_Global::formatoDecXML($cabFact[0]["TotalSinImpuesto"])));
            $infoNotaCredito->appendChild($xml->createElement('valorModificacion', cls_Global::formatoDecXML($cabFact[0]["ValorModificacion"])));
            $infoNotaCredito->appendChild($xml->createElement('moneda', $cabFact[0]["Moneda"]));
            
            //$infoFactura->appendChild($xml->createElement('totalDescuento', cls_Global::formatoDecXML($cabFact[0]["ValorModificacion"])));
           
                $TConImpuestos=$xml->createElement('totalConImpuestos');
                $IRBPNR = 0; //NOta validar si existe casos para estos
                $ICE = 0;
                for ($i = 0; $i < sizeof($impFact); $i++) {
                    if ($impFact[$i]['Codigo'] == '2') {//Valores de IVA
                        switch ($impFact[$i]['CodigoPorcentaje']) {
                            case 0:
                                //$BASEIVA0=$impFact[$i]['BaseImponible'];
                                $TConImpuestos->appendChild($this->totalImpuestoXML($impFact,$i,$xml));
                                break;
                            case 2://IVA 12%
                                //$BASEIVA12 = $impFact[$i]['BaseImponible'];
                                //$VALORIVA12 = $impFact[$i]['Valor'];
                                $TConImpuestos->appendChild($this->totalImpuestoXML($impFact,$i,$xml));
                                break;
                            case 3://IVA 14%
                                //$BASEIVA12 = $impFact[$i]['BaseImponible'];
                                //$VALORIVA12 = $impFact[$i]['Valor'];
                                $TConImpuestos->appendChild($this->totalImpuestoXML($impFact,$i,$xml));
                                break;
                            case 6://No objeto Iva
                                //$NOOBJIVA=$impFact[$i]['BaseImponible'];
                                break;
                            case 7://Excento de Iva
                                //$EXENTOIVA=$impFact[$i]['BaseImponible'];
                                break;
                            default:
                        }
                    }
                    //NOta Verificar cuando el COdigo sea igual a 3 o 5 Para los demas impuestos
                    $infoNotaCredito->appendChild($TConImpuestos);
                } 
            $infoNotaCredito->appendChild($xml->createElement('motivo', $cabFact[0]["MotivoModificacion"]));
            //$infoFactura->appendChild($xml->createElement('propina', cls_Global::formatoDecXML($cabFact[0]["Propina"])));
            //$infoFactura->appendChild($xml->createElement('importeTotal', cls_Global::formatoDecXML($cabFact[0]["ImporteTotal"])));
 
        $dom->appendChild($infoNotaCredito);
        
            //DETALLE DE NOTA CREDITO
            $detalles=$xml->createElement('detalles');
            for ($i = 0; $i < sizeof($detFact); $i++) {//DETALLE DE FACTURAS
                $detalle=$xml->createElement('detalle');
                $detalle->appendChild($xml->createElement('codigoInterno', utf8_encode(trim($detFact[$i]['CodigoPrincipal']))));
                //$detalle->appendChild($xml->createElement('codigoAdicional', utf8_encode(trim($detFact[$i]['CodigoAuxiliar']))));
                $detalle->appendChild($xml->createElement('descripcion', $valida->limpioCaracteresXML(trim($detFact[$i]['Descripcion']))));
                $detalle->appendChild($xml->createElement('cantidad', cls_Global::formatoDecXML($detFact[$i]['Cantidad'])));
                $detalle->appendChild($xml->createElement('precioUnitario', (string)$detFact[$i]['PrecioUnitario']));
                $detalle->appendChild($xml->createElement('descuento', cls_Global::formatoDecXML($detFact[$i]['Descuento'])));
                $detalle->appendChild($xml->createElement('precioTotalSinImpuesto', cls_Global::formatoDecXML($detFact[$i]['PrecioTotalSinImpuesto'])));
                $detalle->appendChild($this->impuestosXML($detFact[$i]['impuestos'], $xml));

                $detalles->appendChild($detalle);

            }
        $dom->appendChild($detalles);
        
        //INFORMACION ADICIONAL
        $dom->appendChild(EMPRESA::infoAdicionalXML($adiFact, $xml));
        
        $xml->formatOutput = true;

        $nomDocfile = $cabFact[0]['NombreDocumento'] . '-' . $cabFact[0]['NumDocumento'] . '.xml';   
        $xml->save(cls_Global::$seaDocXml.$nomDocfile);
        
        return VSexception::messageFileXML('OK', $nomDocfile, $cabFact[0]["ClaveAcceso"], 2, null, null);
    }
    
    public function totalImpuestoXML($impFact,$i,$xml){
        $TImpuesto=$xml->createElement('totalImpuesto');
        $TImpuesto->appendChild($xml->createElement('codigo', $impFact[$i]["Codigo"]));
        $TImpuesto->appendChild($xml->createElement('codigoPorcentaje', $impFact[$i]["CodigoPorcentaje"]));
        $TImpuesto->appendChild($xml->createElement('baseImponible', cls_Global::formatoDecXML($impFact[$i]["BaseImponible"])));
        //$TImpuesto->appendChild($xml->createElement('tarifa', $impFact[$i]["Tarifa"]));
        $TImpuesto->appendChild($xml->createElement('valor', cls_Global::formatoDecXML($impFact[$i]["Valor"])));
        return $TImpuesto;
    }
    
    public function impuestosXML($impuesto,$xml){
        $impuestosG=$xml->createElement('impuestos');
        for ($j = 0; $j < sizeof($impuesto); $j++) {//DETALLE IMPUESTO DE FACTURA
            $imp=$xml->createElement('impuesto');
            $imp->appendChild($xml->createElement('codigo',$impuesto[$j]['Codigo']));
            $imp->appendChild($xml->createElement('codigoPorcentaje',$impuesto[$j]['CodigoPorcentaje']));
            $imp->appendChild($xml->createElement('tarifa',cls_Global::formatoDecXML($impuesto[$j]['Tarifa'])));
            $imp->appendChild($xml->createElement('baseImponible',cls_Global::formatoDecXML($impuesto[$j]['BaseImponible'])));
            $imp->appendChild($xml->createElement('valor',cls_Global::formatoDecXML($impuesto[$j]['Valor'])));
            $impuestosG->appendChild($imp);
        }
        return $impuestosG;
    }

    
    
    
    
    
    
}
