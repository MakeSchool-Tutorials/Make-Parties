# Tutorial Template

Each tutorial should have a `README.md` at the root with the following things included:

1. Preview image or gif of the final product
1. Last date / minor version of SDKs used the tutorial was tested on
1. Pre-requisites to understand the tutorial
1. Objectives for the tutorial
1. Instructor FAQ (common areas students make get stuck on, bugs they may encounter, etc)

# Writing your own tutorial

Use this repository as a starting point for your tutorial. Each tutorial must follow it's file structure and include all files included in this repository.

1. Setup Express
1. Use mock data to make initial routes
1. Add Sequelize
1. CRUD Tweets
  - index
  - show
  - edit
  - update
  - delete
  - tweet partial
1. CRUD Users, with auth
1. Associate tweets to Users
  ```bash
  $ sequelize migration:generate --name add-user-id-to-tweet
  ```
1. Follow users





$ npm init
$ npm i express —save

$ npm i express-handlebars --save


var express = require('express');
var exphbs  = require('express-handlebars');

var app = express();

app.engine('.hbs', exphbs({extname: '.hbs', defaultLayout: 'main'}));
app.set('view engine', '.hbs');

app.get('/', function (req, res) {
  res.render('home');
});

app.listen(3000);


<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Make Twitter</title>
</head>
<body>

  {{{body}}}

</body>
</html>




<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">

<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js" integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js" integrity="sha384-ChfqqxuZUCnJSK3+MXmPNIyE6ZbWh2IMqE241rYiqJxyMiZ6OW/JmZQ5stwEULTy" crossorigin="anonymous"></script>


  <div class="container">
      {{{body}}}
  </div>


  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <a class="navbar-brand" href="#">Make Twitter</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item active">
          <a class="nav-link" href="/">Home</a>
        </li>
      </ul>
    </div>
  </nav>


res.render('tweets-index', {tweets: "tweets"});

$ touch views/tweets-index.hbs
{{tweets}}

var tweets = [
  { body: "I am your first tweet" },
  { body: "I am your second tweet" },
  { body: "I am your third tweet" }
]

