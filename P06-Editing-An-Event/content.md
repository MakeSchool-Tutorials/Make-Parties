---
title: "Edit Route: Editing and Updating a Resource"
slug: editing-a-event
---

Now we're checking off the seven **Resourceful Routes**. But we're not done until all of them are complete.

| URL              | HTTP Verb | Action  |
|------------------|-----------|---------|
| /                | GET       | index   |
| /events/new     | GET       | new     |
| /events         | POST      | create  |
| /events/:id     | GET       | show    |
| /events/:id/edit     | GET       | edit    |
| /events/:id     | PUT/PATCH | update  |

Normally in a site like Eventbrite, we would only want authors of events to have the permission to edit or delete a event. However, because we do not have authentication yet, we're just going to let anyone edit and delete events.

# Edit Link

We want people to be able to edit and update events, let's again start from the user's perspective. Edit and Update are similar to New and Create. First we need a link to the edit route that renders a form template called `events-edit`, and then we submit that edit form to the update route which will redirect to the show action.

So let's make the edit link inside our respsonsive row and middle 6 columns:

```html
<!-- views/events-show.handlebars -->

...

    <div class="text-right">
      <a href="/events/{{event.id}}/edit">Edit</a>
    </div>

...
```

Ok now if we click that edit link, we'll see correctly that the route is not found. So let's make our edit action. The edit action is like the show action because we look up the `event` by its `id` in the url parameter, but then we render the information in a template as editable form elements.

```js
// app.js

...

// EDIT
app.get('/events/:id/edit', (req, res) => {
  models.Event.findById(req.params.id, (err, event) => {
    res.render('events-edit', {event: event});
  });
});
```

And of course we'll need that `events-edit` template. This template is a bit weird for three reasons:

1. **`value=""`** - The form needs to display the current values of the event's attributes. We are using the `value` html attribute to pass in the values of the event we are trying to edit.
1. **`<textarea>{{}}</textarea>`** - the `<textarea>` HTML tag does not have a `value` attribute, so its contents must go between its open and close tags.
1. **PUT vs. POST** - Our update action will be expecting a PUT HTTP action; however, HTML forms cannot take an action attribute of `PUT` only `POST`. We must find a sensible work around to HTML's shortcomings. We'll use a piece of middleware called `methodOverride` to get the right method. What we'll do is add this middleware and then add `?_method=PUT` onto the end of the action. This way the server will be able to intercept the POST request and set it to PUT and then direct it towards our update action that takes a PUT HTTP request.

Start with the responsive row and columns

```html
<!-- views/events-edit.handlebars -->
<div class="row mt-4">
  <div class="col-lg-6 offset-lg-3">

  </div>
</div>

```

Now add the Card boilerplate

```html

<div class="card">
  <div class="card-body">
    <div class="card-title">
      <h3>Update Event</h3>
    </div>
    <div class="card-text">

    </div>
  </div>
</div>

```

Now lets add the form into the `.card-text` block.

```html
      <form action="/events/{{event.id}}?_method=PUT" method="post">
        <div class="form-group">
          <label for="title">Title</label>
          <input name="title" class="form-control" placeholder="Pickup Basketball" value="{{event.title}}" />
        </div>
        <div class="form-group">
          <label for="imgUrl">Image Url</label>
          <input name="imgUrl" class="form-control" placeholder="" value="{{event.imgUrl}}"/>
        </div>
        <div class="form-group">
          <label for="desc">Description</label>
          <textarea name="desc" class="form-control" placeholder="A description of your event.">{{event.desc}}</textarea>
        </div>
        <div class="text-right">
          <a href="/events/{{event.id}}" class='text-muted mr-2'>Cancel</a>
          <button type="submit" class="btn btn-primary" name="button">Update</button>
        </div>
      </form>
```

If you submit this form now, it will say there is no POST route to this path. We have to initialize the `methodOverride` middleware to direct this route to the PUT update action.

# Update Route

Remember that you needed to intercept this POST request and make sure its processed as a PUT request so it goes to our update action. (this will work for our delete action later too!). We'll use the [method-override]((https://github.com/expressjs/method-override)) middleware.

```bash
$ npm install method-override --save
```

After importing the new package use `require('method-override')` to import it into your project. Add the following at the top of `app.js`.

```js
...
const methodOverride = require('method-override')
```

Now add `methodOverride` as middleware after `const express = require('express')` but before your reoutes.

```js

const express = require('express')
const methodOverride = require('method-override')

...

const app = express()

...

// override with POST having ?_method=DELETE or ?_method=PUT
app.use(methodOverride('_method'))
```

Now you can create your update action and it will receive requests with a PUT method.

```js
// app.js
...

// EVENTS#UPDATE
app.put('/events/:id', (req, res) => {
  models.Event.findById(req.params.id).then(event => {
    event.update(req.body).then(event => {
      res.redirect(`/events/${req.params.id}`);  
    }).catch((err) => {
      console.log(err);
    });
  });
});
```

Try updating your event.

# DRY Code & Sub Templates

Did you notice that the code of our `events-new` and `events-edit` have a lot of similarities? Pretty much everything inside the `form` tag is the same. Let's use a **Partial Template** to pull that code out into its own template.

First make a folder called `partials` inside the `views` folder. Now in that `partials` folder create the `events-form.handlebars`.

```html
<!-- views/partials/events-form.handlebars -->

<div class="form-group">
  <label for="title">Title</label>
  <input name="title" class="form-control" placeholder="Pickup Basketball" value="{{event.title}}" />
</div>
<div class="form-group">
  <label for="imgUrl">Image Url</label>
  <input name="imgUrl" class="form-control" placeholder="" value="{{event.imgUrl}}"/>
</div>
<div class="form-group">
  <label for="desc">Description</label>
  <textarea name="desc" class="form-control" placeholder="A description of your event.">{{event.desc}}</textarea>
</div>
```

And now we can use this partial to replace that information in both our new and edit templates.

```html
<!-- views/events-new.handlebars -->
...
<form action="/events" method="post">
  {{> events-form }}
  <div class="text-right">
    <button type="submit" class="btn btn-primary" name="button">Save</button>
  </div>
</form>
...
```

```html
<!-- views/events-edit.handlebars -->

<form action="/events/{{event.id}}?_method=PUT" method="post">
  {{> events-form }}
  <div class="text-right">
    <button type="submit" class="btn btn-primary" name="button">Update</button>
  </div>
</form>
```

Triumph! DRY code. (Don't Repeat Yourself)

> Reminder - Do not make partials unless the code is very long or repeated. Otherwise you are running the risk of "overengineering" your code.

# Now Commit

```bash
$ git add .
$ git commit -m 'Users can edit and update events'
$ git push
```

# Extra Credit: "Cancel" buttons

Sometimes people might start making a resource and then want to cancel. Can you add a "Cancel" button next to the "Save Event" button? What will it do? Where will it link to?
