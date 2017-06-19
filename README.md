# don’t use @import

Why we need to use @import when we have ``` <link> ``` element. @import has a negative impact on web page performance.

### LINK vs. @import

There are two ways to include a stylesheet in your web page. 

### link
```html
<link rel='stylesheet' href='a.css'>
```
### @import
```html
@import url('a.css');
```

I prefer using LINK for simplicity. You have to remember to put @import at the top of the style block or else it won’t work.

I’m going to walk through the different ways LINK and @import can be used.

# Example -1

#### @import @import

```html
<style>
@import url('a.css');
@import url('b.css');
</style>
```

In these examples, there are two stylesheets: a.css and b.css. If you always use @import in this way, there are no performance problems, the two stylesheets are downloaded in parallel.
(The first tiny request is the HTML document.) 

![screenshot](https://user-images.githubusercontent.com/6780840/27283123-7609e1c8-5510-11e7-92e0-6a7d5f125025.png)

# Example -2

#### LINK @import

This example uses LINK for a.css, and @import for b.css

```html
<link rel='stylesheet' type='text/css' href='a.css'>
<style>
@import url('b.css');
</style>
```
This causes the stylesheets to be downloaded sequentially. As shown here, this behavior in IE causes the page to take a longer time to finish.

![screenshot](https://user-images.githubusercontent.com/6780840/27283237-fdbb2a28-5510-11e7-8e5e-142931745a75.png)

# Example -3

#### LINK with @import

In this example, a.css is inserted using LINK, and a.css has an @import rule to pull in b.css

```html
<link rel='stylesheet' type='text/css' href='a.css'>
in a.css:
@import url('b.css');
```
This pattern also prevents the stylesheets from loading in parallel, but this time it happens on all browsers. 

![screenshot](https://user-images.githubusercontent.com/6780840/27283738-57696e84-5513-11e7-8f82-4c2950d9c6f2.png)

The browser has to download a.css and parse it. At that point, the browser sees the @import rule and starts to fetch b.css.

# Example -4

#### many @imports

The many @imports example shows that using @import in IE causes resources to be downloaded in a different order than specified.This example has six stylesheets (each takes two seconds to download) followed by a script (a four second download).

```html
<style>
@import url('a.css');
@import url('b.css');
@import url('c.css');
@import url('d.css');
@import url('e.css');
@import url('f.css');
</style>
<script src='one.js' type='text/javascript'></script>
```

![screenshot](https://user-images.githubusercontent.com/6780840/27283864-fbf3f352-5513-11e7-8713-61552af796cf.png)

the longest bar is the four second script. Even though it was listed last, it gets downloaded first in IE. If the script contains code that depends on the styles applied from the stylesheets (a la getElementsByClassName, etc.), then unexpected results may occur because the script is loaded before the stylesheets, despite the developer listing it last.


# Example -5

#### LINK LINK

It’s simpler and safer to use LINK to pull in stylesheets:

```html
<link rel='stylesheet' type='text/css' href='a.css'>
<link rel='stylesheet' type='text/css' href='b.css'>
```
Using LINK ensures that stylesheets will be downloaded in parallel across all browsers.

![screenshot](https://user-images.githubusercontent.com/6780840/27283123-7609e1c8-5510-11e7-92e0-6a7d5f125025.png)

Using LINK also guarantees resources are downloaded in the order specified by the developer.
