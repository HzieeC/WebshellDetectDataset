<?php

class DirController extends Controller {

    public $pages = array(
        "目录" => '',
    );

    public function action() {
        $dir = Func::getVar('dir') ? $this->qvar(str_replace('\\', '/', Func::getVar('dir'))) : 'NULL';
        $this->var['dir'] = $dir;
        if ($this->config['shellEncode'] != 'UTF-8') {
            $this->var['dir'] = mb_convert_encoding($this->var['dir'], $this->config['shellEncode'], 'UTF-8');
        }
        $script = $this->loadScript();
        $_data = $this->runShell($script);
        if (!$_data) {
            return;
        }
        $_data = @unserialize($_data);
        if (!$_data) {
            $_data = array('path' => "", 'files' => array());
        }
        $data = array();
        if ($this->config['shellEncode'] != 'UTF-8') {
            $_data['path'] = mb_convert_encoding($_data['path'], 'UTF-8', $this->config['shellEncode']);
            foreach ($_data['files'] as $k => $v) {
                $data[mb_convert_encoding($k, 'UTF-8', $this->config['shellEncode'])] = $v;
            }
        }
        $_data['files'] = $data;
        $this->render(null, $_data);
    }

    public function downloadAction() {
        $file = Func::getVar('f');
        if (!$file) {
            return;
        }
        $_t = explode('/', $file);
        $name = end($_t);
        $this->var['file'] = $this->qvar($file);
        if ($this->config['shellEncode'] != 'UTF-8') {
            $this->var['file'] = mb_convert_encoding($this->var['file'], $this->config['shellEncode'], 'UTF-8');
        }
        $script = $this->loadScript('download');
        $data = $this->runShell($script);
        header('Content-Type: application/octet-stream');
        header('Content-Disposition: attachment; filename="' . $name . '"');
        echo $data;
    }

}
