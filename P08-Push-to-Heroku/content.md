---
title: "Push to Production with Heroku"
slug: push-to-heroku
---

Time to ship some code! Since we've built and styled our Rotten Potatoes app, let's show it to the world by putting it online. We'll use a service called [Heroku](https://www.heroku.com) that is free, but it does require a **credit card** to be on file.

# Sign Up For and Install Heroku

First you need to create a Heroku account at [heroku.com](https://www.heroku.com).

Then you will need to add the Heroku Command Line Interface (CLI) to your bash terminal so we can interact with the Heroku service via the terminal and git.

Next we'll have to initialize our project as a git repository...

# Adding Your Procfile

Before we push to Heroku, we'll have to create a special file, called `Procfile`, that lets Heroku know how to run your website. Create and open your new `Procfile` using the following commands:

```bash
$ touch Procfile
$ atom Procfile
```

Next, we need to insert the command to run our application on Heroku. Paste the following into your new `Procfile` and save:

```bash
web: node app.js
```

Great! When we push to Heroku, it'll know how to run our application. Keep going!


# Pushing to Heroku using Github

Let's start by installing git if you don't already have it installed. And then initializing our project folder as a git repository. Then we can check the git status. We should see all our files as uncommitted and unstaged to commit.

```bash
$ brew install git
$ git init
$ git status
```

Now we can stage all the files to commit, then commit them adding a commit message, and then double check our status. We should see we have no files to commit bc we just committed them all.

```bash
$ git add .
$ git commit -m 'init'
$ git status
```

Now let's use the `heroku` command to create a heroku app and name it with "rotten-potatoes" and then our initials. So someone named Samantha Bee would be "rotten-potatoes-sb". This `heroku create` command will add our heroku app as a git remote repository that we will be able to push to using the git command `git push`. We can see our remote repos by using the command `git remote -v`.

```bash
$ heroku create rotten-potatoes-MY-INITIALS
$ git remote -v
```

Alright, now we can push our code to heroku and open our new website.

```bash
$ git push heroku master
$ heroku open
```

You are probably seeing an error screen right now! Bit of a let down maybe. But also a good lesson - **Things never work the first time**. Let's debug our issues.

# Scale Your Heroku application

We need to run an additional command that tells Heroku to assign a free worker to our deployment in order to run your website. In Heroku syntax, that means executing the `ps:scale` command, like so:

```bash
$ heroku ps:scale web=1
```

**Just like `heroku create`, you only need to execute this command once per project**!

# Heroku Logs

When ever you have an error or problem with Heroku, you can see the logs by running `heroku logs`.

```bash
$ heroku logs
```

If you want to see the logs to continually you can add `--tail` to this command.

```bash
$ heroku logs --tail
```

What error are we seeing in heroku now? What do we need to do?

You might need to add a "start" command to your `package.json` file:

```
// package.json

"scripts": {
  "start": "node app.js"
}
```

# Adding a Production Database

It looks like the error is that we cannot connect to our mongodb database. That's because it is looking at the `'mongodb://localhost/rotten-potatoes'` URI, but that is on our local computer and heroku, which is remote, doesn't have access to that. So we have to add a mongodb heroku add-on called mLabs.

```bash
$ heroku addons:create mongolab:sandbox
```

Then we have to point to this production mongodb database URI in our `app.js` file.

```js
// app.js

var mongoose = require('mongoose');
mongoose.connect(process.env.MONGODB_URI || 'mongodb://localhost/rotten-potatoes');
```

Now if we try to open our heroku app what happens?

```bash
$ heroku open
```

# ... Hanging?

Another error! This is a weird one. First it just hangs for a while, then times out, and then the error says "cannot bind on $PORT". This is because Heroku does not use port 3000, it uses another port available in production at `process.env.PORT`, just like your mongoDB URI.

Let's fix that by setting the port also with the `process.env`

Then we have to point to this production mongodb database URI in our `app.js` file.

```js
// app.js

const port = process.env.PORT || 3000;
app.listen(port);
```

Now if we try to open our heroku app what happens?

```bash
$ heroku open
```

# WooHoo!

Awwww yeah - you did it! You made your first RESTful and Resourceful app using Node.js, Express.js, and MongoDB!
