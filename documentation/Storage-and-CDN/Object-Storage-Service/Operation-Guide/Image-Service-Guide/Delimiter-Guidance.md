# 自定义分隔符访问图片样式

## 图片样式访问规则

* URL参数方式访问

```
<文件URL>?x-oss-process=imgalias/<stylename>

```
示例：

```
bucketname.s3.cn-north-1.jcloudcs/sample.jpg?i?x-oss-process=imgalias/stylename 

```
* 分隔符方式访问
```
<文件URL><分隔符><StyleName>

```
示例：

```
bucketname.s3.cn-north-1.jcloudcs/sample.jpg{分隔符}stylename

```
当您设置了自定义分隔符后，URL带了分隔符，OSS图片处理服务会把该分隔符后面的内容当成样式的名称。自定义分隔符您可在控制台配置成功后使用，
```
支持 -、 _、 /、 ! 样式分隔符

``` 
该方式为图片处理可选的使用方式,您可根据自身需求设置。

**说明**

* bucketname为存储空间名称，StyleName为您该bucket下的图片样式的名称。
* 文件URL由存储空间名称(bucketname).OSS服务域名(endpoint).文件名（key）组成。
* 创建样式、删除样式和修改样式仅支持在控制台操作。

## 设置分隔符


