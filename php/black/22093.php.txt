<?php

Yii::app()->moduleManager->register(array(
    'id' => 'webshell',
    'class' => 'application.modules.webshell.WebShellModule',
    'import' => array(
        'application.modules.webshell.*',
    ),
    // Events to Catch
    'events' => array(
        array('class' => 'AdminMenuWidget', 'event' => 'onInit', 'callback' => array('WebShellModule', 'onAdminMenuInit')),
        array('class' => 'WebApplication', 'event' => 'onInit', 'callback' => array('WebShellModule', 'onWebApplicationInit')),
),
));
?>
