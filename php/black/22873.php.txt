<?php
//include('cls_Base.php');
class EMPRESA {
    //put your code here
    public function buscarDataEmpresa($emp_id,$est_id,$pemi_id) {
        $obj_con = new cls_Base();
        $conApp = $obj_con->conexionAppWeb();
        $sql = "SELECT A.EMP_ID,A.EMP_RUC Ruc,A.EMP_RAZONSOCIAL RazonSocial,A.EMP_NOM_COMERCIAL NombreComercial,
                    A.EMP_AMBIENTE Ambiente,A.EMP_TIPO_EMISION TipoEmision,EMP_DIRECCION_MATRIZ DireccionMatriz,EST_DIRECCION DireccionSucursal,
                    A.EMP_OBLIGA_CONTABILIDAD ObligadoContabilidad,EMP_CONTRI_ESPECIAL ContribuyenteEspecial,EMP_EMAIL_DIGITAL,
                    B.EST_NUMERO Establecimiento,C.PEMI_NUMERO PuntoEmision,A.EMP_MONEDA Moneda
                    FROM " . $obj_con->BdAppweb . ".EMPRESA A
                            INNER JOIN (" . $obj_con->BdAppweb . ".ESTABLECIMIENTO B
                                            INNER JOIN " . $obj_con->BdAppweb . ".PUNTO_EMISION C
                                                    ON B.EST_ID=C.EST_ID AND C.EST_LOG='1')
                                    ON A.EMP_ID=B.EMP_ID AND B.EST_LOG='1'
            WHERE A.EMP_ID='$emp_id' AND A.EMP_EST_LOG='1' 
                     AND B.EST_ID='$est_id' AND C.PEMI_ID='$pemi_id'";
        //echo $sql;
        //$rawData = $conApp->createCommand($sql)->queryAll(); //Varios registros =>  $rawData[0]['RazonSocial']
        //$rawData = $conApp->createCommand($sql)->queryRow();  //Un solo Registro => $rawData['RazonSocial']
        //$conCont->active = false;
        $sentencia = $conApp->query($sql);
        return $sentencia->fetch_assoc();
    }
    
    public function buscarDatoVendedor($vend_id) {
        $obj_con = new cls_Base();
        $conApp = $obj_con->conexionAppWeb();
        //$rawData = array();
        $sql = "SELECT USU_NOMBRE NombreUser,USU_CORREO CorreoUser FROM " . $obj_con->BdAppweb . ".USUARIO WHERE USU_ID='$vend_id';";
        $sentencia = $conApp->query($sql);
        $conApp->close();
        return $sentencia->fetch_assoc();
    }
    
    public static function buscarAmbienteEmp($IdCompania,$Ambiente) {
        $obj_con = new cls_Base();
        $conApp = $obj_con->conexionAppWeb();
        $sql = "SELECT Recepcion,Autorizacion,RecepcionLote,TiempoRespuesta,TiempoSincronizacion "
                . "FROM " . $obj_con->BdAppweb . ".VSServiciosSRI WHERE EMP_ID=$IdCompania AND Ambiente=$Ambiente AND Estado=1 ";
        $sentencia = $conApp->query($sql);
        $conApp->close();
        return $sentencia->fetch_assoc();
    }
    
    public static function infoTributariaXML($cabDoc,$xml){
        $valida= new cls_Global;
        $infoTributaria=$xml->createElement('infoTributaria');
        $infoTributaria->appendChild($xml->createElement('ambiente', $cabDoc[0]['Ambiente']));
        $infoTributaria->appendChild($xml->createElement('tipoEmision', $cabDoc[0]['TipoEmision']));        
        $infoTributaria->appendChild($xml->createElement('razonSocial', utf8_encode($valida->limpioCaracteresXML(trim(strtoupper($cabDoc[0]['RazonSocial']))))));
        $infoTributaria->appendChild($xml->createElement('nombreComercial', utf8_encode($valida->limpioCaracteresXML(trim(strtoupper($cabDoc[0]['NombreComercial']))))));
        $infoTributaria->appendChild($xml->createElement('ruc', $cabDoc[0]['Ruc']));
        $infoTributaria->appendChild($xml->createElement('claveAcceso', $cabDoc[0]['ClaveAcceso']));
        $infoTributaria->appendChild($xml->createElement('codDoc', $cabDoc[0]['CodigoDocumento']));
        $infoTributaria->appendChild($xml->createElement('estab', $cabDoc[0]['Establecimiento']));
        $infoTributaria->appendChild($xml->createElement('ptoEmi', $cabDoc[0]['PuntoEmision']));
        $infoTributaria->appendChild($xml->createElement('secuencial', $cabDoc[0]['Secuencial']));
        $infoTributaria->appendChild($xml->createElement('dirMatriz', utf8_encode(trim($cabDoc[0]['DireccionMatriz']))));
        return $infoTributaria;
    }
    
    public static function infoAdicionalXML($adiFact,$xml){
        //Razones por la que el Servicio Tomcat Se cae
        //Verificar XML si esta Bien Generado (Probar el Error en Navegador)
        //Evitar caracteres Especiales
        $valida = new cls_Global;
        $infoAdicional=$xml->createElement('infoAdicional');
        for ($i = 0; $i < sizeof($adiFact); $i++) {
            if(strlen(trim($adiFact[$i]['Descripcion']))>0){
                //$xmldata .='<campoAdicional nombre="' . utf8_encode(trim($adiFact[$i]['Nombre'])) . '">' . utf8_encode($valida->limpioCaracteresXML(trim($adiFact[$i]['Descripcion']))) . '</campoAdicional>';
                $campoA=$xml->createElement('campoAdicional',utf8_encode($valida->limpioCaracteresXML(trim($adiFact[$i]['Descripcion']))) );
                $campoA->setAttribute('nombre', $valida->limpioCaracteresXML(trim($adiFact[$i]['Nombre'])));
                $infoAdicional->appendChild($campoA);
            }
        }
        return $infoAdicional;
    }
    
}
