---
title: "Show Route: See One Resource"
slug: showing-one-event
---

We are building out all the **Resourceful Routes** for our `Event` resource.

| URL              | HTTP Verb | Action  |
|------------------|-----------|---------|
| /                | GET       | index   |
| /events/new     | GET        | new     |
| /events         | POST       | create  |
| /events/:id     | GET        | show    |

We've already completed the index, new, and create actions. Now you will add a show action that will display a single resource via it's `id`.

Now let's setup the **show** action so we give each single event its own page and unique url path.

# Show One Event

Remember, always start by building what the user will see and do. To create the show action, you will want to start by making a link to the event from our index action template. Your route has to follow the `/events/:id` structure.

SQL automatically creates an `id` attribute on anything you save. So we can use that `id` attribute for our `:id` in the route. This is called the **Url or Request Parameter** and we access it in Express using the `req.params` parameter inside a controller route.

```html
<!-- views/events-index.handlebars -->

...
        <a href="/events/{{this.id}}" class="btn btn-primary">View</a>
...
```

What happens if you click on that link? A friendly error! Let's do what it says and make the route.

```js
// app.js

...

// SHOW
app.get('/events/:id', (req, res) => {
  res.send('I\'m a event')
});
```

Now what happens if you go to that route? It should say the text we are sending back. That's good. That means the route is working. Now let's take the next step.

> Taking baby steps like this is a GREAT habit as a software engineer. Aim to take many small steps at a good pace.

# req.params & Event.findById()

Ok time to add a template with an actual `event` object!

```js
// app.js

...

// SHOW
app.get('/events/:id', (req, res) => {
  models.Event.findById(req.params.id).then((event) => {
    res.render('events-show', { event: event })
  }).catch((err) => {
    console.log(err.message);
  })
})
```

Now if we go to the route, we'll see the error that no template `events-show` is found. That's great! We knew that didn't exist, so we were expecting this error. The program is working! Let's make the template now.

```html
<!-- views/events-show.handlebars -->

<img src={{event.imgUrl}}/>
<h1>{{event.title}}</h1>
<h2>{{event.desc}}</h2>

```

Now what do you see? All the links to events should work now.

# Add a Back Link

This is good, the show action is working, but there is a bit of a problem. Once you are on the show action page, you can't get back home. Let's fix that by putting in a "Back" link.

```html
<!-- views/events-show.handlebars -->

<a href="/">Back to Home</a>

<img src={{event.imgUrl}}/>
<h1>{{event.title}}</h1>
<h5>{{event.desc}}</h5>

```

That's better. What else could we do now that we have this show route?

# Update the Create Action's Redirect

It makes sense from the user's perspective that after we create a new event, we should be automatically redirected to it, no? Let's change our create route to redirect to the show path.

```js
// CREATE
...
    res.redirect(`/events/${event.id}`) // Redirect to events/:id
...
})
```

# Adding some Bootstrap

Let's make that show template a little prettier with some bootstrap.

We'll wrap it in a resposive row and columns with an offset so it its in the middle 6 columns of our container. We'll also add some classes that make the image responsively the full width (`w-100` = width 100%), rounded corners, and a margin bottom level 3. And we'll use the `.lead` class to fancy-up the description text.

```html
<div class="row mt-4">
  <div class="col-lg-6 offset-lg-3">
    <img class="img-fluid w-100 rounded mb-3" src="{{event.imgUrl}}" alt="Card image cap">
    <h3>{{event.title}}</h3>
    <div class="lead">{{event.desc}}</div>
  </div>
</div>
```

Check out your handywork and let's commit.

# Now Commit

```bash
$ git add .
$ git commit -m 'Users can see single events'
$ git push
```

Now our user experience is getting very smooth, and our code is getting more and more complete. Onward!
