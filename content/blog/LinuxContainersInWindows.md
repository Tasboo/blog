---
title: "Developing using Linux containers using Windows"
date: "2019-02-06T04:00:00+01:00"
draft: false
---

[Scott Hanselman](https://twitter.com/shanselman) had [a post](https://www.hanselman.com/blog/MovingAnASPNETCoreFromAzureAppServiceOnWindowsToLinuxByTestingInWSLAndDockerFirst.aspx) recently that piqued my interest. In that post, he goes through the process of moving a .net core app that is hosted in an Azure App Service for Windows instance to a Linux Container. I have very little experience doing development with containers, but I am familiar with .Net Core, Azure App Service, and to a lesser extent the WSL so I thought this is probably a good opportunity to start diving into development using Linux containers.

<h3>WSL Setup</h3>
I have explained my process for getting WSL2 setup in a previous post, so I won't repeat the things here I've already done, but I will go into the additional things I had to do for this particular endevour.

<h3>.Net Core 3.1</h3>
Installing .Net Core in Windows is something that I am very familiar with, but installing it in Linux is not. The easiest way to install it in Linux seems to be using apt. The process for installing things using apt in Linux is still akward for me.

To install .Net Core 3.1 using apt I had to navigate the following links:


<img src="/images/NetCore1.png" />
<img src="/images/NetCore2.png" />
<img src="/images/NetCore3.png" />

After that it brought me to microsoft docs page that goes through how to install .Net Core using apt in Linux. Just in case links for things break, here are the commands I ran in the Ubuntu terminal:

```
wget -q https://packages.microsoft.com/config/ubuntu/19.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```
```
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-3.1
```
```
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install aspnetcore-runtime-3.1
```

I was able to run those commands without any issues. The last set of commands for installing the runtime was not necessary as the SDK installed the runtime already.

<h3>Setting Up My Project</h3>
I decided to just use a blank webapi project for my dotnet project. This would keep the scope of what I was trying to accomplish to a smaller size as I wouldn't have to worry about connecting to anything from API at this point. I ran the following command in the Ubuntu terminal to create the project:

```
dotnet new webapi -n SomeAPI
```

<h3>Installing Docker Desktop</h3>
The next step I needed to do was to install [Docker Desktop](https://docs.docker.com/docker-for-windows/wsl-tech-preview/). Something to mention is that to get Docker Desktop installed, you need to have a recent Windows Insider preview build 19018 or higher installed with the [WSL2 feature enabled](https://docs.microsoft.com/en-us/windows/wsl/wsl2-install). After that, install a Linux Ubuntu distro from the Windows Store. This step was relatively painless. Do remember to set version 2 as the default WSL version before you install Docker Desktop. If you have any distros set to WSL1, set them to 2. After that is all done, you should be able to configure Docker Desktop with the following settings. If the WSL2 feature is not available, I'd recommend uninstalling Docker Desktop and starting the process over again.

<img src="/images/DockerDesktopGeneral.jpg" />
<img src="/images/DockerDesktopResources.png" />

<h3>Dockerfile</h3>
This is where my post really starts to diverge from Scott's post, for a couple of reasons:
1. I do not have any tests to run, so the test running part is not relavant. What I want to do instead is get my API running locally so that I can browse to it via the Chrome instance on Windows.
2. I am not using Linux ARM, I am using Linux AMD64. I will need to set my dockerfile to reflect this.

In summary, my dockerfile is going to have to be different than Scott's. This part took me a while to get working right since I'm not very familiar with Docker. I found (a useful post)[https://softchris.github.io/pages/dotnet-dockerize.html#create-a-dockerfile] to figure out what I needed to change from Scott's docker file. Here is [my API and dockerfile](https://github.com/Tasboo/SomeAPI) if you want to see what I did to get it working. I have also documented the docker commands that I needed to run to create the images, build the container, and run the container in the Readme.md file of that repo. The biggest changes I needed to make was targeting AMD64 and the last few lines.

After I had my dockerfile ready, I needed to run this command to run the container:

```
sudo docker run -d -p 8080:80 --name myapi someapi
```

After running that I was able to browse to http://localhost:8080/WeatherForecast in my Windows Chrome browser and recieve the expected response from the API running in the Linux container.
