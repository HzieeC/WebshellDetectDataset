<?php
/**
 * Webshell PHP library
 *
 * @category   Webshell
 * @package    Webshelll
 * @copyright  Copyright (c) 2011-2012 Webshell SAS
 * @license    http://www.gnu.org/copyleft/lesser.html     LGPL License
 */

/**
 * @category   Webshell
 * @package    Webshell
 * @copyright  Copyright (c) 2011-2012 Webshell SAS
 * @license    http://www.gnu.org/copyleft/lesser.html     LGPL License
 */
class Webshell {
    /**
     * Public API Key
     *
     * @var string
     */
    private $_apiKey = '';

    /**
     * Secret API Key
     *
     * @var string
     */
    private $_secretKey = '';

    /**
     * Client session identifier
     *
     * @var string
     */
    private $_userid = '';

    /**
     * Webshell API url
     *
     * @var string
     */
    private $_baseUrl = 'http://webshell.io/api';

    private static $_instance;

    private function __construct()
    {
    }

    /**
     * Retrieve current instance
     *
     * @return Webshell
     */
    public static function getInstance()
    {
        if ( ! isset(self::$_instance))
            self::$_instance = new Webshell();

        return self::$_instance;
    }

    /**
     * Set user session identifier
     *
     * @param  string $user_id Session identifier to set
     * @return Webshell
     */
    public function setUserId($user_id)
    {
        $this->_userid = $user_id;
        return $this;
    }

    /**
     * Initialize webshell
     *
     * @param  string $apikey App's public key
     * @param  string $secret App's secret key
     * @return Webshell
     */
    public function init($apikey, $secret = '') {
        $this->_apiKey = $apikey;
        $this->_secretKey = $secret;
        return $this;
    }

    /**
     * Execute wsh code
     *
     * @param  string $cmd Code to execute
     * @return string Raw data
     */
    public function exec($cmd)
    {
        if (empty($this->_apiKey))
            return null;
        if (! $this->_userid)
            $this->_userid = sha1(uniqid());
        $url = $this->_baseUrl . "?response=json&request=" . urlencode($cmd) . "&key=" . $this->_apiKey . "&secret=" . $this->_secretKey . "&cip=" . $this->_userid;
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL, $url);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
        curl_setopt($ch, CURLOPT_REFERER, "http://" . $_SERVER['HTTP_HOST']);
        $body = curl_exec($ch);
        curl_close($ch);
        return $body;
    }
}
?>