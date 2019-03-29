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

# 控制台设置分隔符

1.登录京东云控制台，选择对象存储->空间管理->图片处理，如图：

 ![自定义分隔符](../../../../image/Object-Storage-Service/OSS-148.png)
 
 2.点击【图片访问设置】，打开设置页面。
 
 ![自定义分隔符](../../../../image/Object-Storage-Service/OSS-149.png)
 
 **说明**
 
 * 支持设置原图保护与分隔符。原图保护详细介绍以及设置方式请参考[原图保护](../../../../image/Object-Storage-Service/OSS-148.png)
 * 自定义分隔符。
 
 4.在 **自定义分隔符** 选项处，选择一个或多个分隔符。
 
5. 点击【确定】，完成设置。

## 示例
### 直接参数访问


### 通过URL参数，使用样式访问

### 通过样式分隔符，使用样式方式访问


三者可以达到同样的效果。


