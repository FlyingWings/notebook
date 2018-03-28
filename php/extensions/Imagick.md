# Imagick扩展
利用ImageMagic API来创建+修改图像, 可以代替GD使用

## 需求
- PHP >= 5.1.3
- ImageMagic API(libimagick-dev)

## 安装
- yum install php-imagick(Redhat)
- apt-get install php-imagick(Ubuntu)
- pecl install imagick(php-pear extension loaded)


## 用途
1. 生成缩略图`thumbnailImage`
- 参数:
    - int $columns: 宽度
    - int $rows: 长度
    - bool $bestfit(给定后, 会根据原有的比例进行转换, 而上面两个变为最大值)
```lang=php
$img = new Imagick("test.png");
$image->thumbnailImage(400, 400, true);
var_dump
```
2. 为图形设置边框`borderImage`
- 参数:
    - 

