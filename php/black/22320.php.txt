<?php

class App {

    public $config;
    public $controllers = array();
    protected $controllerData;
    protected $controller;
    protected $shell;
    protected $driver;

    public function __construct($config = null) {
        $this->config = $config;
        $this->loadControllers();
        $this->loadDriver();
    }

    public function run() {
        if (!Func::getVar('shell')) {
            return $this;
        }
        $default = $this->config['defaultController'] ? $this->config['defaultController'] : 'index';
        $c = isset($_GET['c']) ? $_GET['c'] : $default;
        $this->controller = $this->controllers[$c];
        if (isset($this->controllers[$c])) {
            $this->controllerData = $this->controller->setDriver($this->driver)->run();
        }
        return $this;
    }

    public function show() {
        if (isset($_GET['ajax'])) {
            require(__DIR__ . '/html/ajax.php');
            return;
        }
        require(__DIR__ . '/html/index.php');
    }

    public function getPages() {
        $pages = array();
        foreach ($this->controllers as $c => $controllers) {
            if ($controllers->pages) {
                $pages[$c] = $controllers->pages;
            }
        }
        return $pages;
    }

    protected function loadControllers() {
        $dir = __DIR__ . '/modules/';
        $files = scandir($dir);
        foreach ($files as $file) {
            if ($file == '.' || $file == '..') {
                continue;
            }
            $classFile = $dir . $file . '/' . ucfirst(strtolower($file)) . 'Controller.php';
            if (!file_exists($classFile)) {
                continue;
            }
            require_once($classFile);
            $class = ucfirst($file) . "Controller";
            $this->controllers[strtolower($file)] = new $class($this->config);
        }
    }

    protected function loadDriver() {
        $driver = isset($this->config['driver']) ? $this->config['driver'] . 'Driver' : 'defaultDriver';
        if (Func::getVar('driver')) {
            $driver = Func::getVar('driver') . 'Driver';
        }
        require_once(__DIR__ . '/driver/' . $driver . '.php');
        $this->driver = new $driver();
    }

    static public function loadDrivers() {
        $files = scandir(__DIR__ . '/driver/');
        $drivers = array();
        foreach ($files as $file) {
            if (!preg_match('/^(\w+Driver)\.php$/', $file, $match)) {
                continue;
            }
            require_once(__DIR__ . '/driver/' . $file);
            $name = $match[1];
            $class = new $name();
            $drivers[trim($name, 'Dirver')] = $class->shellCode;
        }
        return $drivers;
    }

}

class Controller {

    public $config;
    public $pages = array();
    protected $var;
    protected $path;
    protected $driver;

    public function __construct($config = null) {
        $this->config = $config;
        $this->path = $this->path();
    }

    public function setDriver(ShellDriver $driver) {
        $this->driver = $driver;
        return $this;
    }

    static function url($c, $a = null, $p = array()) {
        $params['c'] = $c;
        if ($a) {
            $params['a'] = $a;
        }
        if (Func::getVar('shell')) {
            $params['shell'] = Func::getVar('shell');
        }
        if (Func::getVar('driver')) {
            $params['driver'] = Func::getVar('driver');
        }
        $params = array_merge($params, $p);
        return 'index.php?' . http_build_query($params);
    }

    public function run() {
        $a = Func::getVar('a');
        $method = $a . 'Action';
        if (method_exists($this, $a . 'Action')) {
            ob_start();
            $this->$method();
            $data = ob_get_clean();
            return $data;
        }
    }

    protected function path() {
        $class = get_class($this);
        $reflect = new ReflectionClass($class);
        return dirname($reflect->getFileName());
    }

    public function render($view = NULL, $data = NULL) {
        $file = $view ? 'html_' . $view : 'html';
        require($this->path . '/' . $file . '.php');
    }

    public function loadScript($script = NULL) {
        $name = $script ? 'script_' . $script : 'script';
        $file = $this->path . '/' . $name . '.php';
        if (file_exists($file)) {
            $script = file_get_contents($file);
            $script = str_replace('<?php', '', $script);
            $script = preg_replace_callback('/\$VAR\[\'([\w_]+?)\'\]/', array($this, 'replace'), $script);
            return trim($script);
        }
    }

    protected function qvar($string) {
        $string = str_replace('$', "\\$", $string);
        return '"' . $string . '"';
    }

    protected function replace($match) {
        if (isset($this->var[$match[1]])) {
            return $this->var[$match[1]];
        }
    }

    public function runShell($script, $ext = array()) {
        $this->driver->url = Func::getVar('shell');
        return $this->driver->runShell($script, $ext);
    }

}

abstract class ShellDriver {

    public $url;
    public $shellCode;

    abstract public function runShell($script, $ext = array());
}

class Func {

    static public function getVar($var) {
        $return = isset($_GET[$var]) ? $_GET[$var] : NULL;
        if (get_magic_quotes_gpc()) {
            return stripslashes($return);
        }
        else {
            return $return;
        }
    }

    static public function postVar($var) {
        $return =  isset($_POST[$var]) ? $_POST[$var] : NULL;
        if (get_magic_quotes_gpc()) {
            return stripslashes($return);
        }else {
            return $return;
        }

    }

}
