---
layout:     post
title:      "Install ubuntu in virtualbox on win7 host"
date:       2017-02-14 12:00:00
author:     "henry"
header-img: "img/post-bg-05.jpg"
---

<h2 class="section-heading">Install virtualbox on win7 host</h2>
<p><a href="https://www.virtualbox.org/">virtualbox home page</a></p>

<h2 class="section-heading">Enable virtual technology</h2>
<p>Before --> Only 32 bit options</p>
<a href="#">
    <img src="{{ site.baseurl }}/img/install-ubuntu-in-virtualbox-on-win7-host/32-bit-option-only.png" alt="32-bit-option-only">
</a>
<p><a href="http://www.fixedbyvonnie.com/2014/11/virtualbox-showing-32-bit-guest-versions-64-bit-host-os/#.WLTqqvIrRzw">This is the crucial step, after which we can see and choose the 64 bit options</a></p>
<a href="#">
    <img src="{{ site.baseurl }}/img/install-ubuntu-in-virtualbox-on-win7-host/enable-virtual-technology.jpg" alt="enable-virtual-technology">
</a>
<p>After --> 64 bit options are available</p>
<a href="#">
    <img src="{{ site.baseurl }}/img/install-ubuntu-in-virtualbox-on-win7-host/with-64-bit-option.png" alt="with-64-bit-option">
</a>

<h2 class="section-heading">Install ubuntu in virtualbox</h2>
<p><a href="http://www.wikihow.com/Install-Ubuntu-on-VirtualBox">reference guide</a></p>
<p><a href="http://www.psychocats.net/ubuntu/virtualbox">reference guide</a></p>

<h2 class="section-heading">Enable share folder between win7 and ubuntu</h2>
<p>Setting on virtualbox</p>
<a href="#">
    <img src="{{ site.baseurl }}/img/install-ubuntu-in-virtualbox-on-win7-host/setting-on-virtualbox.png" alt="setting-on-virtualbox">
</a>
<p>Mount to ubuntu</p>
{% highlight Bash %}
# sudo mount -t vboxsf maven-repo /mnt/maven-repo
{% endhighlight %}

<h2 class="section-heading">Enable auto mount on start</h2>
<a href="#">
    <img src="{{ site.baseurl }}/img/install-ubuntu-in-virtualbox-on-win7-host/enable-auto-mount-on-start.png" alt="enable-auto-mount-on-start">
</a>
<a href="#">
    <img src="{{ site.baseurl }}/img/install-ubuntu-in-virtualbox-on-win7-host/enable-auto-mount-on-start-result.png" alt="enable-auto-mount-on-start-result">
</a>

<h2 class="section-heading">Edit maven settings.xml</h2>
<a href="#">
    <img src="{{ site.baseurl }}/img/install-ubuntu-in-virtualbox-on-win7-host/edit-maven-setting.png" alt="edit-maven-setting">
</a>
