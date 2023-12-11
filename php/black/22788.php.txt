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
class NubeFactura {
    private $tipoDoc='01';//Tipo Doc SRI
    
    private function buscarFacturas() {
        try {
            $obj_con = new cls_Base();
            $obj_var = new cls_Global();
            $conCont = $obj_con->conexionServidor();
            $rawData = array();
            $fechaIni=$obj_var->dateStartFact;
            $limitEnv=$obj_var->limitEnv;
            //$sql = "SELECT TIP_NOF,CONCAT(REPEAT('0',9-LENGTH(RIGHT(NUM_NOF,9))),RIGHT(NUM_NOF,9)) NUM_NOF,
            $sql = "SELECT TIP_NOF, NUM_NOF,
                            CED_RUC,NOM_CLI,FEC_VTA,DIR_CLI,VAL_BRU,POR_DES,VAL_DES,VAL_FLE,BAS_IVA,
                            BAS_IV0,POR_IVA,VAL_IVA,VAL_NET,POR_R_F,VAL_R_F,POR_R_I,VAL_R_I,GUI_REM,0 PROPINA,
                            USUARIO,LUG_DES,NOM_CTO,ATIENDE,'' ID_DOC,ClaveAcceso CLAVE,FOR_PAG_SRI,PAG_PLZ,PAG_TMP
                        FROM " .  $obj_con->BdServidor . ".VC010101 WHERE IND_UPD='L' ";
            //$sql .=         "AND FEC_VTA>'$fechaIni' AND ENV_DOC='0' LIMIT $limitEnv";
            $sql .= " AND NUM_NOF='0000146377' AND TIP_NOF='F4' ";//Probar Factura
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

    private function buscarDetFacturas($tipDoc, $numDoc) {
        $obj_con = new cls_Base();
        $conCont = $obj_con->conexionServidor();
        $rawData = array();
        $sql = "SELECT TIP_NOF,NUM_NOF,FEC_VTA,COD_ART,NOM_ART,CAN_DES,P_VENTA,
                        T_VENTA,VAL_DES,I_M_IVA,VAL_IVA
                    FROM " . $obj_con->BdServidor . ".VD010101
                WHERE TIP_NOF='$tipDoc' AND NUM_NOF='$numDoc' AND IND_EST='L'";
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

    public function insertarFacturas() {
        
        $obj_con = new cls_Base();
        $obj_var = new cls_Global();
        $con = $obj_con->conexionIntermedio();
        $objEmpData= new EMPRESA();
        /****VARIBLES DE SESION*******/
        $emp_id=cls_Global::$emp_id;
        $est_id=cls_Global::$est_id;
        $pemi_id=cls_Global::$pemi_id;
        try {
            $cabFact = $this->buscarFacturas();
            $empresaEnt=$objEmpData->buscarDataEmpresa($emp_id,$est_id,$pemi_id);//recuperar info deL Contribuyente

            $codDoc=$this->tipoDoc;//Documento Factura
            for ($i = 0; $i < sizeof($cabFact); $i++) {
                $ClaveAcceso=$this->InsertarCabFactura($con,$obj_con,$cabFact, $empresaEnt,$codDoc, $i);
                $idCab = $con->insert_id;
                $detFact=$this->buscarDetFacturas($cabFact[$i]['TIP_NOF'],$cabFact[$i]['NUM_NOF']);
                $this->InsertarDetFactura($con,$obj_con,$cabFact[$i]['POR_IVA'],$detFact,$idCab);
                $this->InsertarFacturaFormaPago($con, $obj_con, $i, $cabFact, $idCab);//Inserta Forma de Pago 8 SEP 2016
                $this->InsertarFacturaDatoAdicional($con,$obj_con,$i,$cabFact,$idCab);
                $cabFact[$i]['ID_DOC']=$idCab;//Actualiza el IDs Documento Autorizacon SRI
                $cabFact[$i]['CLAVE']=$ClaveAcceso;
            }
            $con->commit();
            $con->close();
            //$this->actualizaErpCabFactura($cabFact);
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
    
    private function InsertarCabFactura($con,$obj_con, $objEnt, $objEmp, $codDoc, $i) {
        $valida = new VSValidador();
        $tip_iden = $valida->tipoIdent($objEnt[$i]['CED_RUC']);
        $Secuencial = $valida->ajusteNumDoc($objEnt[$i]['NUM_NOF'], 9);
        //$GuiaRemi=$valida->ajusteNumDoc($objEnt[$i]['GUI_REM'],9);
        $GuiaRemi = (strlen($objEnt[$i]['GUI_REM']) > 0) ? $objEmp['Establecimiento'] . '-' . $objEmp['PuntoEmision'] . '-' . $valida->ajusteNumDoc($objEnt[$i]['GUI_REM'], 9) : '';
        $ced_ruc = ($tip_iden == '07') ? '9999999999999' : $objEnt[$i]['CED_RUC'];
        /* Datos para Genera Clave */
        //$tip_doc,$fec_doc,$ruc,$ambiente,$serie,$numDoc,$tipoemision
        $objCla = new VSClaveAcceso();
        $serie = $objEmp['Establecimiento'] . $objEmp['PuntoEmision'];
        $fec_doc = date("Y-m-d", strtotime($objEnt[$i]['FEC_VTA']));
        
        $ClaveAcceso =$objEnt[$i]['ClaveAcceso']; //$objCla->claveAcceso($codDoc, $fec_doc, $objEmp['Ruc'], $objEmp['Ambiente'], $serie, $Secuencial, $objEmp['TipoEmision']);
        /** ********************** */
        $nomCliente=str_replace("'","`",$objEnt[$i]['NOM_CLI']);// Error del ' en el Text se lo Reemplaza `
        //$nomCliente=$objEnt[$i]['NOM_CLI'];// Error del ' en el Text
        $TotalSinImpuesto=floatval($objEnt[$i]['BAS_IVA'])+floatval($objEnt[$i]['BAS_IV0']);//Cambio por Ajuste de Valores Byron Diferencias
        $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeFactura
                            (Ambiente,TipoEmision, RazonSocial, NombreComercial, Ruc,ClaveAcceso,CodigoDocumento, Establecimiento,
                            PuntoEmision, Secuencial, DireccionMatriz, FechaEmision, DireccionEstablecimiento, ContribuyenteEspecial,
                            ObligadoContabilidad, TipoIdentificacionComprador, GuiaRemision, RazonSocialComprador, IdentificacionComprador,
                            TotalSinImpuesto, TotalDescuento, Propina, ImporteTotal, Moneda, SecuencialERP, CodigoTransaccionERP,UsuarioCreador,Estado,FechaCarga) VALUES (
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
                            '$GuiaRemi',               
                            '$nomCliente', 
                            '$ced_ruc', 
                            '" . $TotalSinImpuesto . "', 
                            '" . $objEnt[$i]['VAL_DES'] . "', 
                            '" . $objEnt[$i]['PROPINA'] . "', 
                            '" . $objEnt[$i]['VAL_NET'] . "', 
                            '" . $objEmp['Moneda'] . "', 
                            '$Secuencial', 
                            '" . $objEnt[$i]['TIP_NOF'] . "',
                            '" . $objEnt[$i]['ATIENDE'] . "',
                            '1',CURRENT_TIMESTAMP() )";

        $command = $con->prepare($sql);
        $command->execute();
        return $ClaveAcceso;
    }

    private function InsertarDetFactura($con,$obj_con,$por_iva, $detFact, $idCab) {
        $valSinImp = 0;
        $val_iva12 = 0;
        $vet_iva12 = 0;
        $val_iva0 = 0;//Valor de Iva
        $vet_iva0 = 0;//Venta total con Iva
        //TIP_NOF,NUM_NOF,FEC_VTA,COD_ART,NOM_ART,CAN_DES,P_VENTA,T_VENTA,VAL_DES,I_M_IVA,VAL_IVA
        for ($i = 0; $i < sizeof($detFact); $i++) {
            $valSinImp = floatval($detFact[$i]['T_VENTA']) - floatval($detFact[$i]['VAL_DES']);
            if ($detFact[$i]['I_M_IVA'] == '1') {
                //$val_iva12 = $val_iva12 + floatval($detFact[$i]['VAL_IVA']);
                //MOdificacion por que iva no cuadra con los totales
                //$val_iva12 = $val_iva12 + (floatval($detFact[$i]['CAN_DES'])*floatval($detFact[$i]['P_VENTA'])* (floatval($por_iva)/100));
                $val_iva12 = $val_iva12 + ((floatval($detFact[$i]['CAN_DES'])*floatval($detFact[$i]['P_VENTA'])-floatval($detFact[$i]['VAL_DES']))* (floatval($por_iva)/100));
                $vet_iva12 = $vet_iva12 + $valSinImp;
            } else {
                $val_iva0 = 0;
                $vet_iva0 = $vet_iva0 + $valSinImp;
            }

            $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeDetalleFactura 
                    (CodigoPrincipal,CodigoAuxiliar,Descripcion,Cantidad,PrecioUnitario,Descuento,PrecioTotalSinImpuesto,IdFactura) VALUES (
                    '" . $detFact[$i]['COD_ART'] . "', 
                    '1',
                    '" . $detFact[$i]['NOM_ART'] . "', 
                    '" . $detFact[$i]['CAN_DES'] . "',
                    '" . $detFact[$i]['P_VENTA'] . "',
                    '" . $detFact[$i]['VAL_DES'] . "',
                    '$valSinImp',
                    '$idCab')";
            $command = $con->prepare($sql);
            $command->execute();
            $idDet = $con->insert_id;
            //Inserta el IVA de cada Item 
            if ($detFact[$i]['I_M_IVA'] == '1') {//Verifico si el ITEM tiene Impuesto
                //Segun Datos Sri
                $this->InsertarDetImpFactura($con,$obj_con, $idDet, '2',$por_iva, $valSinImp, $detFact[$i]['VAL_IVA']); //12%
            } else {//Caso Contrario no Genera Impuesto
                $this->InsertarDetImpFactura($con,$obj_con, $idDet, '2','0', $valSinImp, $detFact[$i]['VAL_IVA']); //0%
            }
        }
        //Inserta el Total del Iva Acumulado en el detalle
        //Insertar Datos de Iva 0%
        If ($vet_iva0 > 0) {
            $this->InsertarFacturaImpuesto($con,$obj_con, $idCab, '2','0', $vet_iva0, $val_iva0);
        }
        //Inserta Datos de Iva 12
        If ($vet_iva12 > 0) {
            $this->InsertarFacturaImpuesto($con,$obj_con, $idCab, '2', $por_iva, $vet_iva12, $val_iva12);
        }
    }

    private function InsertarDetImpFactura($con,$obj_con, $idDet, $codigo, $Tarifa, $t_venta, $val_iva) {
        $CodigoPor=cls_Global::retornaTarifaDelIva($Tarifa);
        $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeDetalleFacturaImpuesto 
                 (Codigo,CodigoPorcentaje,BaseImponible,Tarifa,Valor,IdDetalleFactura)VALUES(
                 '$codigo','$CodigoPor','$t_venta','$Tarifa','$val_iva','$idDet')";
        $command = $con->prepare($sql);
        $command->execute();
        //$command = $con->query($sql);
    }

    private function InsertarFacturaImpuesto($con,$obj_con, $idCab, $codigo, $Tarifa, $t_venta, $val_iva) {
        $CodigoPor=cls_Global::retornaTarifaDelIva($Tarifa);
        $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeFacturaImpuesto 
                 (Codigo,CodigoPorcentaje,BaseImponible,Tarifa,Valor,IdFactura)VALUES(
                 '$codigo','$CodigoPor','$t_venta','$Tarifa','$val_iva','$idCab')";
        $command = $con->prepare($sql);
        $command->execute();
        //$command = $con->query($sql);
    }
    
    private function InsertarFacturaFormaPago($con,$obj_con, $i, $cabFact, $idCab) {
        //Implementado 8/08/2016
        //FOR_PAG_SRI,PAG_PLZ,PAG_TMP,VAL_NET
        //Nota la Tabla Forma de Pago debe ser aigual que la SEA Y WEBSEA los IDS deben conincidir.
        //Si no tiene codigo usa el codigo 1 (SIN UTILIZACION DEL SISTEMA FINANCIERO o Efectivo)
        $IdsForma = ($cabFact[$i]['FOR_PAG_SRI']!='')?$cabFact[$i]['FOR_PAG_SRI']:'1';
        $Total=($cabFact[$i]['VAL_NET']!='')?$cabFact[$i]['VAL_NET']:0;
        $Plazo=($cabFact[$i]['PAG_PLZ']>0)?$cabFact[$i]['PAG_PLZ']:'30';
        $UnidadTiempo=($cabFact[$i]['PAG_TMP']!='')?$cabFact[$i]['PAG_TMP']:'DIAS';
        $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeFacturaFormaPago
                (IdForma,IdFactura,FormaPago,Total,Plazo,UnidadTiempo)VALUES(
                '$IdsForma','$idCab','$IdsForma',$Total,'$Plazo','$UnidadTiempo');";
        $command = $con->prepare($sql);
        $command->execute();
    }

    private function InsertarFacturaDatoAdicional($con,$obj_con, $i, $cabFact, $idCab) {
        $direccion = $cabFact[$i]['DIR_CLI'];
        $destino = $cabFact[$i]['LUG_DES'];
        $contacto = $cabFact[$i]['NOM_CTO'];
        $sql = "INSERT INTO " . $obj_con->BdIntermedio . ".NubeDatoAdicionalFactura 
                 (Nombre,Descripcion,IdFactura)
                 VALUES
                 ('Direccion','$direccion','$idCab'),('Destino','$destino','$idCab'),('Contacto','$contacto','$idCab')";
        $command = $con->prepare($sql);
        $command->execute();
        //$command = $con->query($sql);
    }
    
    private function actualizaErpCabFactura($cabFact) {
        $obj_con = new cls_Base();
        $conCont = $obj_con->conexionServidor();
        try {
            for ($i = 0; $i < sizeof($cabFact); $i++) {
                $numero = $cabFact[$i]['NUM_NOF'];
                $tipo = $cabFact[$i]['TIP_NOF'];
                $clave = $cabFact[$i]['CLAVE'];
                $ids=$cabFact[$i]['ID_DOC'];//Contine el IDs del Tabla Autorizacion
                $sql = "UPDATE " . $obj_con->BdServidor . ".VC010101 SET ENV_DOC='$ids',ClaveAcceso='$clave'
                        WHERE TIP_NOF='$tipo' AND NUM_NOF='$numero' AND IND_UPD='L'";
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
        $ArchivoXml=$objEnt[$i]['NombreDocumento'];
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
    /*********FUNCIONES IGUALES A LAS APLICACION WEB PARA PDF
    /************************************************************/
    
    private function mostrarCabFactura($con,$obj_con,$id) {
        $rawData = array();
        $sql = "SELECT A.IdFactura IdDoc,A.Estado,A.EstadoEnv,A.CodigoTransaccionERP,A.SecuencialERP,A.UsuarioCreador,
                        A.FechaAutorizacion,A.RazonSocial,A.NombreComercial,A.AutorizacionSRI,A.DireccionMatriz,A.DireccionEstablecimiento,
                        CONCAT(A.Establecimiento,'-',A.PuntoEmision,'-',A.Secuencial) NumDocumento,
                        A.ContribuyenteEspecial,A.ObligadoContabilidad,A.TipoIdentificacionComprador,
                        A.CodigoDocumento,A.Establecimiento,A.PuntoEmision,A.Secuencial,
                        A.FechaEmision,A.IdentificacionComprador,A.RazonSocialComprador,
                        A.TotalSinImpuesto,A.TotalDescuento,A.Propina,A.ImporteTotal,A.USU_ID,
                        'FACTURA' NombreDocumento,A.ClaveAcceso,A.Ambiente,A.TipoEmision,A.GuiaRemision,A.Moneda,A.Ruc,A.CodigoError
                        FROM " . $obj_con->BdIntermedio . ".NubeFactura A
                WHERE A.CodigoDocumento='$this->tipoDoc' AND A.IdFactura =$id ";
            $sentencia = $con->query($sql);
            if ($sentencia->num_rows > 0) {
                while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                    $rawData[] = $fila;
                }
            }
            //$conCont->close();
            return $rawData;
    }
    private function mostrarDetFacturaImp($con,$obj_con,$id) {
        $rawData = array();
        $sql = "SELECT * FROM " . $obj_con->BdIntermedio . ".NubeDetalleFactura WHERE IdFactura=$id";
        $sentencia = $con->query($sql);
        if ($sentencia->num_rows > 0) {
            while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
        }
        for ($i = 0; $i < sizeof($rawData); $i++) {
            $rawData[$i]['impuestos'] = $this->mostrarDetalleImp($con,$obj_con,$rawData[$i]['IdDetalleFactura']); //Retorna el Detalle del Impuesto
        }
        return $rawData;
    }

    private function mostrarDetalleImp($con,$obj_con,$id) {
        $rawData = array();
        $sql = "SELECT * FROM " . $obj_con->BdIntermedio. ".NubeDetalleFacturaImpuesto WHERE IdDetalleFactura=$id";
        $sentencia = $con->query($sql);
        if ($sentencia->num_rows > 0) {
            while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
        }
        return $rawData;
    }
    
    private function mostrarFacturaImp($con,$obj_con,$id) {
        $rawData = array();
        $sql = "SELECT * FROM " . $obj_con->BdIntermedio . ".NubeFacturaImpuesto WHERE IdFactura=$id";
        $sentencia = $con->query($sql);
        if ($sentencia->num_rows > 0) {
            while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
        }
        return $rawData;
    }
    
    //Mostrar Formas de Pago Factura
    public function mostrarFormaPago($con,$obj_con,$id) {
        $rawData = array();
        $sql = "SELECT B.FormaPago,A.Total,A.Plazo,A.UnidadTiempo,A.FormaPago Codigo  
                FROM " . $obj_con->BdIntermedio . ".NubeFacturaFormaPago A
                        INNER JOIN " . $obj_con->BdIntermedio . ".VSFormaPago B
                                ON A.IdForma=B.IdForma
                    WHERE A.IdFactura=$id ";
        $sentencia = $con->query($sql);
        if ($sentencia->num_rows > 0) {
            while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
        }
        return $rawData;
    }
    
    private function mostrarFacturaDataAdicional($con,$obj_con,$id) {
        $rawData = array();
        $sql = "SELECT * FROM " . $obj_con->BdIntermedio . ".NubeDatoAdicionalFactura WHERE IdFactura=$id";
        $sentencia = $con->query($sql);
        if ($sentencia->num_rows > 0) {
            while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                $rawData[] = $fila;
            }
        }
        return $rawData;
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
     
        $dataMail->file_to_attachXML=$obj_var->rutaXML.'FACTURAS/';//Rutas FACTURAS
        $dataMail->file_to_attachPDF=$obj_var->rutaPDF;//Ructa de Documentos PDF
        try {
            $cabDoc = $this->buscarMailFacturasRAD($con,$obj_var,$obj_con);//Consulta Documentos para Enviar
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
            //return true;//Finaliza la accion por pruebas.
            //Envia l iformacion de Correos que ya se completo
            for ($i = 0; $i < sizeof($cabDoc); $i++) {
                if(strlen($cabDoc[$i]['CorreoPer'])>0){                
                    $mPDF1=$rep->crearBaseReport();
                    //Envia Correo                   
                    include('mensaje.php');
                    $htmlMail=$mensaje;
                    //$htmlMail=file_get_contents('mensaje.php');
                    $dataMail->Subject='Ha Recibido un(a) Factura Nuevo(a)!!! ';
                    $dataMail->fileXML='FACTURA-'.$cabDoc[$i]["NumDocumento"].'.xml';
                    $dataMail->filePDF='FACTURA-'.$cabDoc[$i]["NumDocumento"].'.pdf';
                    //CREAR PDF
                    $mPDF1->SetTitle($dataMail->filePDF);
                    $cabFact = $this->mostrarCabFactura($con,$obj_con,$cabDoc[$i]["Ids"]);
                    $detFact = $this->mostrarDetFacturaImp($con,$obj_con,$cabDoc[$i]["Ids"]);
                    $impFact = $this->mostrarFacturaImp($con,$obj_con,$cabDoc[$i]["Ids"]);
                    $pagFact = $this->mostrarFormaPago($con,$obj_con,$cabDoc[$i]["Ids"]);
                    $adiFact = $this->mostrarFacturaDataAdicional($con,$obj_con,$cabDoc[$i]["Ids"]);
                    
                    $usuData=$objEmpData->buscarDatoVendedor($cabFact[0]["USU_ID"]);//DATOS vendedor Correos
                    include('formatFact/facturaPDF.php');
                  
                    //COMETAR EN CASO DE NO PRESENTAR ESTA INFO
                    $mPDF1->SetWatermarkText('ESTA INFORMACIÓN ES UNA PRUEBA');
                    $mPDF1->watermark_font= 'DejaVuSansCondensed';
                    $mPDF1->watermarkTextAlpha = 0.5;
                    $mPDF1->showWatermarkText=($cabDoc[$i]["Ambiente"]==1)?TRUE:FALSE; // 1=Pruebas y 2=Produccion
                    //****************************************                    
                    $mPDF1->WriteHTML($mensajePDF); //hacemos un render partial a una vista preparada, en este caso es la vista docPDF
                    $mPDF1->Output($obj_var->rutaPDF.$dataMail->filePDF, 'F');//I=lo presenta navegador  F=ENVIA A UN ARCHVIO
                    
                    //$usuData=$objEmpData->buscarDatoVendedor($cabFact[0]["USU_ID"]);                    
                    
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
            $obj_var->actualizaEnvioMailRAD($cabDoc,"FA");
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
    
    private function buscarMailFacturasRAD($con,$obj_var,$obj_con) {
            $rawData = array();
            $fechaIni=$obj_var->dateStartFact;
            $limitEnvMail=$obj_var->limitEnvMail;
            $sql = "SELECT IdFactura Ids,AutorizacionSRI,FechaAutorizacion,IdentificacionComprador CedRuc,RazonSocialComprador RazonSoc,
                    'FACTURA' NombreDocumento,Ruc,Ambiente,TipoEmision,EstadoEnv,
                    ClaveAcceso,ImporteTotal Importe,CONCAT(Establecimiento,'-',PuntoEmision,'-',Secuencial) NumDocumento
                FROM " . $obj_con->BdIntermedio . ".NubeFactura WHERE Estado=3 "
                    . "AND EstadoEnv=2 AND FechaAutorizacion>='$fechaIni' limit $limitEnvMail "; 
                    //. "AND IdFactura=1654 ";  
            $sentencia = $con->query($sql);
            if ($sentencia->num_rows > 0) {
                while ($fila = $sentencia->fetch_assoc()) {//Array Asociativo
                    $rawData[] = $fila;
                }
            }
            //$conCont->close();
            return $rawData;
       
    }
    
    public function updateErpDocAutorizado($cabDoc) {
        $obj_con = new cls_Base();
        $conCont = $obj_con->conexionServidor();
        try {
            $cabDoc=cls_Global::buscarDocAutorizacion('FA');
            for ($i = 0; $i < sizeof($cabDoc); $i++) {
                $Estado=$cabDoc[$i]['EstadoEnv'];
                if($Estado=6){//Actualiza Solo los que fueron enviados Correctamente
                    $ClaveAcceso = $cabDoc[$i]['ClaveAcceso'];
                    $AutorizacionSri = $cabDoc[$i]['AutorizacionSRI'];                
                    $ids=$cabDoc[$i]['Ids'];//Contine el IDs del Tabla Autorizacion                
                    $sql = "UPDATE " . $obj_con->BdServidor . ".VC010101 SET ClaveAcceso='$ClaveAcceso',AutorizacionSri='$AutorizacionSri' 
                            WHERE ClaveAcceso IS NULL AND AutorizacionSri IS NULL AND ENV_DOC='$ids' AND IND_UPD='L'";
                    //echo $sql;
                    $command = $conCont->prepare($sql);
                    $command->execute();                    
                }                
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
    /*********CONSUMO DE WEB SERVICES
    /************************************************************/
    private function buscarDocFactAUT($con,$obj_var,$obj_con,$nEstado) {
        //Solo Envia los Documentos Recibidos
        $rawData = array();
        $fechaIni=$obj_var->dateStartFact;
        $limitEnvAUT=  cls_Global::$limitEnvAUT; 
        $sql = "SELECT A.IdFactura Ids,A.UsuarioCreador UsuCre,A.ClaveAcceso,A.NombreDocumento
            FROM " . $obj_con->BdIntermedio . ".NubeFactura A WHERE A.Estado IN($nEstado) "
                . "AND A.EstadoEnv=2 AND A.FechaCarga>='$fechaIni' limit $limitEnvAUT "; 
                //. "AND IdFactura=2584 ";
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
            //Envia Documentos Ingrsados y Clave en Proceso
            $nEstado="1,4";
            //$nEstado="4,5,6";//Descomentar Solo Pruebas
            $docAut= $this->buscarDocFactAUT($con, $obj_var, $obj_con,$nEstado); 
            //cls_Global::putMessageLogFile($docAut);            
            for ($i = 0; $i < count($docAut); $i++) {
                //cls_Global::putMessageLogFile($docAut[$i]); 
                $ids=$docAut[$i]["Ids"];
                if ($ids !== "") {
                    //Retorna Resultado Generado
                    $result = $this->generarFileXML($con,$obj_con,$ids,'NubeFactura','IdFactura');
                    $DirDocAutorizado=  cls_Global::$seaDocAutFact; 
                    $DirDocFirmado=cls_Global::$seaDocFact;
                    if ($result['status'] == 'OK') {//Retorna True o False 
                        //return $autDoc->AutorizaDocumento($result,$ids,$DirDocAutorizado,$DirDocFirmado,'NubeFactura','FACTURA','IdFactura');
                        $autDoc->AutorizaDocumento($result,$ids,$DirDocAutorizado,$DirDocFirmado,'NubeFactura','FACTURA','IdFactura');
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
            $docAut= $this->buscarDocFactAUT($con, $obj_var, $obj_con,$nEstado); 
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
                    $DirDocAutorizado=  cls_Global::$seaDocAutFact; 
                    $DirDocFirmado=cls_Global::$seaDocFact;
                    $autDoc->autorizaComprobante($result, $ids, $DirDocAutorizado, $DirDocFirmado, 'NubeFactura','FACTURA','IdFactura');
                
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
        $cabFact = $this->mostrarCabFactura($con,$obj_con,$ids);
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
        
        $detFact = $this->mostrarDetFacturaImp($con,$obj_con,$ids);
        $impFact = $this->mostrarFacturaImp($con,$obj_con,$ids);
        $adiFact = $this->mostrarFacturaDataAdicional($con,$obj_con,$ids);
        $pagFact = $this->mostrarFormaPago($con,$obj_con,$ids);//Agregar forma de pago

        
        //http://www.itsalif.info/content/php-5-domdocument-creating-basic-xml
        $xml = new DomDocument('1.0', 'UTF-8');
        //$xml->version='1.0';
        //$xml->encoding='UTF-8';
        $xml->standalone= TRUE;
        
        //NODO PRINCIPAL
        $dom = $xml->createElement('factura');
        $dom->setAttribute('id', 'comprobante');
        $dom->setAttribute('version', '1.1.0');
        $dom = $xml->appendChild($dom);
        
        $dom->appendChild(EMPRESA::infoTributariaXML($cabFact,$xml));
        
            //INFORMACION DE FACTURAS
            $infoFactura=$xml->createElement('infoFactura');
            $infoFactura->appendChild($xml->createElement('fechaEmision', date(cls_Global::$dateXML, strtotime($cabFact[0]["FechaEmision"]))));
            $infoFactura->appendChild($xml->createElement('dirEstablecimiento', utf8_encode(trim($cabFact[0]["DireccionEstablecimiento"]))));
            if(strlen(trim($cabFact[0]['ContribuyenteEspecial']))>0){                
                $infoFactura->appendChild($xml->createElement('contribuyenteEspecial', $cabFact[0]["ContribuyenteEspecial"]));
            }
            $infoFactura->appendChild($xml->createElement('obligadoContabilidad', $cabFact[0]["ObligadoContabilidad"]));
            $infoFactura->appendChild($xml->createElement('tipoIdentificacionComprador', $cabFact[0]["TipoIdentificacionComprador"]));
            $infoFactura->appendChild($xml->createElement('razonSocialComprador', utf8_encode($valida->limpioCaracteresXML(trim($cabFact[0]["RazonSocialComprador"])))));
            $infoFactura->appendChild($xml->createElement('identificacionComprador', $cabFact[0]["IdentificacionComprador"]));
            $infoFactura->appendChild($xml->createElement('totalSinImpuestos', cls_Global::formatoDecXML($cabFact[0]["TotalSinImpuesto"])));
            $infoFactura->appendChild($xml->createElement('totalDescuento', cls_Global::formatoDecXML($cabFact[0]["TotalDescuento"])));
           
                $TConImpuestos=$xml->createElement('totalConImpuestos');
                //$IRBPNR = 0; //NOta validar si existe casos para estos
                //$ICE = 0;
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
                    $infoFactura->appendChild($TConImpuestos);
                }                
            $infoFactura->appendChild($xml->createElement('propina', cls_Global::formatoDecXML($cabFact[0]["Propina"])));
            $infoFactura->appendChild($xml->createElement('importeTotal', cls_Global::formatoDecXML($cabFact[0]["ImporteTotal"])));
            $infoFactura->appendChild($xml->createElement('moneda', $cabFact[0]["Moneda"]));

            //DATOS DE FORMA DE PAGO APLICADO 8 SEP 2016 
            $infoFactura->appendChild($this->pagosXML($pagFact,$xml));
            
        $dom->appendChild($infoFactura);
        
            //DETALLE DE FACTURAS
            $detalles=$xml->createElement('detalles');
            for ($i = 0; $i < sizeof($detFact); $i++) {//DETALLE DE FACTURAS
                $detalle=$xml->createElement('detalle');
                $detalle->appendChild($xml->createElement('codigoPrincipal', utf8_encode(trim($detFact[$i]['CodigoPrincipal']))));
                $detalle->appendChild($xml->createElement('codigoAuxiliar', utf8_encode(trim($detFact[$i]['CodigoAuxiliar']))));
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
        //$strings_xml = $xml->saveXML();
	//echo $strings_xml;
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
    
    //DATOS DE FORMA DE PAGO APLICADO 8 SEP 2016 
    public function pagosXML($pagFact,$xml){
        $valida = new VSValidador;
        $pagos=$xml->createElement('pagos');
        for ($xi = 0; $xi < sizeof($pagFact); $xi++) {
            $pago=$xml->createElement('pago');
            $pago->appendChild($xml->createElement('formaPago', $valida->ajusteNumDoc($pagFact[$xi]["Codigo"], 2) ));
            $pago->appendChild($xml->createElement('total', cls_Global::formatoDecXML($pagFact[$xi]["Total"])));
            $pago->appendChild($xml->createElement('plazo', cls_Global::formatoDecXML($pagFact[$xi]["Plazo"])));
            $pago->appendChild($xml->createElement('unidadTiempo', utf8_encode(trim($pagFact[$xi]['UnidadTiempo']))));
            $pagos->appendChild($pago);
        }
        return $pagos;
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
    
    //*************************************************
    //Funcion solo para Actualizar y Renovar la Informacion de la Firma
    //$res=$obj->actDataFD(2,'xxxx');
    public function actDataFD($ids, $clave) {//Actualiza Firma Digital
        $obj_con = new cls_Base();
        $conCont = $obj_con->conexionAppWeb();
        try {
            //APPWEB.VSFirmaDigital V;          
            $sql = "UPDATE " . $obj_con->BdAppweb . ".VSFirmaDigital SET  Clave= '" . base64_encode($clave) . "' WHERE Id='$ids';";
            //echo $sql;
            $command = $conCont->prepare($sql);
            $command->execute();
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
    //*************************************************

}
