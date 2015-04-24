---
layout: post
title: "CCTV Security with the Raspberry Pi"
description: "Building a scalable, high-resolution video system on low-cost hardware."
category: articles
tags: [raspberry, pi, camera, cctv, video, recording]
image:
  feature: circuit-board.jpg
---

Since having our house burgled a couple of months ago, I wanted to setup a camera to capture footage outside our front door.  There are plenty of commercial CCTV systems available but so many seemed to be plagued with very poor video quality or awkward proprietary formats and interfaces.

If you want to build your own system, the Raspberry Pi is a great choice given its very low-power consumption and excellent onboard 1080p h264 encoder.

There already exists a CCTV monitoring system for the Raspberry Pi called [motionPie](https://github.com/ccrisan/motionPie), which is an excellent purpose-built distro and a great starting point for those who don't want to tweak too much.

For my system I wanted good-quality video and to have motion detection running, which was something motionPie was not able to do.  Also, my camera is placed in my doorway so I need to transmit and store the footage immediately to ensure anyone who tries to smash up the camera will still leave me with the footage of them doing it.

My full setup is described below and all code is freely available on GitHub: [PiMonitor Repository](https://github.com/adamkingenergy/pimonitor)

### Camera Module

For the camera unit I would recommend:

- [Nwazet Pi Camera Box](http://www.modmypi.com/raspberry-pi/camera/nwazet-pi-camera-box-bundle-case,-lens-and-wall-mount-b-plus) - a really sturdy box specifically designed to house a camera unit.
- [Raspberry Pi NoIR Camera](https://www.raspberrypi.org/products/pi-noir-camera/) - the variant of the camera module with no infra-red filter, enhancing the night-time perfomance (particularly if you get an infra-red illuminator).
- [USB WiFi Adapter](http://www.modmypi.com/raspberry-pi/accessories/wifi-dongles/wifi-dongle-nano-usb)

<figure>
	<img src="http://farm9.staticflickr.com/8426/7758832526_cc8f681e48_c.jpg">
	<figcaption>Installed camera module</figcaption>
</figure>

On the software side, the camera only needs the picamera and pyzmq modules.

{% highlight bash linenos %}
$ sudo apt-get install python-picamera
$ sudo apt-get install python-zmq
{% endhighlight %}

You can then install the source on the camera and set to run as a daemon:

{% highlight bash linenos %}
$ cd /usr/local/bin/
$ git clone git@github.com:adamkingenergy/pimonitor.git
$ sudo ln -s /usr/local/bin/pimonitor/daemon/pimonitorcam.sh /etc/init.d/pimonitorcam
$ sudo update-rc.d pimonitorcam defaults
{% endhighlight %} 

### Recorder Modules

For the recorder unit you can use pretty much any machine with reasonable storage capacity.  I re-used a Raspberry Pi I have running as a RAID NAS server, adding an extra disk for the CCTV.

The recorder does require ffmpeg to wrap the h264 stream into MP4, and this was an awkward build.  Using slightly modified instructions from [this blog](http://www.jeffreythompson.org/blog/2014/11/13/installing-ffmpeg-for-raspberry-pi/), I got it working.

First installing the x264 library:

{% highlight bash linenos %}
cd /usr/src
git clone git://git.videolan.org/x264
cd x264
./configure --host=arm-unknown-linux-gnueabi --enable-static --disable-opencl
make
sudo make install
{% endhighlight %} 

Then performing the build of ffmpeg itself:

{% highlight bash linenos %}
git clone git://source.ffmpeg.org/ffmpeg.git ffmpeg 
cd ffmpeg
sudo ./configure --arch=armel --target-os=linux --enable-gpl --enable-libx264 --enable-nonfree
make
sudo make install
{% endhighlight %} 


Once it was done, I got the daemon setup and everything should be running automatically:

{% highlight bash linenos %}
$ cd /usr/local/bin/
$ git clone git@github.com:adamkingenergy/pimonitor.git
$ sudo ln -s /usr/local/bin/pimonitor/daemon/pimonitorrec.sh /etc/init.d/pimonitorcam
$ sudo update-rc.d pimonitorrec defaults
$ sudo /etc/init.d/pimonitorrec start
{% endhighlight %} 


### Monitor Modules

The monitor unit can be any machine with wxPython and ØMQ installed (which should not preclude Windows if you like).  Sticking with tradition, I chose a Raspberry Pi for this part and picked up a specially designed case and screen.

- [Tontec 3.5" TFT and case](http://www.amazon.co.uk/dp/B00OFLKPG4)
- [USB WiFi Adapter](http://www.modmypi.com/raspberry-pi/accessories/wifi-dongles/wifi-dongle-nano-usb)

<figure>
	<img src="http://farm9.staticflickr.com/8426/7758832526_cc8f681e48_c.jpg">
	<figcaption>Installed camera module</figcaption>
</figure>

As you can see, this leads to a pretty nice little device which now lives at the side of our bed.  When I have some more time I will integrate the motion detection into this so it wakes the device and displays images only when something of interest happens.


### Source Code

As mentioned at the start, all source code is available [here on GitHub](https://github.com/adamkingenergy/pimonitor).  If you want to add any features, please go ahead and fork the repo, make your changes and then create a pull-request.





