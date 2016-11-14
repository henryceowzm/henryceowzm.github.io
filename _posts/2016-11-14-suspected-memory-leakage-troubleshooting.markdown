---
layout:     post
title:      "Suspected memory leakage investigation"
date:       2016-11-14 12:00:00
author:     "henry"
header-img: "img/post-bg-05.jpg"
---

<h2 class="section-heading">Problem description</h2>
<p>Memory usage keeps going up, suspected memory leakage in the program.</p>

<h2 class="section-heading">Initial analysis</h2>
<p>From VisualVM</p>
<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-1.png" alt="suspected memory leakage">
</a>
<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-3.png" alt="suspected memory leakage">
</a>

<p>From MemoryAnalyzer</p>
<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-10.png" alt="suspected memory leakage">
</a>
<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-11.png" alt="suspected memory leakage">
</a>
<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-2.png" alt="suspected memory leakage">
</a>

<p>So, the problem should between X and Y. A litter backgroud here: X provides a webservice interface to Y which will be used to send data to X.</p>

<h2 class="section-heading">At the crossroad</h2>

<p>Sometimes, even google can’t lead you to a solution of a problem. So start reading the source code related to these parts.</p>

<blockquote>The dreams of yesterday are the hopes of today and the reality of tomorrow. Science has not yet mastered prophecy. We predict too much for the next year and yet far too little for the next ten.</blockquote>

<p>Spaceflights cannot be stopped. This is not the work of any one man or even a group of men. It is a historical process which mankind is carrying out in accordance with the natural laws of human development.</p>

<h2 class="section-heading">Reaching for the Stars</h2>

<p>As we got further and further away, it [the Earth] diminished in size. Finally it shrank to the size of a marble, the most beautiful you can imagine. That beautiful, warm, living object looked so fragile, so delicate, that if you touched it with a finger it would crumble and fall apart. Seeing this has to change a man.</p>

<a href="#">
    <img src="{{ site.baseurl }}/img/post-sample-image.jpg" alt="Post Sample Image">
</a>
<span class="caption text-muted">To go places and do things that have never been done before – that’s what living is all about.</span>

<p>Space, the final frontier. These are the voyages of the Starship Enterprise. Its five-year mission: to explore strange new worlds, to seek out new life and new civilizations, to boldly go where no man has gone before.</p>

<p>As I stand out here in the wonders of the unknown at Hadley, I sort of realize there’s a fundamental truth to our nature, Man must explore, and this is exploration at its greatest.</p>

<p>Placeholder text by <a href="http://spaceipsum.com/">Space Ipsum</a>. Photographs by <a href="https://www.flickr.com/photos/nasacommons/">NASA on The Commons</a>.</p>