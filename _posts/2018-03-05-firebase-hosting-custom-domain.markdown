---
layout: post
title:  "Firebase Hosting with Custom Domain"
date:   2018-03-04 22:17:01 -0800
categories: jekyll update
---


Google Firebase has a feature that enable you to host your website for free using its Hosting services. For 1GB size of data, 10GB/month of traffic and custom domain, it is all under Spark Plan that will cost you $0/month. Awesome, right!?

[Check the details on Google Firebase Pricing Plans](https://firebase.google.com/pricing/?authuser=0)

To start with Google Firebase, go check their [website](https://firebase.google.com/).

Chances are, you've probably had a Google Account before(or at least a GMail account), so you can just directly click on Go To Console in the top right section of your page. If you haven't had a Google Account, just simply create one.

After you go into the console page, click on Create Project, and name your Project. I will just name my project as "firebase-hosting".

![Add Console]({{ "/assets/img/cap15.PNG" | absolute_url }})

![Project Name]({{ "/assets/img/cap16.PNG" | absolute_url }})

![Create Project]({{ "/assets/img/cap17.PNG" | absolute_url }})

This is all you need to do in the firebase webpage. The rest, will be done in CLI (Command Line Interface). Yay or Yikes???

## Firebase CLI

To host your site, you need to install Firebase command line tools using npm (Node.js)

```
Install Firebase tools:

$ npm install -g firebase-tools

````

If you've previously installed Firebase command line tools, run the install command again to make sure you have the latest version

After your firebase tools is installed, open a terminal window and navigate to or create a directory for your site

```
Sign in to Google:

$ firebase login

```
```
Initiate your project:

$ firebase init
```
Add your static files to your deploy directory (the default is public)

```
Deploy your website:

$ firebase deploy
```


Now, let's try to do this. Create a directory where you will want to init your firebase. I am using  `C:\firebase-hosting>`. Open a terminal in there and do `firebase login`. After you connected your account, run `firebase init`. Proceed Y and choose the Hosting option.

![CLI1]({{ "/assets/img/cap18.PNG" | absolute_url }})

Choose the project name that you have created. When it is done, you should get this result.

```
i  Writing configuration info to firebase.json...
i  Writing project information to .firebaserc...

+  Firebase initialization complete!
```

Now, create our deploy directory, and just name it "public" (default directory name from Firebase). So, right now we need to put the file that we want to deploy inside the `C:\firebase-hosting\public>` 	

I've created a simple HTML file to use. Download the file in [here](https://firebasestorage.googleapis.com/v0/b/fir-hosting-6eaaa.appspot.com/o/index.html?alt=media&token=ffd8ce4a-db48-423a-8ddb-7bf7b586c619).

Next step, we need to Customize Hosting Behavior. To do that, open your `firebase.json`, and write this settings.

```
{
	"hosting": {
    "public": "public",
    "ignore": [
      "firebase.json"
    ]
  }
}

```

Now, from your `C:\firebase-hosting>` terminal, run 

```
firebase deploy
```
You should get a success message like this:

![CLI2]({{ "/assets/img/cap19.PNG" | absolute_url }})

Now, go to your Firebase Console in the webpage, and go to the Hosting menu. In the Hosting Dashboard, tt should show your Domain and Deployment History.

![Console Dash]({{ "/assets/img/cap20.PNG" | absolute_url }})

## Custom Domain

In the Domain area, click on to `Connect Domain`.
Proceed onto the steps just like the screenshot below.

![Domain1]({{ "/assets/img/cap21.PNG" | absolute_url }})

![Domain2]({{ "/assets/img/cap22.PNG" | absolute_url }})

Now, notice that in your DNS settings, there might need to be some adjustment on your hostname. For instance, in my NameCheap, I should change my hostname into `@`.

![Domain3]({{ "/assets/img/cap23.PNG" | absolute_url }})

And the last thing to add is the A record to direct the IP into your domain name. In this case, exactly follow the instruction that Google provides.

![Domain3]({{ "/assets/img/cap24.PNG" | absolute_url }})

Voila! You have a website running now!

checkout my [website](https://hosting.iomario.me/) running on Google Hosting

