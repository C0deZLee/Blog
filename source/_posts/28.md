title: AppleScript Based Timer to Awake Shanbay.com
date: 2016-7-26  21:29:50
categories:
- Tech
tags:
- AppleScript
---
众所周知AppleScript是Mac系统上一个非常强大的功能，通过简单几行script可以达到很多强大的效果。
正好最近在准备GRE，给自己定了每天都要背单词的计划，可是每次忘记，于是就想到写一个AppleScript脚本来强制弹出Chrome窗口背单词，正好还可以上手练习一下。
<!-- more -->
为了达到每日定时弹出的目的，首先要获取当日时间：
{% codeblock %}
current date
{% endcodeblock %}
这行代码return的值是
{% codeblock %}
date "Monday, June 20, 2016 at 8:21:50 PM"
{% endcodeblock %}
明显不符合我的需求，然而
{% codeblock %}
time of(current date)
{% endcodeblock %}
会return一个int值，类似这样
{% codeblock %}
73329
{% endcodeblock %}
这个int值是从当日零点开始的以秒为单位的counter，因此就很方便了，我想要每天下午五点钟打开chrome，转到扇贝网背单词，因此下午五点钟就是**61200**.

{% codeblock %}
set currTime to time of (current date)
if currTime < 61200 then
	set waitTime to (61200 - currTime)
	log "Start Delay"
	delay waitTime
end if
{% endcodeblock %}
接下来就要处理Chrome启动的问题了，AppleScript对程序唤醒的支持还是比较好的，只要几行就搞定了

{% codeblock %}
tell application "Google Chrome"
	activate
	make new window
	open location "https://www.shanbay.com/bdc/review/"
	display dialog "It's time to perpare for GRE!"
end tell
{% endcodeblock %}

然后就基本上大功告成，输出成osascript，添加到启动项里就好啦。
