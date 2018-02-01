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

### Creating the Post

Here is the most exciting part. You just need to add your post content by modifying all of the stuff in `_posts` folder. The post that you are making can be in HTML file or a .md [Markdown](https://daringfireball.net/projects/markdown/) file. Using [Markdown](https://daringfireball.net/projects/markdown/) is probably the most simple one, since it is just contains a plain regular text with a very little coding format.

#### Creating Post Files

To create a new post, all you need to do is create a file in the `_posts`
directory. However, the way that you name the files in this folder is VERY important, since Jekyll requires blog post files to be named according to the following format:

```
YEAR-MONTH-DAY-title.md
```

`YEAR` is a four-digit number,
`MONTH` and `DAY` are both two-digit numbers,
`md` is the file extension representing the format used in the file. 

For example, the following are examples of valid post filenames:

```
2017-12-31-new-years-eve-is-awesome.md
2017-12-25-christmas-day-is-cold.md
```
#### Front Matter

Now, going inside the file itself, every `.md` file that you created must follow a [YAML](http://yaml.org/) front matter block. It must be the first thing in the file
and must take the form of valid YAML set between triple-dashed lines. Here is a
basic example:

```yaml
---
layout: post
title: Blogging Like a Hacker
---
```

Between these triple-dashed lines, you can set predefined variables (see below
for a reference) or even create custom ones of your own. These variables will
then be available to you to access using Liquid tags both further down in the
file and also in any layouts or includes that the page or post in question
relies on.

#### Global Variables

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Variable</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p><code>layout</code></p>
      </td>
      <td>
        <p>

          If set, this specifies the layout file to use. Use the layout file
          name without the file extension. Layout files must be placed in the
          <code>_layouts</code> directory.

        </p>
        <ul>
          <li>
            Using <code>null</code> will produce a file without using a layout
            file. However this is overridden if the file is a post/document and has a
            layout defined in the <a href="../configuration/#front-matter-defaults">
            frontmatter defaults</a>.
          </li>
          <li>
            Starting from version 3.5.0, using <code>none</code> in a post/document will
            produce a file without using a layout file regardless of frontmatter defaults.
            Using <code>none</code> in a page, however, will cause Jekyll to attempt to
            use a layout named "none".
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>permalink</code></p>
      </td>
      <td>
        <p>

          If you need your processed blog post URLs to be something other than
          the site-wide style (default <code>/year/month/day/title.html</code>), then you can set
          this variable and it will be used as the final URL.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>published</code></p>
      </td>
      <td>
        <p>
          Set to false if you don’t want a specific post to show up when the
          site is generated.
        </p>
      </td>
    </tr>
  </tbody>
</table>
</div>

#### Variables for Posts

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Variable</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p><code>date</code></p>
      </td>
      <td>
        <p>
          A date here overrides the date from the name of the post. This can be
          used to ensure correct sorting of posts. A date is specified in the
          format <code>YYYY-MM-DD HH:MM:SS +/-TTTT</code>; hours, minutes, seconds, and timezone offset
          are optional.
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>category</code></p>
        <p><code>categories</code></p>
      </td>
      <td>
        <p>

          Instead of placing posts inside of folders, you can specify one or
          more categories that the post belongs to. When the site is generated
          the post will act as though it had been set with these categories
          normally. Categories (plural key) can be specified as a <a
          href="https://en.wikipedia.org/wiki/YAML#Basic_components">YAML list</a> or a
          space-separated string.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>tags</code></p>
      </td>
      <td>
        <p>

          Similar to categories, one or multiple tags can be added to a post.
          Also like categories, tags can be specified as a <a
          href="https://en.wikipedia.org/wiki/YAML#Basic_components">YAML list</a> or a
          space-separated string.

        </p>
      </td>
    </tr>
  </tbody>
</table>
</div>

#### Custom Variables

Any variables in the front matter that are not predefined are mixed into the
data that is sent to the Liquid templating engine during the conversion. For
instance, if you set a title, you can use that in your layout to set the page
title:

```liquid
<!DOCTYPE HTML>
<html>
  <head>
    <title>{% raw %}{{ page.title }}{% endraw %}</title>
  </head>
  <body>
    …
```

### Config your _config.yml

This config file is meant for settings that affect your whole blog, values which you are expected to set up once and rarely edit after that. 

**Note:** For technical reasons, this file is *NOT* reloaded automatically when you use the command 'jekyll serve'. If you change this file, please restart the server process.

Your default `_config.yml` that was generated from `jekyll new <path>` should have this common information:

```config
title: Your awesome title
email: your-email@example.com
description: >- # this means to ignore newlines until "baseurl:"
  Write an awesome description for your new site here. You can edit this
  line in _config.yml. It will appear in your document head meta (for
  Google search results) and in your feed.xml site description.
baseurl: "" # the subpath of your site, e.g. /blog
url: "" # the base hostname & protocol for your site, e.g. http://example.com
twitter_username: jekyllrb
github_username:  jekyll

# Build settings
markdown: kramdown
theme: minima
plugins:
  - jekyll-feed

```
You can modify and change some data, such as `title`, `email`, `github_username`.

And, since I am gonna use image assets into my blog, we will need to add some line of code into the file. Put this code at the end of the row:

```image
defaults:
  - scope:
      path: "assets/img"
    values:
      image: true
```
`path: "assets/img"` is the path of the folder of your image from the root. In this case, my image files are located in the folder 'assets' and the subfolder 'img', relative to my root folder path. (The same folder path where you have this `_config.yml`)
### Serve your file locally

For a local test, you can `serve` and run your Blog on your local machine. The general command for that is:

```sh
jekyll serve
```

However, in Windows 10 Bash, there is some bug on the filesystem watchers, where if you run this command, you will get an error like this:

```error
jekyll 3.7.2 | Error:  Invalid argument - Failed to watch "/mnt/c/
clone-blog/blog/.git/branches": the given event mask contains no legal
events; or fd is not an inotify file descriptor.
```
Thus, in order to deal with this issue, you should use this command instead:

```sh
jekyll serve -w --force_polling
```
By doing so, the terminal will define that `Auto-regeneration` is working and it will provide you with a local `Server address` ( usually the address is http://127.0.0.1:4000/).


## Deploying to GitHub

Get `git` into working in your terminal. You can either install git for Windows, or you can install it from terminal by using the command:
```git
sudo apt-get git
```

Then create a new repository on your GitHub account.

![New Repo]({{ "/assets/img/cap07.PNG" | absolute_url }})

Name your repository as the same name of your folder project.
And then click 'Create repository'

![Create Repo]({{ "/assets/img/cap08.PNG" | absolute_url }})
***

Basically, you just need to push your local file/repository into the remote repository. One way to do it is by following the instruction on '…or create a new repository on the command line'.


![Push Repo]({{ "/assets/img/cap09.PNG" | absolute_url }})
***

But, to be precise, I will walk you through the whole process correctly. So, go into your terminal and `cd` (Change Directory) into your Project Folder. In my case, the project folder is `/mnt/c/blog$`, therefore:
```cd
cd /mnt/c/blog$
```

Then we need to create a local repository, that acts as a transit for our files before we send them to a remote (GitHub).
```git
git init 
```

After you type `git init`, you will see that .git folder is created. This where they will store your 'temporary' transit, both `index` and the `local repository`.

Next, we would like to add all of our files in the folder into `index` pool. Type this command.
```add
git add .
```

This will put all of our files into the `index` pool. The `.` (period) means that it will select all files, but ignoring some of the files that is excluded by the list from `.gitignore`, which is automatically generated. After you add the files, you can run `git status` to check whether the files are added or not. Now, we need to commit the files added in the `index` pool into our `local repository`. Hence:
```commit
git commit -m "First Commit"
```

Then, we just need to push our file into the `remote repository`.
```
git remote add origin git@github.com:imoark/blog-demo.git
git push -u origin master
```

Congrats, your files are on GitHub.

## Setting Up GitHub Page and Custom Domain

Create a new branch named `gh-pages` on your repository, and then go the Settings.
![Branch]({{ "/assets/img/cap10.PNG" | absolute_url }})
***

Set your source into the `gh-pages` branch and then click 'Save'.
![Save]({{ "/assets/img/cap11.PNG" | absolute_url }})
***

**Setting Up GitHub page is done!!**

Now, we need set Custom Domain. I decided to use `blog.iomario.me` as my domain name, so input it into the tab.
![domain]({{ "/assets/img/cap12.PNG" | absolute_url }})
***

In your DNS Host Record, add a CNAME Record. In the `Host` column, put up your subdomain name. In the `Value`, put your default GitHub Page domain.
![DNS]({{ "/assets/img/cap13.PNG" | absolute_url }})
***

**Congratulation, you have your own Jekyll Blog hosted on GitHub with custom domain**