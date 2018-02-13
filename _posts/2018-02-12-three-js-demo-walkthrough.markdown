---
layout: post
title:  "Three.js Demo Walkthrough"
date:   2018-02-12 16:08:33 -0800
categories: jekyll update
---


Today, I will discuss about Three.js. A powerful 3D graphic JavaScript library that has been recently gain its popularity. In this tutorial, I will walk you through on a simple yet interactive demo of Three.js. It will cover the interactivity of Three.js and basic concept of using 3D. Here we go!

## Get Three.js

As usual, first of all you will need to get the Three.js library. You can get the [minified library](http://threejs.org/build/three.min.js) of Three.js from the link above and include it in your HTML, or install and import it as a [module](http://threejs.org/docs/#manual/introduction/Import-via-modules),
Alternatively see [how to build the library yourself](https://github.com/mrdoob/three.js/wiki/Build-instructions).

Three.js has a very complete [documentation](http://threejs.org/) and a good [GitHub](https://github.com/mrdoob/three.js/) page that you can checkout. Kudos to Three.js!!

After that, create the initiation .js file to create our 3D demo. In my case, I make `threeinit.js`. So, basically right now you should have a file structure like this in your project folder.

![Project Folder]({{ "/assets/img/cap14.PNG" | absolute_url }})

## Start Typing Down Something

Now, let's jump in into our `index.html`. Simply create HTML5 markdown and load both of our JavaScript file.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Three.js Demo</title>
</head>
<body>
  
  <script type="text/javascript" src="three.js"></script>
  <script type="text/javascript" src="threeinit.js"></script>

</body>
</html>
```

And then, the serious part will begin. All of our code will be mainly on our `threeinit.js`.

## **threeinit.js**

So, things are gonna get a little bit complicated now.

The basic concept of how three.js works, is there are 3 main component. `scene`, `camera` and `renderer`. These 3 key compenent is pretty basic if you are familiar with how 3D works in computer world. In a stupid way, if I may explain, `scene` is like the background of your 3D world. `camera` is your 'eye' or point-of-view in this 3D space. `renderer` is how the 3D object will be perceived, for example: is it gonna be realistic, cartoony, cell-shading, or just wireframe, low-poly, etc.

But, just to make it simple, let's go back to our code and make these 3 variable.

```let
// Three main component
let camera, scene, renderer;
```

We have now set up the scene, our camera and the renderer.
I will setup my `camera` inside of the `function init(){}`, so it doesn't interfere with anything else.

```code

function init() {

    camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 1, 10000);
    camera.position.z = 500;

}
```
There are a few different cameras in three.js. For now, let's use the common one, which is **_PerspectiveCamera_**.

The first attribute is the field of view `60`. FOV is the extent of the scene that is seen on the display at any given moment. The value is in degrees.

The second one is the aspect ratio `window.innerWidth / window.innerHeight`. You almost always want to use the width of the element divided by the height, or you'll get the same result as when you play old movies on a widescreen TV - the image looks squished.

The next two attributes are the near `1` and far `10000` clipping plane. What that means, is that objects further away from the camera than the value of far or closer than near won't be rendered. You don't have to worry about this now, but you may want to use other values in your apps to get better performance.

Then, we will set our `renderer` variable.

Next up is the renderer. This is where the magic happens. In addition to the WebGLRenderer we use here, three.js comes with a few others, often used as fallbacks for users with older browsers or for those who don't have WebGL support for some reason.
In addition to creating the renderer instance, we also need to set the size at which we want it to render our app. It's a good idea to use the width and height of the area we want to fill with our app - in this case, the width and height of the browser window. For performance intensive apps, you can also give setSize smaller values, like window.innerWidth/2 and window.innerHeight/2, which will make the app render at half size.
If you wish to keep the size of your app but render it at a lower resolution, you can do so by calling setSize with false as updateStyle (the third argument). For example, setSize(window.innerWidth/2, window.innerHeight/2, false) will render your app at half resolution, given that your `<canvas>` has 100% width and height.
Last but not least, we add the renderer element to our HTML document. This is a `<canvas>` element the renderer uses to display the scene to us.

<p data-height="374" data-theme-id="dark" data-slug-hash="jZLqzW" data-default-tab="result" data-user="imoark" data-embed-version="2" data-pen-title="jZLqzW" class="codepen">See the Pen <a href="https://codepen.io/imoark/pen/jZLqzW/">jZLqzW</a> by imoark (<a href="https://codepen.io/imoark">@imoark</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

