---
title: "Push to Production with Heroku"
slug: push-to-heroku
---

Time to ship some code! Since we've built and styled our Make Parties app, let's show it to the world by putting it online. We'll use a service called [Heroku](https://www.heroku.com) that is free, but it does require a **credit card** to be on file.

## Resources

It can be fairly common to get Heroku set up. If you run into trouble, here are some resources to help you along the way:

- [Heroku Error Codes](https://devcenter.heroku.com/articles/error-codes)
- [Running Migrations on Heroku Using Sequelize](https://sequelize.readthedocs.io/en/1.7.0/articles/heroku/#running-migrations)

And don't forget the best resource: Googling your error!

# Sign Up For and Install Heroku

> [action]
>
> First you need to create a Heroku account at [heroku.com](https://www.heroku.com).

We will only be using free tools from Heroku, but to add free versions of specific add-on's, Heroku will require a credit card or debit card number.

Then you will need to add the Heroku Command Line Interface (CLI) to your bash terminal so we can interact with the Heroku service via the terminal and git.

## Cloud9

> [action]
>
> In Cloud9 it is already installed but should be updated:
>
```bash
$ wget -O- https://toolbelt.heroku.com/install-ubuntu.sh | sh
```

## Your Computer

> [action]
>
> In your computer you can install it with homebrew
>
```bash
$ brew install heroku/brew/heroku
```

# Login to Heroku

> [action]
>
> Now login to heroku
>
```bash
$ heroku login -i
```

Now let's use the `heroku` command to create a heroku app and name it with "make-parties" and then our initials. So someone named Samantha Bee would be "make-parties-sb". This `heroku create` command will add our heroku app as a git remote repository that we will be able to push to using the git command `git push`. We can see our remote repos by using the command `git remote -v`.

> [action]
>
> Run the `heroku create` command:
>
```bash
$ heroku create make-parties-YOURINITIALS
```

Navigate your browser to `https://make-parties-YOURINITIALS.herokuapp.com` and see that there is an error there, and that it instructs you to run `heroku logs --tail`.

> [action]
>
> Run `heroku logs --tail` in your terminal:
>
```bash
$ heroku logs --tail
```

 After running that command, you should see something like this:

```
2018-12-14T19:07:54.989284+00:00 app[api]: Release v1 created by user ajbraus@gmail.com
2018-12-14T19:07:54.989284+00:00 app[api]: Initial release by user ajbraus@gmail.com
2018-12-14T19:07:55.926176+00:00 app[api]: Release v2 created by user ajbraus@gmail.com
2018-12-14T19:07:55.926176+00:00 app[api]: Enable Logplex by user ajbraus@gmail.com
2018-12-14T19:08:56.223988+00:00 app[api]: Deploy 8477ac80 by user ajbraus@gmail.com
2018-12-14T19:08:56.223988+00:00 app[api]: Release v3 created by user ajbraus@gmail.com
2018-12-14T19:08:56.245417+00:00 app[api]: Scaled to web@1:Free by user ajbraus@gmail.com
2018-12-14T19:08:58.822761+00:00 heroku[web.1]: Starting process with command `npm start`
2018-12-14T19:09:02.131028+00:00 heroku[web.1]: State changed from starting to crashed
2018-12-14T19:09:02.136221+00:00 heroku[web.1]: State changed from crashed to starting
2018-12-14T19:09:02.106945+00:00 heroku[web.1]: Process exited with status 1
2018-12-14T19:09:02.035080+00:00 app[web.1]: npm ERR! missing script: start
2018-12-14T19:09:02.044094+00:00 app[web.1]:
2018-12-14T19:09:02.044534+00:00 app[web.1]: npm ERR! A complete log of this run can be found in:
2018-12-14T19:09:02.044699+00:00 app[web.1]: npm ERR!     /app/.npm/_logs/2018-12-14T19_09_02_037Z-debug.log
2018-12-14T19:09:04.679625+00:00 heroku[web.1]: Starting process with command `npm start`
2018-12-14T19:09:08.105133+00:00 heroku[web.1]: State changed from starting to crashed
2018-12-14T19:09:08.083706+00:00 heroku[web.1]: Process exited with status 1
2018-12-14T19:09:07.985068+00:00 app[web.1]: npm ERR! missing script: start
2018-12-14T19:09:08.015827+00:00 app[web.1]:
2018-12-14T19:09:08.016884+00:00 app[web.1]: npm ERR! A complete log of this run can be found in:
2018-12-14T19:09:08.017337+00:00 app[web.1]: npm ERR!     /app/.npm/_logs/2018-12-14T19_09_07_997Z-debug.log
2018-12-14T19:09:10.316856+00:00 heroku[router]: at=error code=H10 desc="App crashed" method=GET path="/" host=make-parties-ajb.herokuapp.com request_id=e2405128-55ee-4c0a-b447-184d879f900a fwd="104.197.64.166" dyno= connect= service= status=503 bytes= protocol=https
2018-12-14T19:13:56.767347+00:00 heroku[router]: at=error code=H10 desc="App crashed" method=GET path="/" host=make-parties-ajb.herokuapp.com request_id=a543045a-1d7e-497f-b2cd-ef49095b4c9a fwd="142.254.99.26" dyno= connect= service= status=503 bytes= protocol=https
2018-12-14T19:13:57.468424+00:00 heroku[router]: at=error code=H10 desc="App crashed" method=GET path="/favicon.ico" host=make-parties-ajb.herokuapp.com request_id=339b35a7-399d-4420-822c-faaa3d328fb2 fwd="142.254.99.26" dyno= connect= service= status=503 bytes= protocol=https
^C
```

The important line here is in the middle:

```
2018-12-14T19:09:02.035080+00:00 app[web.1]: npm ERR! missing script: start
```

Heroku requires a start script. So we're going to add that through our `package.json` file.

> [info]
>
> Even if you don't receive any of these errors, it's important to still follow these steps so that your setup for heroku is correct.

# Adding Your Procfile

Heroku calls `npm start` when it starts an node project. So you have to define that script in the `package.json` file.

> [action]
>
> Open `package.json` and add the following "start" script to it:
>
```json
...
  "scripts": {
    "start": "node app.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
...
```

Alright, now we can add and commit and push our code to heroku and open our new website.

> [action]
>
> Follow the usual commit steps, but this time we'll push to `heroku master`:
>
```bash
$ git add .
$ git commit -m 'adding start script'
$ git push heroku master
```

Navigate your browser to `https://make-parties-YOURINITIALS.herokuapp.com`. You are probably seeing an error screen right now! Bit of a let down maybe. But also a good lesson - **Things never work the first time**. Let's debug our issues.

# Adding a Production Database

It looks like the error is that we cannot connect to our PostgreSQL database. That's because it is looking at our `db/config/config.json` file and then at the `production` option, and then looking for the `PROD_DATABASE_URL` URI, but that is not defined on Heroku. So we have to add a PostgreSQL database to Heroku using an add-on called [Heroku Postgres](https://elements.heroku.com/addons/heroku-postgresql).

> [action]
>
> Add Heroku Postgres:
>
```bash
$ heroku addons:create heroku-postgresql:hobby-dev
```

We will still have problems though because we need to update our `config.json`.

> [action]
>
> Update `db/config/config.json` to have the following in the `production` section:
>
```json
 ...
 "production": {
   "use_env_variable": "DATABASE_URL",
   "dialect": "postgres",
   "dialectOptions": {
      "ssl": true
    }
 }
```

We also need to add a Heroku config variable to mark that we are in production:

> [action]
>
> Add a Heroku config variable
>
```bash
$ heroku config:set NODE_ENV=production
```

Finally, we need run our migrations onto Heroku as well!

> [action]
>
> Run the following commands for the migration:
>
```bash
$ heroku run bash
$ sequelize -m # this just makes sure sequelize is installed
$ sequelize db:migrate
```

Ok now our database should connect!

> [action]
>
> Push up to Heroku:
>
```bash
$ git add .
$ git commit -m "pushing heroku config info"
$ git push heroku master
```

# WooHoo!

If everything went well, you should be able to navigate to your Heroku site and see a blank version of our app:

![heroku-live](assets/heroku-live.png)

Awwww yeah - you did it! You made your first RESTful and Resourceful app using Node.js, Express.js, and PostgreSQL! V1 is done and shipped!
