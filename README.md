FacebookViewer
==============

A mini-project to practice Passport/Node.js

##Objectives
Create a simple app that shows Facebook Profile information.

You can use the [passport-facebook](https://github.com/jaredhanson/passport-facebook) GitHub repo for a guide.

##Step 1: Set up passport, passport-facebook, express
Set up a server.js file and include these npm dependencies:
* express
* express-session
* passport
* passport-facebook

Go over to the [Facebook Developer Portal](https://developers.facebook.com/) and Add a New App. Call it whatever you'd like.

Now let's put in the code necessary to get our authentication working:
* Create your express app, have it listen to a port that works for you
* Require passport and the passport FacebookStrategy
* Include the session middleware

`app.use(session({secret: 'some-random-string'}))`

* Include the passport.initialize middleware

`app.use(passport.initialize())`

* Include the passport.session middleware 

`app.use(passport.session())`

* Define the FacebookStrategy

```javascript
passport.use(new FacebookStrategy({
  clientID: '<your_client_id>',
  clientSecret: '<your_client_secret>',
  callbackURL: 'http://localhost:3000/auth/facebook/callback'
}, function(token, refreshToken, profile, done) {
  return done(null, profile);
}));
```

##Step 2: Define your auth endpoints
Create two routes that will handle your Facebook auth.

####GET /auth/facebook
This route simply implements the passport.authenticate method, passing 'facebook' as the parameter.

####GET /auth/facebook/callback
This route needs to pass the passport.authenticate method again, except we also need to pass in an object that passes the successRedirect and failureRedirect paths.

##Step 3: Create the deserialize/serializer methods on passport.
Since you won't be doing anything further than just passing objects to/from passport and the session, we just need bare bones methods here:

```
passport.serializeUser(function(user, done) {
  done(null, user);
});
 
passport.deserializeUser(function(obj, done) {
  done(null, obj);
});
```

###Step 4: Create viewer endpoint
Now we're going to create an endpoint that returns the current logged in user's Facebook profile data.

####GET /me
Create this route in your server.js that returns the user's Facebook profile data. The data is stored in `req.user` if you've set everything up correctly. Return a JSON representation of this data at the `/me` endpoint.

Use Postman or the browser to verify that you can in fact get the JSON data from the `/me` endpoint.


