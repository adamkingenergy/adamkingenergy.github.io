---
layout: post
title: "Distributed CCTV using the Raspberry Pi"
description: "Building a high-resolution and scalable system on low-cost hardware."
category: articles
tags: [raspberry, pi, camera, cctv, video, recording]
image:
  feature: circuit-board.jpg
---

Since being robbed a couple of months ago, I had been looking to install a system to capture a decent shot of whoever might be out there jimmying our locks at 04:00 AM.  There are plenty of commercial systems available but many fall down on various aspects of cost, features, resolution or intrusiveness of installation.  In the free-software world, [motionPie](https://github.com/ccrisan/motionPie) is an excellent tailored-distro which lets you get a camera up and running in minutes.  For my purposes, motionPie seemed only able to capture video at relatively low-quality and was therefore not worth keeping, so put together a small monitor project which uses the Raspberry Pi GPU's H264 encoding power.



[Raspberry Pi GPIO pinout](http://pi.gadgetoid.com/pinout)




### Single Image Figure

<figure>
	<img src="http://farm9.staticflickr.com/8426/7758832526_cc8f681e48_c.jpg">
	<figcaption>Morning Fog Emerging From Trees by A Guy Taking Pictures, on Flickr</figcaption>
</figure>

{% highlight html linenos %}
<figure>
	<img src="/images/image-filename-1.jpg">
	<figcaption>Caption describing these two images.</figcaption>
</figure>
{% endhighlight %}
