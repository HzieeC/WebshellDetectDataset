<?php
/**
 * File UtilTest
 *
 * @file     UtilTest
 * @category None
 * @package  Tests
 * @author   Enzo Borel <borelenzo@gmail.com>
 * @license  https://raw.githubusercontent.com/RUCD/webshell-detector/master/LICENSE Webshell-detector
 * @link     https://github.com/RUCD/webshell-detector
 */
namespace RUCD\WebshellDetector;

use PHPUnit\Framework\TestCase;

/**
 * UtilTest class extending TestCase. Performs some basic tests on utilitarian routines
 *
 * @file     UtilTest
 * @category None
 * @package  Tests
 * @author   Enzo Borel <borelenzo@gmail.com>
 * @license  https://raw.githubusercontent.com/RUCD/webshell-detector/master/LICENSE Webshell-detector
 * @link     https://github.com/RUCD/webshell-detector
 */
class UtilTest extends TestCase
{
    private $_strings = ["<?php\n
    \$a =               10;
    \$b = 'phpi     nfo';

    echo \$a;
    \$b();?>",
    "<?php function inline(){return             true;}
    phpinfo();?>"
    ];

    /**
     * Tests the routine Util::removeCRLF
     * 
     * @return void
     */
    public function testRemoveCRLF()
    {
        foreach ($this->_strings as $string) {
            $this->assertTrue(Util::removeCRLF($string) !== $string);
        }
    }

    /**
     * Tests the routine Util::removeMultiWhiteSpaces
     * 
     * @return void
     */
    public function testRemoveMultiWhiteSpaces()
    {
        foreach ($this->_strings as $string) {
            $this->assertTrue(Util::removeMultiWhiteSpaces($string) !== $string);
        }
    }

    /**
     * Tests the routine Util::removeAllWhiteSpaces
     * 
     * @return void
     */
    public function testRemoveAllWhiteSpaces()
    {
        foreach ($this->_strings as $string) {
            $this->assertTrue(Util::removeAllWhiteSpaces($string) !== $string);
        }
    }

    /**
     * Tests the routine Util::removeWhiteSpacesOutsideString
     * 
     * @return void
     */
    public function testRemoveWhiteSpacesOutsideString()
    {
        foreach ($this->_strings as $string) {
            $this->assertTrue(Util::removeWhiteSpacesOutsideString(token_get_all($string)) !== $string);
        }
    }
}
