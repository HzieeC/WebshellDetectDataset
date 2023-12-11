<?php

class ShellController extends Controller {

    public $pages = array(
        "执行命令" => '',
    );

    public function action() {
        $this->render();
    }

    public function execAction() {
        if (!Func::postVar('command')) {
            return;
        }
        $this->var['command'] = $this->qvar(Func::postVar('command'));
        $script = $this->loadScript();
        $data = $this->runShell($script);
        if ($this->config['shellEncode'] != 'UTF-8') {
            $data = mb_convert_encoding($data, 'UTF-8', $this->config['shellEncode']);
        }

        echo $data;
    }

}
