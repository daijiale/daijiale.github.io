title: 【PHP】cURL使用之PHP
date: 2015-9-21 21:21:11
tags:

 - PHP


categories:

 - 原创博文
 - Web开发笔记

---


# 前言 #

> cURL(Client URL Library Functions): curl is a command line tool for transferring data with URL syntax,即使用URL语法传输数据的命令行工具。
<!--more-->


# 学习篇 #
- 1、初始化cURL：curl_init()
- 2、发送请求到server：curl_exec()
- 3、接收server数据：curl_exec()
- 4、关闭cURL：curl_close()





# 开发篇 #

php -f xxx.php > xxx.html

- 1、利用cURL直接抓取“百度”首页

```php
<?php
$curl=curl_init('http://www.baidu.com');
curl_exec($curl);
curl_close($curl);
?>
```

- 2、网络上下载一个网页并把内容中的“百度”替换为“屌丝”之后输出

```php
<?php
$curlobj = curl_init();			// 初始化
curl_setopt($curlobj, CURLOPT_URL, "http://www.baidu.com");		// 设置访问网页的URL
curl_setopt($curlobj, CURLOPT_RETURNTRANSFER, true);			// 执行之后不直接打印出来
$output=curl_exec($curlobj);	// 执行
curl_close($curlobj);			// 关闭cURL
echo str_replace("百度","屌丝",$output);
?>

```

- 3、登录慕课网并下载个人空间页面

```php
<?php
/**
 * 自定义实现页面链接跳转抓取
 * 
 */
$data='username=demo_peter@126.com&password=123qwe&remember=1';
$curlobj = curl_init();			// 初始化
curl_setopt($curlobj, CURLOPT_URL, "http://www.imooc.com/user/login");		// 设置访问网页的URL
curl_setopt($curlobj, CURLOPT_RETURNTRANSFER, true);			// 执行之后不直接打印出来

// Cookie相关设置，这部分设置需要在所有会话开始之前设置
date_default_timezone_set('PRC'); // 使用Cookie时，必须先设置时区
curl_setopt($curlobj, CURLOPT_COOKIESESSION, TRUE); 
curl_setopt($curlobj, CURLOPT_HEADER, 0); 
// 注释掉这行，因为这个设置必须关闭安全模式 以及关闭open_basedir，对服务器安全不利
//curl_setopt($curlobj, CURLOPT_FOLLOWLOCATION, 1);  

curl_setopt($curlobj, CURLOPT_POST, 1);  
curl_setopt($curlobj, CURLOPT_POSTFIELDS, $data);  
curl_setopt($curlobj, CURLOPT_HTTPHEADER, array("application/x-www-form-urlencoded; charset=utf-8", 
	"Content-length: ".strlen($data)
	)); 
curl_exec($curlobj);	// 执行
curl_setopt($curlobj, CURLOPT_URL, "http://www.imooc.com/space/index");
curl_setopt($curlobj, CURLOPT_POST, 0);  
curl_setopt($curlobj, CURLOPT_HTTPHEADER, array("Content-type: text/xml"
	)); 
$output=curl_redir_exec($curlobj);	// 执行
curl_close($curlobj);			// 关闭cURL
echo $output;

/**
 * 自定义实现页面链接跳转抓取
 */
function curl_redir_exec($ch,$debug="") 
{ 
    static $curl_loops = 0; 
    static $curl_max_loops = 20; 

    if ($curl_loops++ >= $curl_max_loops) 
    { 
        $curl_loops = 0; 
        return FALSE; 
    } 
    curl_setopt($ch, CURLOPT_HEADER, true); // 开启header才能够抓取到重定向到的新URL
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true); 
    $data = curl_exec($ch); 
    // 分割返回的内容
    $h_len = curl_getinfo($ch, CURLINFO_HEADER_SIZE); 
    $header = substr($data,0,$h_len);
    $data = substr($data,$h_len - 1);

    $http_code = curl_getinfo($ch, CURLINFO_HTTP_CODE); 
    if ($http_code == 301 || $http_code == 302) { 
        $matches = array(); 
        preg_match('/Location:(.*?)\n/', $header, $matches); 
        $url = @parse_url(trim(array_pop($matches))); 
        // print_r($url); 
        if (!$url) 
        { 
            //couldn't process the url to redirect to 
            $curl_loops = 0; 
            return $data; 
        } 
        $last_url = parse_url(curl_getinfo($ch, CURLINFO_EFFECTIVE_URL)); 
        if (!isset($url['scheme'])) 
            $url['scheme'] = $last_url['scheme']; 
        if (!isset($url['host'])) 
            $url['host'] = $last_url['host']; 
        if (!isset($url['path'])) 
            $url['path'] = $last_url['path'];

        $new_url = $url['scheme'] . '://' . $url['host'] . $url['path'] . (isset($url['query'])?'?'.$url['query']:''); 
        curl_setopt($ch, CURLOPT_URL, $new_url); 

        return curl_redir_exec($ch); 
    } else { 
        $curl_loops=0; 
        return $data; 
    } 
} 
?>
```

