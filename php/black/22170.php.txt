<?php
/**
 * File SignaturesAnalyzer
 *
 * @file     SignaturesAnalyzer
 * @category None
 * @package  Source
 * @author   Enzo Borel <borelenzo@gmail.com>
 * @license  https://raw.githubusercontent.com/RUCD/webshell-detector/master/LICENSE Webshell-detector
 * @link     https://github.com/RUCD/webshell-detector
 */
namespace RUCD\WebshellDetector;

use Exception;

/**
 * Class SignaturesAnalyzer implementing Analyzer
 * Performs an analysis of the file, looking for known web shells signatures
 *
 * @file     SignaturesAnalyzer
 * @category None
 * @package  Source
 * @author   Enzo Borel <borelenzo@gmail.com>
 * @license  https://raw.githubusercontent.com/RUCD/webshell-detector/master/LICENSE Webshell-detector
 * @link     https://github.com/RUCD/webshell-detector
 */
class SignaturesAnalyzer implements Analyzer
{

    const ENCODE_MAX = 10;
    const FINGERPRINTS_FILE = "shells.txt";

    /**
     * Iterates over the array of signatures and checks if a pattern matches
     * 
     * @param array  $pFingerprints Array of signatures
     * @param string $pFileContent  The content of a file
     * 
     * @return NULL|string Returns the first mathing signature, NULL if nothing matches
     */
    private function _compareFingerprints($pFingerprints, $pFileContent)
    {
        $key = null;
        foreach ($pFingerprints as $fingerprint => $shell) {
            if (preg_match($fingerprint, $pFileContent)) {
                if ($fingerprint != "/version/") {
                    $key = $shell;
                    break;
                }
            }
        }
        return $key;
    }
    
    /**
     * TEST: comes from PHP-Webshell Detector, will be updated. Only for testing
     *
     * @param string $pFileContent The content of a file
     * 
     * @return NULL|string Returns NULL if the file content doesn't exist, a string if a flag is found
     */
    public function scanFile($pFileContent)
    {
        if ($pFileContent == null || !strlen($pFileContent)) {
            return null;
        }
        $fingerprints = $this->_getFingerprints();
        if ($fingerprints == null || !count($fingerprints)) {
            return null;
        }
        $fp_regex = array();
        $version = 0;
        foreach ($fingerprints as $fingerprint => $shell) {
            if (strpos($fingerprint, 'bb:') !== false) {
                $fingerprint = base64_decode(str_replace('bb:', '', $fingerprint));
            }
            $fp_regex['/' . preg_quote($fingerprint, '/') . '/'] = $shell;
        }
        $flag = $this->_compareFingerprints($fp_regex, $pFileContent);
        if ($flag != null)
            return $flag;
        $flag = $this->_compareFingerprints($fp_regex, base64_encode($pFileContent));
        if ($flag != null)
            return $flag;
        $tokens = token_get_all(Util::extendOpenTag($pFileContent));
        $decode = ["base64_decode", "gzuncompress", "gzinflate", "gzdecode"];
        foreach ($tokens as $token) {
            if (is_array($token)) {
                if ($token[0] === T_CONSTANT_ENCAPSED_STRING || $token[0] === T_ENCAPSED_AND_WHITESPACE) {
                    $str = substr($token[1], 1, strlen($token[1])-1);
                    $flag = $this->_compareFingerprints($fp_regex, $str);
                    if ($flag != null)
                        return $flag;
                    foreach ($decode as $decodefunc) {
                        $param = $flag = null;
                        try {
                            $param = $decodefunc($str);
                        } catch (Exception $e) {
                            continue;
                        }
                        if ($param != null) {
                            $flag = $this->_compareFingerprints($fp_regex, $param);
                            if ($flag != null)
                                return $flag;
                        }
                    }
                }
            }
        }
    }

    /**
     * Reads the file containing signatures
     *
     * @return array An array containing shells signatures
     */
    private function _getFingerprints()
    {
        $res = [];
        $fileName = __DIR__.'/../res/'. self::FINGERPRINTS_FILE;
        if (file_exists($fileName)) {
            $lines = file($fileName, FILE_IGNORE_NEW_LINES);
            for ($i = 0; $i < count($lines)-1; $i+=2) {
                $res[$lines[$i]] = $lines[$i+1];
                /*if (substr($lines[$i+1], -1) !== "]") {
                    //version
                    echo $lines[$i].PHP_EOL.$lines[$i+1].PHP_EOL.'****************'.PHP_EOL;
                }*/
            }
        }
        return $res;
    }
    
    /**
     * Performs an analysis on a file regarding signatures of known signatures
     * {@inheritDoc}
     * 
     * @param string $filecontent The content of the file to analyze
     * 
     * @see \RUCD\WebshellDetector\Analyzer::analyze()
     * 
     * @return double The score of the file
     */
    public function analyze($filecontent)
    {
        $ret = $this->scanFile($filecontent);
        return $ret === null ? 0: 1; 
    }
}
