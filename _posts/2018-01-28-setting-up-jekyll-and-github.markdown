---
layout: post
title:  "Setting Up Jekyll and Github"
date:   2018-01-29 16:31:01 -0800
categories: jekyll update
---


In this section, I will explain how I setup this blog using Jekyll as a static site generator and hosting it up to GitHub as well as using a custom domain for my own blog.

First of all, getting Jekyll installed is what you want to do and it should only take a few minutes.

## What you need to install Jekyll

Since I am using Windows 10 Anniversary Update, the easiest way to run Jekyll is by [installing](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide) the new Bash on Ubuntu on Windows.

Here is how you have [Bash on Ubuntu on Windows](https://www.windowscentral.com/how-install-bash-shell-command-line-windows-10) enabled.

Open a new Command Prompt / PowerShell instance from your Windows Explorer by using `Shift + Right Click`, and then type the following:

```sh
bash
```


![Shift + Right Click]({{ "/assets/img/cap01.PNG" | absolute_url }})

![bash command]({{ "/assets/img/cap02.PNG" | absolute_url }})

First let's make sure all our packages / repositories are up to date. In your Bash instance, type this:

```sh
sudo apt-get update -y && sudo apt-get upgrade -y
```
Next we should install Ruby. To do this we will use a repository from [BrightBox](https://www.brightbox.com/docs/ruby/ubuntu/), which hosts optimized versions of Ruby for Ubuntu.

```sh
sudo apt-add-repository ppa:brightbox/ruby-ng
sudo apt-get update
sudo apt-get install ruby2.3 ruby2.3-dev build-essential
```

Next let's update our Ruby gems:

```sh
sudo gem update
```

After making sure all of our Ruby and Gem installation are up to date, now all that is left to do is install Jekyll and Bundler.

```sh
sudo gem install jekyll bundler
```

Check if Jekyll installed properly by running:

```sh
jekyll -v
```
![jekyll installed]({{ "/assets/img/cap03.PNG" | absolute_url }})

**And that's it!**

**Note:** Bash on Ubuntu on Windows is still under development, so you may run into issues.


## Starting to set up the Project

I will start to make a project named `blog` and I will need to make a folder by the name of it. Jekyll can do it for you, and it will generate a folder structure complete with the Jekyll's necessity file. So, just run:

```sh
sudo jekyll new blog
```
![Project Command]({{ "/assets/img/cap04.PNG" | absolute_url }})

![Project Setup]({{ "/assets/img/cap05.PNG" | absolute_url }})

![Project Folder]({{ "/assets/img/cap06.PNG" | absolute_url }})

**Note:** You can make sure time management is working properly by inspecting your `_posts` folder. You should see a markdown file with the current date in the filename.

