---
layout: post
title: Setting up an Elm base
comments: true
---

Have done a few Elm tutorials and planning to now move onto some development. I'm going to start with an app shell that I can reuse including the ability to register and login, with connections to an authentication/authorisation service such as AWS Cognito.

I decided despite this being my first Elm attempt that there are a couple of nice-to-haves that it would actually be nice to have. These are Webpack - as I want more experience in it - and Travis CI - to auto-deploy from github to Travis CI to a hosting service. If in the end I go down a different path, Travis CI is easy to disengage from.

## Webpack
I had some issues with Webpack in a tutorial because of where files were placed in a very basic solution. So this time I went on the hunt for a proven setup. I got more than I bargined for with [create-elm-app](https://github.com/halfzebra/create-elm-app). This superb generator sets you up with webpack dev and production scripts and hot loading as well as the node-test-runner for unit testing.

While I want to have as few extraneous node scripts in my project as possible, I'll assume for now that there's good reason for what the creator has included and enjoy the added benefits. The structure of the application is a step up from the basic tutorials, including a method for passing an image path into the solution. Not sure yet while that is necessary, but will go with it.

I did run into one wrinkle. The create-elm-app is designed to live on your system and enable you to generate apps when you want. If you want to configure your own base app then you need to run it's eject command to copy some files into your solution. Only that leaves some of its command files still there and they don't point to the scripts in your solution. So I deleted those myself.

## Travis CI
Setting up Travis CI was not too much of a problem. I copied the suggested travis.yml file from Richard Feldman's [node-test-runner](https://github.com/rtfeldman/node-test-runner). That's when I discovered the elm-make command he had in it didn't work in this context. Rather than spend too much time working out why, when I'm building with Webpack, I took the suggestion in a forum and used the travis.yml file from his elm.css, removing a couple of lines that seemed unnecessary, and that worked fine. 

Note there's some code in these tried-and-tested yml files that [overcomes a problem of long rebuild times on Travis](https://github.com/elm-lang/elm-compiler/issues/1473#issuecomment-245704142).

So, good result. The empty app is up locally and building and running tests on Travis. Next steps are to add elm-bootstrap, get a basic login form going and add an authentication facade or service connection.

[rwoodnz/elm-base](https://github.com/rwoodnz/elm-base)