{{#each tweets}}
  <p>{{this.body}}</p>
{{/each}}

<div class="list-group mt-4">
  {{#each tweets}}
    <div class="list-group-item">{{this.body}}</div>
  {{/each}}
</div>


<div class="row mt-4">
  <div class="col-lg-6 offset-lg-3">
    <form action="/tweets" method="post" class="mt-4">
      <div class="form-group">
        <textarea name="body" class="form-control" placeholder="140 characters max"></textarea>
      </div>
      <div class="text-right">
        <button type="submit" class="btn btn-primary" name="button">Save</button>
      </div>
    </form>

    <div class="list-group mt-4">
      {{#each tweets}}
        <div class="list-group-item">{{this.body}}</div>
      {{/each}}
    </div>
  </div>
</div>


<a href="#" class="list-group-item list-group-item-action">{{this.body}}</a>


app.get('/login', function (req, res) {
  res.render('login');
})

<div class="row mt-4">
  <div class="col-lg-4 offset-lg-4">
    <div class="card">
      <div class="card-body">
        <div class="card-title">
          <legend>Login (insecure)</legend>
        </div>
        <div class="card-text">
          <form action="/login" method="post" class="mt-4">
            <div class="form-group">
              <label for="username">Username</label>
              <input name="username" class="form-control" placeholder="billybob123" />
            </div>
            <div class="text-right">
              <button type="submit" class="btn btn-primary" name="button">Save</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</div>



app.post('/login', function (req, res) {
  res.redirect(`/users/${req.body.username}`);
})


$ npm install body-parser —save

var bodyParser = require('body-parser')

// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))



Make login home

Tweets-index => Users-show

$ npm install --save sequelize pg pg-hstore

https://postgresapp.com/ - for installing & turning on
https://eggerapps.at/postico/ - for examining data


const Sequelize = require('sequelize');
const sequelize = new Sequelize('make-twitter', '', '', {
  host: 'localhost',
  dialect: 'postgres'
});

const User = sequelize.define(‘user', {
  firstName: { type: Sequelize.STRING },
  lastName: { type: Sequelize.STRING }
});

// force: true will drop the table if it already exists
User.sync({force: true}).then(() => {
  // Table created
  return User.create({
    firstName: 'John',
    lastName: 'Hancock'
  });
});

$ createdb make-twitter



Executing (default): SELECT 1+1 AS result
Executing (default): DROP TABLE IF EXISTS "Users" CASCADE;
Connection has been established successfully.
Executing (default): CREATE TABLE IF NOT EXISTS "Users" ("id"   SERIAL , "firstName" VARCHAR(255), "lastName" VARCHAR(255), "createdAt" TIMESTAMP WITH TIME ZONE NOT NULL, "updatedAt" TIMESTAMP WITH TIME ZONE NOT NULL, PRIMARY KEY ("id"));
Executing (default): SELECT i.relname AS name, ix.indisprimary AS primary, ix.indisunique AS unique, ix.indkey AS indkey, array_agg(a.attnum) as column_indexes, array_agg(a.attname) AS column_names, pg_get_indexdef(ix.indexrelid) AS definition FROM pg_class t, pg_class i, pg_index ix, pg_attribute a WHERE t.oid = ix.indrelid AND i.oid = ix.indexrelid AND a.attrelid = t.oid AND t.relkind = 'r' and t.relname = 'Users' GROUP BY i.relname, ix.indexrelid, ix.indisprimary, ix.indisunique, ix.indkey ORDER BY i.relname;
Executing (default): INSERT INTO "Users" ("id","firstName","lastName","createdAt","updatedAt") VALUES (DEFAULT,'John','Hancock','2018-12-04 19:16:21.271 +00:00','2018-12-04 19:16:21.271 +00:00') RETURNING *;



username: { type: Sequelize.STRING }

return User.create({
    username: 'JaneDoe'
 });

Remove the return User.create

app.post('/login', function (req, res) {
  User.findOrCreate({ where: { username: req.body.username } }).then((user, created) => {
    console.log(user)
    res.redirect(`/users/${user[0].dataValues.username}`);
  }).catch(function(err) {
    console.log(err)
  })
})


const Tweet = sequelize.define('tweet', {
  body: { type: Sequelize.STRING }
});

// force: true will drop the table if it already exists
Tweet.sync({force: true}).then(() => {
  // Table created
  return Tweet.create({
    body: 'A great tweet'
  });
});


//TWEETS#CREATE
app.post('/users/:username/tweets', function (req, res) {
  Tweet.create(req.body).then(function(tweet) {
    res.redirect(`/users/${req.params.username}`);
  }).catch(function(err) {
    console.log(err)
  })
})


// USERS#SHOW
app.get('/users/:username', function (req, res) {
  const user = { username: req.params.username };
  res.render('users-show', {tweets: tweets, user: user});
})

<form action="/users/{{user.username}}/tweets" method="post" class="mt-4">


// USERS#SHOW
app.get('/users/:username', function (req, res) {
  const user = { username: req.params.username };

  Tweet.findAll().then(function(tweets) {
    res.render('users-show', {tweets: tweets, user: user});
  })
})

      {{#each tweets}}
        <a href="/tweets/{{this.id}}" class="list-group-item list-group-item-action">{{this.body}}</a>
      {{/each}}


<div class="row mt-4">
  <div class="col-lg-4 offset-lg-4">
    <div class="card">
      <div class="card-body">
        <div class="card-title">
          <h4>{{tweet.body}}</h4>
        </div>
        <div class="card-text">
          Posted by <a href="/users/{{user.username}}">{{user.username}}</a>
        </div>
      </div>
    </div>
  </div>
</div>


          <p class="text-right">
            <a href="/users/{{user.username}}/tweets/{{tweet.id}}/edit">Edit</a>
          </p>


CRUD Tweets
CRUD Users



CRUD RSVPS
sequelize model:create --name Rsvp --attributes name:string,email:string
sequelize migration:create --name add-event-id-to-rsvps




sequelize migration:create --name add-img-url-to-events

https://via.placeholder.com/350x150
