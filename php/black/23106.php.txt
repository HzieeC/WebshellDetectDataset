<?php

class machine
{

  public function __construct()
  {
    
    #connect to database
    $this->db = new mysql_pdo();

  }

  
  public function machine_list()
  {

    $this->db->pre('SELECT machine_id, unique_id,name,arch.arch,ip,product.product,release.release,
                     memsize,disksize,description,machine_status.machine_status,
                     anomaly,serialconsole,powerswitch,consoledevice,consolespeed,
                     default_option,type,powertype,powerslot,
                     kernel,`usage` FROM machine 
                     LEFT JOIN arch on machine.arch_id=arch.arch_id 
                     LEFT JOIN machine_status on machine.machine_status_id=machine_status.machine_status_id
                     LEFT JOIN product on product.product_id=machine.product_id 
                     LEFT JOIN `release` on machine.release_id=release.release_id');
    //return $this->db->exe();
    $all_machines = $this->db->exe();
    $new_all=array();
    foreach($all_machines as $machine)
    {
      $this->db->pre('SELECT user.name FROM user_machine LEFT JOIN user ON user.user_id=user_machine.user_id WHERE machine_id=:machine_id');
      $this->db->bind(':machine_id',$machine['machine_id']);
      $resv=$this->db->exe();
      $resva=array();
      foreach($resv as $a)
      {
        $resva[]=$a['name'];
      }
      $resvs=join(',',$resva);
     
      $machine['reservation']=$resvs;
      $new_all[]=$machine;
    }
  return $new_all;
    

  }
  public function machine_get($id)
  {

    $new_all = array();
    $all=$this->machine_list();
    foreach($all as $machine)
    {
       if($machine['machine_id']==$id) return $machine; 
    }

  }
  public function machine_search($str)
  {
    $new_all = array();
    $all=$this->machine_list();
    foreach($all as $machine)
    {
     if(preg_match("/$str/",$machine['name'])) $new_all[]=$machine;
    }
  return $new_all;

  }

  public function machine_free()
  {
    

  }
}


?>

