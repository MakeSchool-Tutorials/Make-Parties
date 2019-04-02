---
title: "Deleting Comments"
slug: deleting-rsvps
---

Finally, since we are creating rsvps, we should also be able to delete them.

Remember that currently we do not have authentication so we'll just be letting any user create and delete rsvps. If we developed this project for a real production use case, we would want to add authentication and only allow people to delete their own rsvps!

# Deleting Comments

You need to define a route. Here is the list of the current routes with a new one added for the rsvp form at the bottom.

| URL              | HTTP Verb | Action  |
|------------------|-----------|---------|
| /                | GET       | index   |
| /events/new     | GET       | new     |
| /events         | POST      | create  |
| /events/:id     | GET       | show    |
| /events/:id/edit| GET       | edit    |
| /events/:id     | PUT/PATCH | update  |
| /events/:id     | DELETE    | destroy |
| /events/:eventId/rsvps/new | GET      | new  |
| /events/:eventId/rsvps | POST      | create  |
| /events/:eventId/rsvps/:id | DELETE      | destroy  |

We'll use the `:id` url parameter to look up the rsvp we want to destroy.

# What the User Sees: Making a Delete Button

As always, we start with what the users sees and does. So let's make a link to delete a rsvp.

We can't set an `<a>` tag's method (it is always GET) so we are going to use a form to submit a DELETE request to our delete action path.

> [action]
>
> Add a button to each rsvp shown in an event by replacing the `.list-group mt-4` block in `views/events-show.handlebars` with the following:
>
```HTML
<!-- views/events-show.handlebars -->
  ...
    <div class="list-group mt-4">
      {{#each event.Rsvps}}
        <div class="list-group-item clearfix">
          <div class="float-left">
            {{this.name}}
          </div>
          <div class="float-right">
            <form class="form-inline" action="/events/{{../event.id}}/rsvps/{{this.id}}?_method=DELETE" method="post">
              <button type="submit" class="btn btn-link btn-sm">Remove</button>
            </form>
          </div>
        </div>
      {{/each}}
    </div>
  ...
```

If you try to submit this form, it won't work yet, because the route it points to does not exist. That is our next step.

> [info]
>
> Notice how we always move from what the user interacts with, to the controllers, and then finally to the models and database, and not the other way around. We do this intentionally because it is faster, and makes leaner more usable code and products.

# Adding the Destroy Route

Now we need a destroy action route. After deleting the rsvp, it should redirect back to the parent event (`events-show`).

> [action]
>
> Add the following `/delete` route to `controllers/rsvps.js`:
>
```js
// controllers/rsvps.js
...
  // DELETE
  app.delete('/events/:eventId/rsvps/:id', (req, res) => {
      models.Rsvp.findByPk(req.params.id).then(rsvp => {
          rsvp.destroy();
          res.redirect(`/events/${req.params.eventId}`);
      }).catch((err) => {
          console.log(err);
      });
  });
```

Now try deleting a rsvp.

# Git Commit and Push, and Push to Heroku

```bash
$ git add .
$ git commit -m 'Users can delete rsvps'
$ git push
$ git push heroku master
```

Before you can test your project on heroku, remember we made some changes to our DB, and we need to run the migrations on Heroku:

> [action]
>
> Run the migrations on Heroku
>
```bash
$ heroku run bash
$ sequelize -m # this just makes sure sequelize is installed
$ sequelize db:migrate
```

Open up your heroku project and do some manual testing. Great work!

# What Happened

So now we've completed all our user stories

1. Users can view all events (index)
1. Users can create a event (new/create)
1. Users can view one event (show)
1. Users can delete a event (destroy)
1. Users can edit a event (edit/update)
1. Users can rsvp on events (rsvps/create)
1. Users can delete rsvps (rsvps/destroy)

You used **Resource Based Development** and **Resourceful Routing** to build a event based app.
