<?php
class mysql_pdo
{
  public static $dbc = null;

  public function __construct()
  {
    if(self::$dbc == null) 
    {
      $conf = ConfigFactory::build ();
      try
      {
        $connection_string = "mysql:host=" . $conf->database->params->host
        . ";dbname=" . $conf->database->params->dbname;

        $pdo = new PDO($connection_string, $conf->database->params->username,
                       $conf->database->params->password);
        $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
	self::$dbc=$pdo;
      } catch(PDOException $e) 
      {
        print $e->getMessage () . "\n";
        die("Could not connect to database.");
      }
    

    }
  self::$dbc=$pdo;

  }
  public function pre($str)
  {
    $this->st = null;
    $this->st = self::$dbc->prepare($str);
  }
  public function bind($str=null,$var=null)
  {
    if($str!=null) $this->st->bindParam($str,$var);  
  }
  public function exe()
  {
    $this->st->execute();
    $row = $this->st->fetchall(PDO::FETCH_ASSOC);
    return $row;
    
  }

}
