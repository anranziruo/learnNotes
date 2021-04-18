##### http请求过程中异步执行脚本
```
在cli模式下，必须要使用&和指定输出（重定向到/dev/null），让命令行异步执行
<?php
    $cmd = 'php  test.php  >/dev/null  &';
    exec($cmd);
    $cmd = 'php test.php   >/dev/null  &';
    system($cmd);
?>
```