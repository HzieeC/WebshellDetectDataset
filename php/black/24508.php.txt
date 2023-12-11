<?php
$cabDocPDF = '<table style="width:100%" class="tabDetalle">
    <tbody>
        <tr>
            <td class="marcoCel titleDetalle">
                <span>Cod. Principal</span>
            </td>            
            <td class="marcoCel titleDetalle">
                <span>Cant.</span>
            </td>
            <td class="marcoCel titleDetalle">
                <span>Descripción</span>
            </td>            
            <td class="marcoCel titleDetalle">
                <span>Precio Unitario</span>
            </td>
            <td class="marcoCel titleDetalle">
                <span>Descuento</span>
            </td>
            <td class="marcoCel titleDetalle">
                <span>Precio Total</span>
            </td>
        </tr>';
        for ($fil = 0; $fil < sizeof($detFact); $fil++) {
            $cabDocPDF .= '<tr>
                <td class="marcoCel">'.$detFact[$fil]['CodigoPrincipal'] .'</td>
                <td class="marcoCel dataNumber">'.intval($detFact[$fil]['Cantidad']).'</td>
                <td class="marcoCel">'. utf8_encode($obj_var->limpioCaracteresXML(trim($detFact[$fil]['Descripcion']))) .'</td>             
                <td class="marcoCel dataNumber">'.$detFact[$fil]['PrecioUnitario'] .'</td>
                <td class="marcoCel dataNumber">'. number_format($detFact[$fil]['Descuento'], $obj_var->decimalPDF, $obj_var->SepdecimalPDF, '')  .'</td>
                <td class="marcoCel dataNumber">'. number_format($detFact[$fil]['PrecioTotalSinImpuesto'], $obj_var->decimalPDF, $obj_var->SepdecimalPDF, '') .'</td>
            </tr>';
        }
    $cabDocPDF .= '</tbody>
</table>';
?>