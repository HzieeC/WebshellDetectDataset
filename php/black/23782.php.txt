<?php

class PhpController extends Controller {

    public $pages = array(
        "执行PHP" => '',
    );

    public function action() {
        $this->render();
    }

    public function execAction() {
        if (!Func::postVar('command')) {
            return;
        }
        // $this->var['command'] = $this->qvar(Func::postVar('command'));
        // $script = $this->loadScript();
        $data = $this->runShell(Func::postVar('command'));
        if ($this->config['shellEncode'] != 'UTF-8') {
            $data = mb_convert_encoding($data, 'UTF-8', $this->config['shellEncode']);
        }

        echo $data;
    }

}
