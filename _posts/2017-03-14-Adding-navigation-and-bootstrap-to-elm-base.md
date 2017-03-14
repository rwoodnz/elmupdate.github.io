---
layout: post
title: Adding navigation and Bootstrap to Elm base
comments: true
---

Went looking for a Bootstrap style library for Elm. I was hoping to find something that completely hid the javascript aspects of bootstrap and fortunately there was a recently released [elm-bootstrap module](http://elm-bootstrap.info/). You just install the module and add the references needed in your elm app.

You also need to reference the Bootstrap css file. That would be easy to load alongside any other css files using webpack. However, I wanted to reference the CDN so placed the suggested code into my index.html file. 

A bonus was that the example elm code showed how to implement a basic multipage navigation. Managed to get it working fairly quickly, at the expense of losing the image path passing mechanism. I couldn't see immediatly how to pass strings when using Navigation.programWithFlags so will leave that out until I need it back or until someone tells me how to...

Also needed to disable the unit tests for now. Rewriting them later should help me understand more how the navigation code works.



