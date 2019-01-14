# WebHookTest

LastUpdateDate:   
![LastUpdateDate](https://cvm.xtxtmtxtx.xyz/LastUpdateDate.php)  
LastRequestDate:   
![LastRequestDate](https://cvm.xtxtmtxtx.xyz/LastRequestDate.php)  
WebHookOutput:   
![WebHookOutput](https://cvm.xtxtmtxtx.xyz/WebHookOutput.php)  
``` php
<?php
  error_reporting(1);
  $secret = "onedayanapple";
  $signature = $_SERVER['HTTP_X_HUB_SIGNATURE'];
  echo shell_exec("date | sudo tee /var/www/html/LastRequestDate.txt && echo \"(UTC/GMT+08:00)\" | sudo tee -a /var/www/html/LastRequestDate.txt");
  if ($signature) {
    $hash = "sha1=".hash_hmac('sha1', file_get_contents("php://input"), $secret);
    if (strcmp($signature, $hash) != 0) {
      shell_exec("echo \"Wrong Signature\" 2>&1 | sudo tee /var/www/html/WebHookOutput.txt");
      exit('Wrong Signature');
    }
    shell_exec("date | sudo tee /var/www/html/LastUpdateDate.txt && echo \"(UTC/GMT+08:00)\" | sudo tee -a /var/www/html/LastUpdateDate.txt");
    echo shell_exec("cd /var/www/html/WebHookTest && sudo git pull 2>&1 | sudo tee /var/www/html/WebHookOutput.txt");
    exit('Updated');
  }
  shell_exec("echo \"Error Request\" 2>&1 | sudo tee /var/www/html/WebHookOutput.txt");
  exit('Error Request');
?>

```  
``` php
<?php
  header ("Content-type: image/png");
  mb_internal_encoding("UTF-8");
  function autowrap($fontsize, $angle, $fontface, $string, $width) {
    $content = "";
    for ($i=0;$i<mb_strlen($string);$i++) {
      $letter[] = mb_substr($string, $i, 1);
    }
    foreach ($letter as $l) {
      $teststr = $content." ".$l;
      $testbox = imagettfbbox($fontsize, $angle, $fontface, $teststr);
      if (($testbox[2] > $width) && ($content !== "")) {
        $content .= "\n";
      }
      $content .= $l;
    }
    return $content;
  }
  $file_path = "/var/www/html/OutputFile.txt";
  if(file_exists($file_path))$text = file_get_contents($file_path);
  else $text="Error";
  $text = autowrap(12, 0, "/usr/share/fonts/Hack/Hack-Bold.ttf", $text, 300);
  $rows = 1 + strval(substr_count($text, "\n"));
  $bg = imagecreate(300, 21 * $rows);
  imagecolorallocate($bg, 0xFF, 0xFF, 0xFF);
  $black = imagecolorallocate($bg, 0x00, 0x00, 0x00);
  imagettftext($bg, 12, 0, 0, 21, $black, "/usr/share/fonts/Hack/Hack-Bold.ttf", $text);
  imagepng($bg);
  imagedestroy($bg);
?>
```  
