title: Linux命令分享
speaker: 孙新杰
transition: move

[slide]

# Linux命令分享
<small>演讲者：孙新杰 <br> http://freeyiyi.com/</small>

[slide]

## Curl

* 作用：文件传输
* 参数：`man curl`、`curl --help`、`curl --manual`
* 使用场景：...

[slide]

## 下载
* 通过ftp下载文件:

    curl -u username:pwd -O http_path

    curl -O ftp://username:pwd@ip:port/path

* 分断下载：

    curl -r 0-100 -o file.part1 file_path --> cat file.part* >filename.extend

* curl -o file_name url | curl -O file_path
* 进度条：

    curl -s -o filename url | curl -# -O url


[slide]
## 上传
* 通过ftp上传:

    curl -T file ftp://username:pwd@ip:port/path

* 断点续传：

    curl -C -O filename


[slide]
## 登录
* 模拟登录，保存cookie信息

    curl -c [tmp_path] -F log=[username] -F pwd=[pwd] login_url

* 模拟登录，保存头信息

    curl -D [tmp_path] -F log=[username] -F pwd=[pwd] login_url

* 使用cookie

    curl -b [cookie_file] url


[slide]

## 其他

* 伪造来源

    curl -e referer_url target_url

* 用代理

    curl -x [ip]:[port] -o filename url


[slide]

## cut

* 从一个文件或者流中提取文本列
* 语法 cut [-bcfd]
* 参数：
    * -b: 字节
    * -c: 字符
    * -f: 域，第几段的意思
    * -d: 后面接分隔字符,与 -f 一起使用
    * -n: 不要将多字节字符拆开
[slide]
## 实例
* 以`:`分隔，取第五段：cut -d ':' -f 5
* `5`、`1-5`、`3-`、`1-3,5`
* cat cut_ch.txt |cut -nb 1,2,3
* 提取每一行的第三个字节: last|cut -b 3

[slide]
##sort
* 对`file[s]`行排序，并将结果写到标准输出
* 语法：sort [-fbMnrtuk] [file or stdin]
* 参数：
    * f: 忽略大小写
    * b: 忽略最前面的空格
    * M: 以月份癿名字杢排序
    * n: 使用『纯数字』进行排序(默认是文本)
    * r: 反向
    * t: 分隔符，默认是用 [tab]
    * u: 相同的数据中，仅出现一行代表
    * k: 以那个区间 (field) 来进行排序的意思


[slide]
## whereis
* 查找目录，建立数据库，快
* 语法whereis [-bmsu] file
    - b: 二进制
    - m: manual
    - s: source
    - other

[slide]
## locate
* 快速查找文件、文件夹
* 语法：locate keyword
* `updatedb`手工建立、更新数据库

[slide]
## find
* 搜寻硬盘,高级查找文件、文件夹，强大，慢
* 语法: find [path] [-args] [reg]
* 参数：[-name] 名称 [-perm] 权限 [-user] 所有者 [-group] 用户组 [-ctime] 创建时间 [-type] 文件类型 [-size] 文件大小

[slide]
## 实例
* find . -name \*keyword\*
* find / -name \*.conf
* find / -perm 777
* find / -type d
* find . -name "a*" -exec ls -l {}  \;    立即重启

[slide]
## uniq
* 去重，重复行必须是相邻的，经常和sort一起使用
* 语法：uniq [-icu]
* 参数：
    - -i：忽略大小写
    - -c：计数
    - -u：只显示不重复的行

[slide]
## 实例
* cat uniq.file|sort|uniq
* sort uniq.file | uniq -c

[grep]
## grep
* 将模式匹配的行写入标准输出中
* 语法：grep [exp] [path]
* 参数：`\(..\)、 \<、 \>、 x\{m\}、 x\{m,\}、 x\{m,n\}、 \w、 \W、 \b`


<!-- [slide]
##正则表达式：
* 元字符集：`^ $ . * [] [^]`
*
 -->

