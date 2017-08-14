title: LionPath Course Spider
categories:
  - Tech
date: 2016-05-31 01:40:53
tags:
  - Python
  - Scrapy
---
爬虫的Github Repo： https://github.com/C0deZLee/CourseSpider

-------
今年学校十分令人无语地把本来挺好用的eLion课程管理系统改成了一个叫LionPath的脑残系统，导致选课时查询课程变成了一件彻彻底底令人崩溃的事情。
于是我把封藏已久的Scrapy挖了出来，决定自己把课程全部爬出来，做一个第三方的课程查询网站。
<!-- more -->
首先第一个阻碍是查询课程需要登录，不过好在LionPath提供了一个[public](https://public.lionpath.psu.edu/psc/CSPRD_2/EMPLOYEE/HRMS/c/PE_TE031.CLASS_SEARCH.GBL)页面，供父母或其他没有学校账号的人查询课程，同时也成为了我可以利用的漏洞。
然而仔细阅读源码之后我发现了一个令人崩溃的事实，LionPath是一个用Pure JavaScript写出来的单！页！面！应！用！他将整个页面所有的HTML整个打包，用String的格式通过一个自定义的Loader载入，实现了我见过的史上最奇葩的Single Page Web App逻辑。
因为LionPath全程使用Js动态载入的缘故，单单使用Scrapy就不太合时宜了，因为Scrapy没有处理Js的能力，所以必须配合selenium提供的webdriver爬内容，缺点就是速度上会慢很多。
Scrapy这个爬虫库的定制性很高，它分三个部分 **Spiders**,**Pipelines** 和 **Items**.
首先 **Items** 定义了你爬下来的数据要以什么样的格式存储，**Spiders** 部分负责爬虫逻辑，最后 **Pipelines** 负责后续加工你从Spiders部分拿到的raw data，存成Items里定义的格式。

比如说这次要爬的是课程数据，我们就可以讲Items定义成以下格式：
{% codeblock %}
class CourseItem(scrapy.Item):
    name = scrapy.Field()
    fullName = scrapy.Field()
    description = scrapy.Field()
    number = scrapy.Field()
    time = scrapy.Field()
    section = scrapy.Field()
    instructor1 = scrapy.Field()
    instructor2 = scrapy.Field()
    room = scrapy.Field()
    unit = scrapy.Field()
    capacity = scrapy.Field()
    waitlist = scrapy.Field()
    enrolled = scrapy.Field()
    waitlistEnrolled = scrapy.Field()
    status = scrapy.Field()
    major = scrapy.Field()
    classType = scrapy.Field()
    notes = scrapy.Field()
{% endcodeblock %}

算了，懒得写了，想要源码的，开头就有。
