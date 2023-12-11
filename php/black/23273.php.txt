<?php

/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */

class VSClaveAcceso {

    public function claveAcceso($tip_doc, $fec_doc, $ruc, $ambiente, $serie, $numDoc, $tipoemision) {
        $valida=new VSValidador();
        //http://es.wikipedia.org/wiki/C%C3%B3digo_de_control
        $clave = '';
        $fecha = date("dmY", strtotime($fec_doc)); //ddmmyyyy
        //Se multiplica la Numero Documento para que nos de un valor Unico y para la coprobacion 
        //se divide y se extrae el valor que debera ser comparado con la serie
        $codNumerico = $valida->ajusteNumDoc((floatval($numDoc) * 777), 8); //Generado por Seguridad Comprobacion 8 Digitos
        $clave = $fecha . $tip_doc . $ruc . $ambiente . $serie . $numDoc . $codNumerico . $tipoemision;
        //echo '<br>'.strlen($clave);
        //echo '<br>'.$clave.$this->modulo11($clave);
        return $clave . $this->modulo11($clave);
    }

    private function modulo11($clave) {
        //echo $clave.'<br>';
        //echo $claveInv=strrev($clave);//Se invierte la CLave para 
        $len = strlen($clave) - 1;
        $resultado = 0;
        $numBase = 2;
        $suma = 0;
        for ($i = $len; $i >= 0; $i--) {
            //echo '<br>'.$digito=$clave[$i];
            $digito = (int) $clave[$i];
            $suma+=$numBase * $digito;
            //echo '<br>'.$digito.'*'.$numBase.'='.$numBase*$digito;
            $numBase = ($numBase < 7) ? $numBase + 1 : 2;
        }
        //echo '<br>Total '.$suma;
        $resMod = $suma % 11;
        //echo '<br>Mod '.$resMod;
        $resta = 11 - $resMod;
        //echo '<br>Rest 11-'.$resMod.'='.$resta;
        if ($resta < 10) {
            $resultado = $resta;
        } elseif ($resta == 10) {
            $resultado = 1; //Cuando es igual a 10
        } else {
            $resultado = 0; //Caso Contrario es 11
        }
        //echo '<br>Digito'.$resultado;
        return $resultado;
    }

}