- 4、获取webservice天气数据

```php
<?php
$data = 'theCityName=北京';
$curlobj = curl_init();	
curl_setopt($curlobj, CURLOPT_URL, "http://www.webxml.com.cn/WebServices/WeatherWebService.asmx/getWeatherbyCityName");  
curl_setopt($curlobj, CURLOPT_HEADER, 0); 
curl_setopt($curlobj, CURLOPT_RETURNTRANSFER, 1);  
curl_setopt($curlobj, CURLOPT_POST, 1);  
curl_setopt($curlobj, CURLOPT_POSTFIELDS, $data);  
curl_setopt($curlobj, CURLOPT_HTTPHEADER, array("application/x-www-form-urlencoded; charset=utf-8", 
	"Content-length: ".strlen($data)
	)); 
$rtn = curl_exec($curlobj);   
if(!curl_errno($curlobj)){
	// $info = curl_getinfo($curlobj); 
	// print_r($info);
	echo $rtn;  
} else {
  echo 'Curl error: ' . curl_error($curlobj);
}
curl_close($curlobj);
?>
```

- 5、从FTP上下载数据

```php
<?php
$curlobj = curl_init();	
curl_setopt($curlobj, CURLOPT_URL, "ftp://192.168.1.100/downloaddemo.txt");  
curl_setopt($curlobj, CURLOPT_HEADER, 0); 
curl_setopt($curlobj, CURLOPT_RETURNTRANSFER, 1);  
curl_setopt($curlobj, CURLOPT_TIMEOUT, 300); // times out after 300s
curl_setopt($curlobj, CURLOPT_USERPWD, "peter.zhou:123456");//FTP用户名：密码
// Sets up the output file
$outfile = fopen('dest.txt', 'wb');//保存到本地的文件名
curl_setopt($curlobj, CURLOPT_FILE, $outfile);

$rtn = curl_exec($curlobj);  
fclose($outfile); 
if(!curl_errno($curlobj)){
	// $info = curl_getinfo($curlobj); 
	// print_r($info);
	echo "RETURN: " . $rtn;  
} else {
  echo 'Curl error: ' . curl_error($curlobj);
}
curl_close($curlobj);
?>
```

- 6、上传数据到FTP 

```php
<?php

$curlobj = curl_init();	
$localfile = 'ftp01.php';
$fp = fopen($localfile, 'r');

curl_setopt($curlobj, CURLOPT_URL, "ftp://192.168.1.100/ftp01_uploaded.php");  
curl_setopt($curlobj, CURLOPT_HEADER, 0); 
curl_setopt($curlobj, CURLOPT_RETURNTRANSFER, 1);  
curl_setopt($curlobj, CURLOPT_TIMEOUT, 300); // times out after 300s
curl_setopt($curlobj, CURLOPT_USERPWD, "peter.zhou:123456");//FTP用户名：密码

curl_setopt($curlobj, CURLOPT_UPLOAD, 1);
curl_setopt($curlobj, CURLOPT_INFILE, $fp);
curl_setopt($curlobj, CURLOPT_INFILESIZE, filesize($localfile));
$rtn = curl_exec($curlobj);  
fclose($fp); 
if(!curl_errno($curlobj)){
	echo "Uploaded successfully.";  
} else {
  echo 'Curl error: ' . curl_error($curlobj);
}
curl_close($curlobj);
?>

```

- 7、下载网络上面的一个HTTPS的资源

```php
<?php
$curlobj = curl_init();			// 初始化
curl_setopt($curlobj, CURLOPT_URL, "https://ajax.aspnetcdn.com/ajax/jquery.validate/1.12.0/jquery.validate.js");		// 设置访问网页的URL
curl_setopt($curlobj, CURLOPT_RETURNTRANSFER, true);			// 执行之后不直接打印出来

// 设置HTTPS支持
date_default_timezone_set('PRC'); // 使用Cookie时，必须先设置时区
curl_setopt($curlobj, CURLOPT_SSL_VERIFYPEER, 0); // 对认证证书来源的检查从证书中检查SSL加密算法是否存在
curl_setopt($curlobj, CURLOPT_SSL_VERIFYHOST, 2); // 

$output=curl_exec($curlobj);	// 执行
curl_close($curlobj);			// 关闭cURL
echo $output;
?>
```


# 更多参考
- [官方手册](http://www.php.net/manual/zh/book.curl.php)
- [PHP中文手册](http://php.net/manual/zh/)


