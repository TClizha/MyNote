闲着没事用selenium库完成了个疫情自动填报的脚本，并配置在Linux服务器上，代码比较简单，这里记录一下配置

[TOC]

### Selenium的配置

#### 浏览器

因为要布置在Ubuntu服务器上，所以要先安装浏览器

搜索Chrome Linux，下载google-chrome-stable_current_amd64.deb，使用命令

```shell
sudo apt install ./google-chrome-stable_current_amd64.deb
```

这里必须要加./，否则会报错

google chrome必须运行在非root账户下，否则会报错

Windows下使用自己的浏览器即可

#### WebDriver

配置完浏览器，接着要配置浏览器驱动

搜索chrome driver、edge driver、Firefox driver等关键词，下载对应浏览器版本的WebDriver

以ChromeDriver为例，从官网下载后解压得到chromedriver，使用如下命令将其移动并给予执行权限

```shell
sudo mv chromedriver /usr/bin/ # 移动驱动
sudo chmod +x /usr/bin/chromedriver # 给予执行权限
```

Windows下据说可以配置环境变量的方法配置驱动，但我配置时遇到了问题，只得直接将解压出来的WebDriver放到python.exe同级目录

#### 安装selenium

Selenium支持多种语言，这里图方便用的python

首先用pip下载Selenium库，一行命令搞定

```shell
pip install selenium
```

### Ubuntu定时任务

ubuntu系统有一个定时任务的管理器crontab，我们只需要编辑定时任务，然后重启定时任务服务就好了。

编辑任务

```shell
crontab -e
```

任务格式

```shell
m h dom mon dow command
(minute) (hour) (day of month) (month) (day of week) command
```

含义如下：

- `m` 每个小时的第几分钟执行该任务
- `h` 每天的第几个小时执行该任务
- `dom` 每月的第几天执行该任务
- `mon` 每年的第几个月执行该任务
- `dow` 每周的第几天执行该任务 
- `command` 指定要执行的程序

```
分      小时    日      月      星期     命令
0-59   0-23   1-31   1-12     0-6     command
```

其他：

- 其中星期中`0`表示周日。
- `*` 代表任何时间，比如第一个分钟，用 `*` 就代表每一小时的每一分钟都执行
- `-` 表示区间，比如`1-3`
- `,` 如果区间不连续，可以用`,`例如`1,3,6`

##### 注意事项

一定要用绝对路径。否则可能会执行失败。

比如，我们要执行

```
python3 autoLinux.py
```

那么你需要干的第一件事是

```
which python3
```

以此来查看`python`命令的真正路径

```
root@ubuntu:~# which python
/usr/bin/python3
```

然后，查看`autoLinux.py`的全路径，在`autoLinux.py`所在文件夹下

```
pwd
/home/usualaccount/code
```

然后路径便为

```
/home/usualaccount/code/autoLinux.py
```

所以整条记录应该这样编辑

```
0 9 * * * /usr/bin/python3 /home/usualaccount/code/autoLinux.py > /tmp/new_blog_bwh.log
```

上面的记录是指每天9点整执行`/root/.pyenv/shims/python /app/python/blog/bwh.py`并将打印日志输出到`/tmp/new_blog_bwh.log`

`>`会重定向输出流，它每次运行都会将文件清空

若不想清空日志，可以使用`>>`

### 参考

* https://www.jianshu.com/p/cbc01d32c7b0

* https://viencoding.com/article/157