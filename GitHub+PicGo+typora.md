国内的网站备案十分麻烦，只得将图床和未来的静态网站托管在GitHub上

GitHub做图床十分简单，就是开个仓库把图片放进去而已

GitHub免费用户一个项目有1G的空间，足够一些小的博客使用，况且还可以多开仓库

[TOC]

### 新建GitHub仓库

进入网页版GitHub，创建一个Public仓库

接着从右上角的Settings中进入Developer Settings，找到Personal access tokens，点击`Generate new tokens

note中随便填，把`repo`的√打上，`generate token`

生成的token只会显示一次，重新生成后的token与之前不同，且之前所有用到该token的地方都会失效

### 安装并配置PicGo

PicGo是一个用于快速上传图片并获取图片 URL 链接的工具，通过它可以方便地将本地或网络上的图片上传至图床并生成markdown格式的链接

PicGo的GitHub仓库里可以找到exe格式的安装包，直接安装即可

PicGo自带的GitHub配置只能上传图片，不能同步删除图片，可以从插件商城里下载`GitHub-Plus`以满足使用要求

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210202230143780.png" alt="image-20210202230143780" style="zoom: 50%;" />

repo格式为{用户名}/{仓库名}

branch为你要使用的分支，一般为main或master

token上文提到了生成，复制即可

path为图片在仓库中保存的位置，比如img/就是说明图片保存在仓库的img文件夹中，这个比较随意，可不填

customUrl为链接地址，这个主要是用来进行CDN加速的，GitHub的默认链接为`https://raw.githubusercontent.com/{用户名}/{仓库名}`，我们可以使用jsDeliver进行CDN加速，将默认链接改为`https://cdn.jsdelivr.net/gh/{用户名}/{仓库名}`即可

origin用于选择GitHub或gitee，众所周知，gitee狗都不用

点击确定保存，并将其设为默认图床

### 配置Typora

`Ctrl+,`打开Typora的偏好设置，点击`图像`

1. 插入图片时设置为`上传图片`，设置对本地和网络上的图片应用此设置
2. 找到`上传服务设定`，设置上传服务为PicGo（app）并配置PicGo.exe的路径
3. 验证图片上传选项

之后我们插入的本地图片都会被自动上传到图床上并生成markdown格式的链接插入文本中

### 参考

[1]: https://picgo.github.io/PicGo-Doc/zh/guide/	"官方文档"
[2]: https://lovelijunyi.gitee.io/posts/a833.html	"图床对比与Github图床的优化"

