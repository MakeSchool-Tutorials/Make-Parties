---
title: "Create Route: Saving a New Resource"
slug: creating-a-event
---

So now we have

1. A new event form that is submitting data to our controller route
1. An Event model
1. And a database setup with an `Events` table.

So now we just need to use our `Event` model to save to our database.

# Using Form Data to Create a Event

So now let's use that form data to save a new event to our SQL database using our `Event` model. We'll use the method provided to us by Sequelize called `create()`. After we create the event, let's redirect to the root path to see our new event.

To access our models we have to require them into our `app.js`

```js
const models = require('./db/models');
```

Now we can use this `models` variable to access our models in our routes:

```js
// app.js

// CREATE
app.post('/events', (req, res) => {
  models.Event.create(req.body).then(event => {
    res.redirect(`/events`);
  }).catch((err) => {
    console.log(err)
  });
})
```

When you submit your form what do you see? Probably ... nothing yet! Because we aren't actually loading our events in `events-index` from our database. Let's add that using Sequelize's `findAll()` method:

```js
// EVENTS#INDEX (HOME) (GET)
app.get('/', (req, res) => {
  models.Event.findAll().then(events => {
    res.render('events-index', { events: events });
  })
})
```

Ok, now do you see your events? Can you add a few?

If you add a few, notice that new ones are going on the bottom, but that's not good, we want new events on the top. We can use the `order` option in the `findAll()` method. the `order` option takes an array of arrays that contain the attribute and either `ASCE` or `DESC` for ascending or descending order.

```js
// EVENTS#INDEX (HOME) (GET)
app.get('/', (req, res) => {
  models.Event.findAll({ order: [['createdAt', 'DESC']] }).then(events => {
    res.render('events-index', { events: events });
  })
})
```

Now the only problem is the images are not showing because we don't have an `imgUrl` attribute. Let's add that in the next step:

# Adding a New Attribute to our Model

The new action will be our form for making a new event. For now events just have one attribute `title`, but we can always add more. Let's add an attribute called `imgUrl` and another called `desc` so we can write some more details about the event we've done.

First let's add what the user sees - the `events-new.handlebars` form input field.

```html
<!-- views/events-new.handlebars -->

```

Next, let's create a migration:

```bash
$ sequelize migration:create --name add-img-url-to-events
```

Open the new migration that is created, and let's add the following `addColumn()` and `removeColumn()` methods into it:

```js
'use strict';

module.exports = {
  up: (queryInterface, Sequelize) => {
    return queryInterface.addColumn('Events', 'imgUrl', { type: Sequelize.STRING });
  },

  down: (queryInterface, Sequelize) => {
    return queryInterface.removeColumn('Events', 'imgUrl');
  }
};

```

> [info]
> Migrations should be **reversible** if possible. Meaning they can be run forwards to make a change, and run backwards to reverse that change. The `up` and `down` functions in a migration are made to run this way.

Next, let's add the `imgUrl` attribute to the `Event` model.

```js
// db/models/event.js
...
  const Event = sequelize.define('Event', {
    title: DataTypes.STRING,
    desc: DataTypes.TEXT,
    imgUrl: DataTypes.STRING //add this line (don't forget the comma above!)
  }, {});
...
```

Can you resubmit the form? What happens now? Oops error! What does it say? Probably something like "there is no attribute imgUrl on Events". That's because we forgot to run the migration and actually update our database `Events` table:

```bash
$ sequelize db:migrate
```

Ok now resubmit the form... can you see your image on your newest event?

# Now Commit

```bash
$ git add .
$ git commit -m 'Users can create events'
$ git push
```

# Stretch Challenge: Adding a TakesPlaceOn Attribute

Events have to take place on a certain day. Can you add a `takesPlaceOn` attribute that is a `Date`, and then use `<input type="date">` elements? You might have to google for an example to see how this works.
