# 关键概念

## 区块

区块用于关联链接，显示自定义排序数据，比如：导航条、推荐排行、热门列表。

在后台区块菜单进行编辑，默认添加了几个常用的区块类别。

>    前台通过 \service\Block::shows 方法调用。
>
>    阅读 /app_frontend/themes/default/site/index.php 获得范例。

## 根目录 /service

这个目录下的文件一般用于提供缓存model的方法。

## 根目录 /wskm

这个目录下的文件用于扩展yii类、封装第三方类库等辅助作用。如果yii已有的类无法帮助你，你可以在此目录中寻找帮助。

## 缓存更新

如果你使用的 \service\Block 或 \service\Content 类获得缓存数据，可通过后台设置中的“刷新缓存”配置项来立即更新缓存。

## Restful

暂时只支持内部访问，代码位于：/common/modules/rest，自行添加代码，可用于vue.js等框架调用。

后台中已经配置，访问：http://后台域名/index.php?r=rest/category 可以得到分类列表。

## 采集

提供导入Rss2数据的功能，代码对于：/console/controllers/ArticleController.php，使用命令行yii article/import-rss。

>    导入会自动下载图片到本地。

>    后台编辑器有提供下载图片的功能。
