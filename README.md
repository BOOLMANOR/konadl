此教程适用于Windows和Linux

如果您是普通用户并且希望用此工具来下载图片，那么此篇教程将适合您。

首先，克隆代码

$ git clone https://github.com/K4YT3X/konadl.git
$ cd konadl
然后安装 Python 依赖库

$ pip install -r requirements.txt
最后运行 konadl_cli.py

以下是一个简单的 KonaDL 运行配置。此配置会将10页的 safe，questionable 和 explicit 评级图片下载到 /tmp/konachan。

$ python3 konadl_cli.py -o /tmp/konachan -e -s -q -n 10
您也可以手动设定多线程下载线程的数量。大多数情况下，默认设定足矣
-c 10 将会启用10条页面爬虫线程，默认10条。
-d 20 将会启用20条下载器线程，默认20条。

$ python3 konadl_cli.py -o /tmp/konachan -e -s -q -n 10 -c 10 -d 20
您可以使用 --update 开关来更新已经下载完的图片库。设定会跟随上次下载。

$ python3 konadl_cli.py -o /tmp/konachan/ --update
完整用法如下:

usage: konadl_cli.py [-h] [-n PAGES] [-a] [-p PAGE] [-y] [-o STORAGE] [-u]
                     [-s] [-q] [-e] [-c CRAWLERS] [-d DOWNLOADERS] [-v]

optional arguments:
  -h, --help            show this help message and exit

Controls:
  -n PAGES, --pages PAGES
                        Number of pages to download
  -a, --all             Download all images
  -p PAGE, --page PAGE  Crawl a specific page
  -y, --yandere         Crawl Yande.re site
  -o STORAGE, --storage STORAGE
                        Storage directory
  -u, --update          Update new images

Ratings:
  -s, --safe            Include Safe rated images
  -q, --questionable    Include Questionable rated images
  -e, --explicit        Include Explicit rated images

Threading:
  -c CRAWLERS, --crawlers CRAWLERS
                        Number of post crawler threads
  -d DOWNLOADERS, --downloaders DOWNLOADERS
                        Number of downloader threads

Extra:
  -v, --version         Show KonaDL version and exit
  
  
  以下是 libkonadl 简单使用介绍。

from libkonadl import konadl  # 导入konadl库

""" 爬虫示例

最基本地从 konachan.com 下载图片。
"""
kona = konadl()  # 创建爬虫对象

# 设置存储位置
# 注意在尾部有一个 “/”
kona.storage = '/tmp/konachan/'
if not os.path.isdir(kona.storage):  # 如果目标路径不存在则退出
    print('Error: storage directory not found')
    exit(1)

# 如果您想从 yande.re 下载图片则设置此项为 True
kona.yandere = False

# 根据评级下载图片
kona.safe = True            # 包含 Safe 评级别图片
kona.questionable = False   # 包含 Questionable 评级别图片
kona.explicit = False       # 包含 Explicit 评级别图片

# 设置页面爬虫和下载器线程数量
kona.post_crawler_threads_amount = 10
kona.downloader_threads_amount = 20
kona.pages = 3  # 爬前三页
if os.path.isfile(kona.progress_file):
    print('Loading downloading progress')
    kona.load_progress = True

# 执行
kona.crawl()
print('\nMain thread exited without errors')
print('Time taken: {} seconds'.format(round((time.time() - kona.begin_time), 5)))
如果您想下载所有页面

kona.crawl_all_pages()
您可以用以下格式覆写 method

class konadl_avalon(konadl):
    """ Overwrite original methods for better
    appearance and readability.
    """

    def print_retrieval(self, url):
        avalon.dbgInfo("Retrieving: {}".format(url))

    def print_crawling_page(self, page):
        avalon.info('Crawling page: {}{}#{}'.format(avalon.FM.BD, avalon.FG.W, page))

    def print_thread_exit(self, name):
        avalon.dbgInfo('[libkonadl] {} thread exiting'.format(name))

# 从覆写过的类创建对象
kona = konadl_avalon()
