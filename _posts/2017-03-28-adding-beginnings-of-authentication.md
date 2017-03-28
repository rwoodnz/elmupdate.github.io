---
layout: post
title: Adding beginnings of authentication
comments: true
---

The idea of my [Elm-base project](https://github.com/rwoodnz/elm-base) is to provide an intial Elm setup to build upon with existing navigation hooks and pages ready to populate.

To be anything more than a toy it will need to connect to an API back-end solution. And for that to work it will need authentication capability. At this point that means choosing technologies.

For the back-end I'm intrigued by the concept of serverless applications, and looking at that I guess AWS is where the action is right now and something I'd like to learn more about. What I've seen so far looks very doable so long as you are happy with a NoSql data store. Once you go relational it seems you're back into server management but happy to hear of options.

Authentication is an area I would also love to learn more about in detail as the whole passing tokens around thing can be a mind-bender. In the meantime the world is also providing out-of-the-box capability.

Looking into this, Auth0 is providing for most Javascript frameworks with a developer friendly service and has info for how to connect Elm up as well through its port facility to the Javascript Auth0 code or CDN.

So I had a merry and sometimes stressful time working through the Elm sample code, which for some inexplicable reason was presented in two modules. I wasn't convinced this was providing much of a facade, if that was the point, so in my implementation I have one Auth module. 

That module works through one port to kick off the 'lock' screen where you can login either with password or social media - that's all provided by the service. Another port then works with a subscription to receive back the authentication JSON Web Token and the profile data about the user. I've put the storing and retrieving of the token and profile data into local browser storage into the main app as that seems not directly about authentication but rather user convenience.

```elm
        ReceiveMaybeLoggedInUser maybeLoggedInUser ->
            let
                newAuthenticationModel =
                    case maybeLoggedInUser of
                        Just loggedInUser ->
                            Auth.LoggedIn loggedInUser

                        Nothing ->
                            Auth.NotLoggedIn
            in
                ( { model
                    | authenticationModel = newAuthenticationModel
                  }
                , if
                    model.authenticationRequired
                        && not (Auth.isLoggedIn newAuthenticationModel)
                  then
                    Auth.showLock
                  else
                    Cmd.none
                )
```

One hiccup with the Auth0 process is that the same user will give you back different profiles depending on whether they logged in with password or social media. You can get Auth0 to move you up to a paid service level and have some tools to handle that. However I think this could and may be better be handled in the back end solution.

There's also a UX glitch when logging in using the popup option. When relogging in, a blank window opens - something to do with the social media logins. It looks pretty bad but will just live with it for now. For that pop up option also had to use CSS to hide the close box because chosing the closable option mean an autoclosing option was disabled. Didn't make a lot of sense.

I've set the application up to have two types of authentication - triggered automatically with page navigation disabled, or login at will from within the app. So it's either for a web application that is behind a login, or an application that hides certain things from you if you are logged in.

So that seems to be working now. Time perhaps to try connecting to AWS or something.
