#Authentication

##Goals
* understand the basics of how passport works
* Use a session to authenticate a user
* Learn how authentication depends on sessions

This is meant as a boilerplate for Project 3 - you will not be expected to construct this from scratch.

##What passport does
* lays down the big picture mechanics of authentication
* puts a user object on req
* req.user shows us what the user is logged in as. If it is an empty object, they are not logged in
* You have to tell passport where to get the user's record from the database
* after login, nothing goes through passport anymore
* req.logout logs users out

##Local strategy of authentication
* We will store authentication info locally (user names and passwords)
* pass the name of the strategy to passport.authenticate, with a successRedirect and failureRedirect
* passport.authenticate returns a function which will be the route handler for post requests to /login
* WHen a post comes to login, the passport will use the function you write for passport to check the user's password from the database. If sucessful, they will be logged in and redirected to the success route. If authentication fails, they will be redirected to the failure route.
*to check if a user is logged in, use ```if(!req.user)```

###User Model
* using Mongoose to set up the Users model
* bcrypt encrypts passwords -> when you get a password from the user, you must run itthrough bcrypt as well to see if it matches
* uniqueValidator plugin allows us to validate that each user name is unique
* the bcrypt#compare function compares the given password to the passwordDigest we have stored

####What goes on in the User Model password encryption
1. Generate a salt from the password
2. Plug in the salt and the password to the same function, bcrypt#hash
3. The result of that function is a digest, which is stuck into the user record as a passwordDigest.

This ensures we're not saving people's passwords as plaintext. They are one-way encrypted so that if stolen, they cannot be used. 


###Passport internals
* in lib/passport.js, we set up passport
* we tell passport where to look for user data in our database
* passport expects a done method
* serialize user reduces the user to a serial number
* 

###Local Strategy constructor
* takes a function which gets the username and password from login request
* done is a callback given to us by LocalStrategy. WHen we are done, we call it with an object or an error
* the first thing it does is see if we have a user with that username
