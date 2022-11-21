---
layout: post
title:  Kali in Browser
subtitle: Learning Docker Containers
gh-repo: bijjybox/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
---

![](https://miro.medium.com/max/828/0*nJtCTW9G3UqCLUzn.jpg)
A lot of time is consumed in installing tools for pentesting in other distros of linux like Ubuntu whereas Kali Linux comes preinstalled with that. But I don’t just want to rip off this ubuntu OS and immediately switch to Kali (It took days to customize everything on Ubuntu. I am clingy, I can’t let go off all the favorite addons!)

So I found an alternative which can fulfil both ways.
{: .box-warning}
You can now run **Kali Linux**, one of the popular and advanced Linux distribution specially designed for penetration testing and ethical hacking, directly on your Web browser, regardless of any operating system you use. Meet **KaliBrowser**, a new project developed by Security Analyst **Mr.Jerry Gamblin** for ethical hackers.

Let’s take a moment to thanks Mr Jerry for his amazing work 🎆

If you are familiar with Docker already, this is going to be easy breezy :P

If not! Check out this official documentation [**here](https://docs.docker.com/get-started/)!** But a brief introduction of great Docker is required here as well.
{: .box-note}
**Docker** is a fast, lightweight and OS level virtualization technology for developers and system administrators who wants to build an application with all of required dependencies, and ship it out as only one package. Unlike other Virtualization methods, such as VMWare, Xen and VirtualBox, there is no need of separate guest operating system for each virtual machine. All Docker containers efficiently share the host operating system’s Kernel. Each container will run in an isolated userspace in the same operating system.
{: .box-note}
Docker containers will also run on any Linux variant. Let us say you’re working in Fedora, and I am using Ubuntu. We can still develop, share and distribute the Docker images with each other. You don’t have to worry about the OS, software, customized settings, or anything. We can continue the development as long as we have Docker installed in our host system. Simply put, it will work everywhere!

## Run KaliBrowser

KaliBrowser is actually a docker image built with [Kali Linux Docker](https://hub.docker.com/r/kalilinux/kali-linux-docker/), [OpenBox](http://openbox.org/wiki/Main_Page), and [NoVNC](https://github.com/kanaka/noVNC) HTML5 VNC client. So, in order to run KaliBrowser, you need to install [**Docker](https://docs.docker.com/engine/install/)** and [**Docker CE](https://docs.docker.com/compose/install/)** first.

Once installed, verify that **Docker CE** is installed properly by running the hello-world image.
```sh
$ sudo docker run hello-world
```
The output will be similar to this-

![](https://miro.medium.com/max/828/1*ozOwr-1t_QoGCPJAzg6cmg.png)

**Then, start and enable the Docker service as shown below.**
```sh
$ sudo systemctl start docker
$ sudo systemctl enable docker
```

### Download and run KaliBrowser

Run the following command to download and KaliBroswer docker image.
```sh
$ sudo docker run -d -t -i -p 6080:6080 jgamblin/kalibrowser
```

The output will be like this-

![](https://miro.medium.com/max/828/1*SJ6KhDrUoeIaV_0O889QEw.png)

Once the download is complete, open your web browser and type: [**http://localhost:6080](http://localhost:6080)** or [**http://IP-Address:6080/](http://IP-Address:6080/)** in the address bar.

That’s it. Start working with Kali Linux right from the web browser.

Here’s how it started in my browser -

![](https://miro.medium.com/max/828/1*0j1DZTf1YVts3d42brdaMg.png)
{: .box-note}
To open menu items, just right click on the empty space. A basic menu will appear.
{: .box-note}
To keep kalibrowser simple and fast, the developer has included only the base installation of Kali Linux. However, you can install additional tools if you want via command-line.

## Stop KaliBrowser

After working with Kali Linux, you can stop it as shown below.

First find the Kali Linux docker image id using command:
```sh
$ sudo docker ps -a
```

![](hhttps://miro.medium.com/max/828/1*xdfHxEP9pIOuZMhH5lTaqg.png)

Got the container id — *96d0dda1c82b*

Now we need to stop this container
```sh
$ sudo docker stop 96d0dda1c82b
```

And that’s it, now we can use all the tools of Kali without sacrificing our current OS :)
