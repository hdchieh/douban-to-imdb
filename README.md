更简单的方式是使用 `豆坟` 与 `IMDB List Importer` in greasyfork.


---

# douban-to-imdb

&ensp;&ensp;&ensp;&ensp;最近发现 Apple Tv上的 Infuse里可以关联 Trakt显示出自己的观看记录，但是我之前的观看记录全部在豆瓣里，所以 Trakt上显示的数字全部是 0，之后的观看 Infuse会自动同步，但是之前的观看就需要自己导入一下了。找了 GitHub之后决定先把豆瓣的评分导入到 IMDB，然后再把 IMDB导出的 CSV文件用别人写好的另外一个程序导入到 Trakt。豆瓣导入 IMDB时决定只导入评分不导入任何评价，毕竟评价都是中文的评价，导入到 IMDB也没有多大意义，就让它们留在豆瓣吧。

## 需求

* [pyenv](https://github.com/pyenv/pyenv)
* [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv)
* [Chrome](https://www.google.com/chrome/)
* [chromedriver](https://chromedriver.chromium.org/downloads)（Selenium的Chrome驱动程序，如何安装请自行谷歌）

## 初始化

    $ pyenv install 3.8.0
    $ pyenv virtualenv 3.8.0 douban-to-imdb-env
    $ pyenv activate douban-to-imdb-env
    $ pip install -r requirements.txt
    
###### *・如果不使用虚拟环境，请参照 requirements.txt中的内容自行安装依赖包*
    
## 使用

&ensp;&ensp;&ensp;&ensp;程序分两步，第一步先将豆瓣评分导出到 CSV文件，第二步再将 CSV文件中的内容导入到 IMDB。

#### 导出豆瓣电影评分到 CSV文件

    $ python douban_to_csv.py chou96 20130901
    
*`[user_id]`为豆瓣的用户 ID，查找方法参见：[如何查找自己的豆瓣 ID](#如何查找自己的豆瓣-ID)*

*`[yyyymmdd]`为爬取的开始日期，即大于（不包含）该日期的电影评分才会被爬取*

#### 导入电影评分到 IMDB

&ensp;&ensp;&ensp;&ensp;由于导入 IMDB需要登录，所以此过程程序会自动打开浏览器，等待用户自行登录 IMDB账号。登录成功后浏览器会自动查找电影并进行打分，这是正常的程序操作，并不是闹鬼，请勿惊慌。 👻👻👻

    $ python csv_to_imdb.py [unmark/-2/-1/0/1/2]
    
###### *・参数如果为 unmark时，则会清除CSV文件中电影对应的 IMDB中的评分*

###### *・参数如果为数字时，则打分时会加上传入的数值，参数范围为 ±2分*

###### *・参数为空时，默认打分 -1分（由于豆瓣打分粒度太大，导致本人评分结果往往会比实际感受稍高 1分左右）*

###### *例：（无参数时）*
  
###### *&ensp;&ensp;&ensp;豆瓣评五星：IMDB打 9分 （考虑到实际上能满分的电影比较少）*
  
###### *&ensp;&ensp;&ensp;豆瓣评一星：IMDB打 1分 （考虑到你既然都打一星了肯定是这电影烂到你恨不得给 0分）*

#### 导入 IMDB里标记的电影到 Trakt的已观看记录

[TraktRater](https://github.com/damienhaynes/TraktRater/releases) 是一个 Windows应用程序，按照里面的说明文字即可成功导入 IMDB记录到 Trakt。

## 运行截图

#### 导出 CSV

&ensp;&ensp;&ensp;&ensp;我也是写这个程序的时候才发现，一些包含情色或者暴力内容的电影，在不登录豆瓣账号的情况下打开对应条目会显示访问的页面不存在，如：罗马帝国艳情史，树大招风等，这些条目请自行手工标记打分。还有就是一些条目在豆瓣的条目页里没有对应的 IMDB条目，应该是本来在 IMDB的数据库中就没有，如：极限挑战第五季等，这种条目就没有办法了。

![image](https://github.com/fisheepx/douban-to-imdb/blob/master/screenshots/douban-to-csv1.png)

![image](https://github.com/fisheepx/douban-to-imdb/blob/master/screenshots/douban-to-csv2.png)

#### 导入 IMDB

![image](https://github.com/fisheepx/douban-to-imdb/blob/master/screenshots/csv-to-imdb2.png)

![image](https://github.com/fisheepx/douban-to-imdb/blob/master/screenshots/csv-to-imdb1.png)

## 其它

* #### 如何查找自己的豆瓣 ID

&ensp;&ensp;&ensp;&ensp; 先登录自己的豆瓣账号，然后点击右上角的名字，打开[个人主页](https://www.douban.com/mine/)，就在跳转到的URL里：https://www.douban.com/people/[这里的数字就是你的user_id]/

## 感谢

* 导出豆瓣电影评分部分的逻辑直接复制了 @IvanWoo的 [douban-exporter-lite](https://github.com/IvanWoo/douban-exporter-lite)，在此感谢。

* 导入到Trakt部分使用了 @damienhaynes的 [TraktRater](https://github.com/damienhaynes/TraktRater)，在此感谢。
~~

* 导出豆瓣电影评分部分的逻辑直接复制了 @IvanWoo的 [douban-exporter-lite](https://github.com/IvanWoo/douban-exporter-lite)，在此感谢。

* 导入到Trakt部分使用了 @damienhaynes的 [TraktRater](https://github.com/damienhaynes/TraktRater)，在此感谢。
