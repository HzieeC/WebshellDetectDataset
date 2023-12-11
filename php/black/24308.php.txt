<?php

/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */

/**
 * Description of VSexception
 *
 * @author root
 */
class VSexception {
    //NOTA: no poner estas funciones como static

    //put your code here
    public static function messageSystem($status, $error, $op, $message, $data) {
        $arroout["status"] = $status;
        $arroout["error"] = $error;
        $arroout["message"] = VSexception::messageError($op, $message);
        $arroout["data"] = $data;
        if($status=="NO_OK"){
            cls_Global::putMessageLogFile($arroout);//Imprime el Error en Logs
            //cls_Global::putMessageLogFile($message);
        }
        return $arroout;
    }
    
    //Mensajes Web Services Sri
    public static function messageWSSRI($status, $error, $op, $message, $data) {
        $arroout["status"] = $status;
        $arroout["error"] = $error;
        $arroout["message"] = VSexception::messageWSSRIError($op, $message);  //$this->messageWSSRIError($op, $message);//$message ." Error Nº ".$op;//
        $arroout["data"] = $data;
        //if($status=="NO_OK"){
            cls_Global::putMessageLogFile($arroout);//Imprime el Error en Logs
        //}
        return $arroout;
    }

    public static function messageFileXML($status, $nomDocfile, $ClaveAcceso, $op, $message, $data) {
        $arroout = array(
            'status' => $status,
            'nomDoc' => $nomDocfile,
            'ClaveAcceso' => $ClaveAcceso,
            'message' => VSexception::messageError($op, $message),
            'data' => $data
        );
        if($status=="NO_OK"){
            cls_Global::putMessageLogFile($arroout);//Imprime el Error en Logs
            cls_Global::putMessageLogFile($message);
        }
        return $arroout;
    }


    private static function messageError($op, $message) {
        $messageError = '';
        switch ($op) {
            case 1:
                //Documento no se Encontro.
                $messageError = 'Error document was not found.';
                break;
            case 2:
                $messageError = 'Gender xml file correctly.';
                break;
            case 3:
                $messageError = 'Failed to perform the signed document.';
                break;
            case 4:
                $messageError = 'Failed to perform validation of the document.';
                break;
            case 6://Petion invalida volver a intentar
                //$messageError=Yii::t('EXCEPTION', 'Invalid request. Please do not repeatt this request again.');
                break;
            case 10://Petion invalida volver a intentar
                $messageError = '<strong>Well done!</strong> your information was successfully saved.';
                break;
            case 11://Petion invalida no volver a intentar
                $messageError = 'Invalid request. Please do not repeatt this request again.';
                break;
            case 12://Datos eliminados Correctamente
                $messageError = '<strong>Well done!</strong> your information was successfully delete.';
                break;
            case 13://Datos eliminados Correctamente
                $messageError = '<strong>Well done!</strong> your information was successfully cancel.';
                break;
            case 15://Datos eliminados Correctamente
                $messageError = '<strong>Well done!</strong> Your document was properly authorized.';
                break;
            case 16://Datos eliminados Correctamente
                $messageError = 'Su Documento fue rechazado o negado.';
                break;
            case 17://Su documento fue Devuelto por errores en el documento
                $messageError = 'Su documento fue Devuelto por errores en el documento';
                break;
            case 20://La solicitud fÚe realizada correctamente.
                $messageError = 'The request was completed successfully.';
                break;
            case 21://No podemos encontrar los datos que está solicitando.
                $messageError = 'We can not find the information you are requesting.';
                break;
            case 22://Por favor vuelva a intentar despues de unos minutos
                $messageError = 'Please come back in a while.';
                break;
            case 30://Su Orden fue guardada correctamente.
                $messageError = '<strong>Well done!</strong> your order was successfully saved.';
                break;
            case 40://Su Orden fue envia correctamente.
                $messageError = '<strong>Well done!</strong> your information was successfully send.';
                break;
            case 41://Su Orden fue guardada correctamente.
                $messageError = 'Failed to send the document, check with your Web Manager.';
                break;
            case 42://Este documento ya fue autorizado por SRI.
                $messageError = 'Este documento ya fue autorizado por SRI.';
                break;
            case 43://Este documento ya fue autorizado por SRI.
                $messageError = 'Access key registered, retry in a few minutes.';
                break;
            case 44://Este documento ya fue autorizado por SRI.
                $messageError = 'At a time when your mail will be sent.';
                break;

            default:
                $messageError = $message;
        }
        return $messageError;
    }
    
    private static function messageWSSRIError($op, $message) {
        $messageError = '';
        switch ($op) {
            case 43:
                //Clave de Acceso Registrada
                $messageError = "Clave de acceso registrada, vuelva a intentarlo en unos minutos. =>>> ".$message;
                break;
            default:
                $messageError = $message ." Error Nº ".$op;//
        }
        return $messageError;
    }
    
    public static function messageErrorDoc($ids,$DBTabDoc,$CampoID,$Estado, $NumDoc, $NomDoc, $Clave, $CodigoError) {
        switch ($Estado) {
            case 2://RECIBIDO SRI (AUTORIZADOS)
                return VSexception::messageFileXML('NO_OK', $NumDoc, null, 42, null, null);
                break;
            case 4://DEVUELTA (NO AUTORIZADOS EN PROCESO)
                //Cuando son devueltas no se deben generar de nuevo la clave de acceso
                //hay que esperar hasta que responda
                switch ($CodigoError) {
                    case 43://CLAVE DE ACCESO REGISTRADA
                        //No genera Nada Envia los datos generados anteriormente
                        //Retorna Automaticamente sin Generar Documento
                        //LA CLAVE DE ACCESO REGISTRADA ingresa directamente a Obtener su autorizacion
                        VSexception::actualizaEstado($ids, 2, $DBTabDoc, $CampoID);
                        return VSexception::messageFileXML('OK_REG', $NomDoc, $Clave, 43, null, null);
                        break;
                    case 70://CLAVE DE ACCESO EN PROCESO
                        return VSexception::messageFileXML('OK', $NomDoc, $Clave, 43, null, null);
                        break;
                    default:
                    //Documento Devuelto hay que volver a generar la clave de Acceso
                    //Esto es Opcional
                }
                break;
            case 8://DOCUMENTO ANULADO
                return VSexception::messageSystem('NO_OK', null, 11, null, null);
                break;
            default:
        }
        //Cuando no ingresa a ninguna opcion y se tiene que generar el XML
        //OK_GER -> Genera el Archivo XML
        return VSexception::messageFileXML('OK_GER', null, null, null, null, null);
    }
    
    public static function actualizaEstado($ids,$estado,$DBTabDoc,$CampoID) {
        $obj_con = new cls_Base();
        $conx = $obj_con->conexionIntermedio();

        try {
            //DescripcionError="'.$DescripcionError.'",CodigoError="'.$CodigoError.'"
            $sql = "UPDATE " . $conx->dbname . ".$DBTabDoc SET Estado='$estado' WHERE $CampoID='$ids'";
            //echo $sql;
            $command = $conx->prepare($sql);
            $command->execute();

            $conx->commit();
            $conx->close();
            return true;
        } catch (Exception $e) {
            $conx->rollback();
            $conx->close();
            //throw $e;
            return false;
        }
    }

}
