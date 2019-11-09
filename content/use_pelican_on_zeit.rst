使用ZEIT部署Pelican博客
###########################################

:date: 2019-11-09 11:22
:modified: 2019-11-09 11:22
:tags: ZEIT, blog, 教程
:category: 教程
:authors: IllusiveMan
:summary: 关于使用zeit.co部署Pelican博客的问题

ZEIT是一个静态页面托管服务，类似于Netlify和GitHub Page。看上它是因为免费、自带https、自带CDN，以及……页面比Netlify漂亮……

虽然Pelican可以正常运行在ZEIT，但很遗憾的是，ZEIT对Pelican并没有优化支持，而Hexo之类是支持的。没办法，谁让ZEIT是做js起家的呢……

ZEIT支持自动构建，也就是说，允许我们使用Pelican工程（托管在Git）并在每次push时自动调用命令构建静态页面，就像在GitHub Page使用Jekyll一样（GitHub不支持Pelican自动构建，要用就得只上传构建好的静态页面）。
为什么自动构建比手动构建后上传静态页面文件好呢？因为如果支持从GitHub自动构建，我们就可以脱离本地环境，直接在GitHub上面用WebIDE写文章了。毕竟在新设备重新配置环境挺烦的，我们要的只是写文章而已。

ZEIT官方给出了Pelican的构建示例：`zeit/now-examples <https://github.com/zeit/now-examples/tree/master/pelican>`_

和Netlify一样，ZEIT的构建也是依托于package.json的。官方的该文件内容如下：

.. code-block:: json

    {
    "name": "pelican",
    "version": "1.0.0",
    "private": true,
    "description": "A Pelican project, ready for deployment with ZEIT Now.",
    "main": "index.js",
    "scripts": {
        "build": "pelican ./ -o public -s publishconf.py"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"
    }

可以看到，build这条命令直接调用了Pelican。我平时都是用Makefile，里面会自动填写一些东西。如果直接使用官方的这个命令，会出现什么样的结果呢？我们来看一下Pelican的命令格式：

`pelican SOURCE_DIR -o OUTPUT_DIR -s CONF_FILE`

可以看到，ZEIT的示例中将源文件目录设置为./，也就是整个工程的根目录。这样做会导致很多无关文件被纳入构建，报一堆错误。Pelican一般是在根目录建立一个content文件夹，将文章的源文件存储在里面；而输出目录是public，这个不用改。尽管Pelican默认是用一个output目录，但ZEIT不像Netlify，它不允许用户自行制定静态页面的产出目录，而是默认使用public目录。即便我们在ZEIT查看整个工程，我们也会发现输出目录不会是我们提供的output目录。这个不用管。

综上，build命令应当是：`pelican content -o public -s publishconf.py`。
