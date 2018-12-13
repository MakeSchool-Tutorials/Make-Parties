---
title: "See All Events"
slug: index-events
---

Following our **User Stories** we are going to define a single **Resource** in this app called an `Event`.

1. Users can view all events (index)
1. Users can create a event (new/create)
1. Users can view one event (show)
1. Users can delete a event (destroy)
1. Users can edit a event (edit/update)
1. Users can comment on events (comments#create)
1. Users can delete comments on events (comments#destroy)

A **Resource** is an abstract object that we use to organize data, code, and the features of our app. For example, a `User` resource we can keep track of logging in and out, email and passwords, and people's birthdays. In a blog, we might have an `Article` or `Post` resource where we would track the titles and bodies of articles and keep track of the code for publishing and sharing them.

Resources can also be related to each other. For example, articles may have comments; a building resource might have floors, and the floors may have units; a user might have friends (like in Facebook).

All resources have a few actions in common that are called **Resourceful Routes**. Memorize this table of routes:

| URL              | HTTP Verb | Action  | What it Does |
|------------------|-----------|---------|---------------|
| /events          | GET       | index   | See all events |
| /events/new      | GET       | new     | See new event form |
| /events          | POST      | create  | Create a new event |
| /events/:id      | GET       | show    | See one event |
| /events/:id/edit | GET       | edit    | See an edit event form |
| /events/:id      | PATCH/PUT | update  | Update a event |
| /events/:id      | DELETE    | destroy | Delete a event |

We're going to start with the index action and then show, and then we'll look at the create, edit, update, and delete.

# Adding a '/events' Route

Let's make a route to `/events` for the index action where we can see all the events that we've created. Eventually this will be on our root route, but we can start making it its own separate path.

```js
// app.js

// OUR MOCK ARRAY OF PROJECTS
var events = [
  { title: "I am your first event", desc: "A great event that is super fun to look at and good", imgUrl: "https://img.purch.com/w/660/aHR0cDovL3d3dy5saXZlc2NpZW5jZS5jb20vaW1hZ2VzL2kvMDAwLzA4OC85MTEvb3JpZ2luYWwvZ29sZGVuLXJldHJpZXZlci1wdXBweS5qcGVn" },
  { title: "I am your second event", desc: "A great event that is super fun to look at and good", imgUrl: "https://img.purch.com/w/660/aHR0cDovL3d3dy5saXZlc2NpZW5jZS5jb20vaW1hZ2VzL2kvMDAwLzA4OC85MTEvb3JpZ2luYWwvZ29sZGVuLXJldHJpZXZlci1wdXBweS5qcGVn" },
  { title: "I am your third event", desc: "A great event that is super fun to look at and good", imgUrl: "https://img.purch.com/w/660/aHR0cDovL3d3dy5saXZlc2NpZW5jZS5jb20vaW1hZ2VzL2kvMDAwLzA4OC85MTEvb3JpZ2luYWwvZ29sZGVuLXJldHJpZXZlci1wdXBweS5qcGVn" }
]

// INDEX
app.get('/events', (req, res) => {
  res.render('events-index', { events: events });
})
```

Notice how we are making a mock array of events, and sending that in as an object into the template `{ events: events }`. So the variable `events` will be available in our template.

# Errors are Your Friends!

Now send your browser to your `/events` path. What do you see? An error!

**That's ok! Errors are our friends**.

What does the error say? I bet it says something like "I can't find the template 'events-index'". That makes sense because we haven't made it yet! It's ok to complete coding tasks that throw a predictable error, and then use that error like a sign post.

> [info]
> Errors often tell you the next step you need to take. In this case the error is telling you that the template does not exist. Let's make it!

So to fix this error, let's add the template `views/events-index.handlebars`. We're going to use the Handlebars.js `{{#each}}` iterator to loop over our array of events and display each one's title.

```html
<!-- events-index -->
<h1>Events</h1>
{{#each events}}
  <h2>{{this.title}}</h2>
  <img class="img-fluid" src="{{this.imgUrl}}" alt="Card image cap">
  <p>{{this.description}}</p>
{{/each}}
```

If you refresh `localhost:3000/events` now what do you see?

> [solution]
> You should see the mock events we wrote into code. Can you add to them or change them?

# Setting the Root Route - '/'

Let's update the `/events` route to be our root route. Just change the path from `/events` to `/` and delete or comment out the hello world root route we made before.

```js
// app.js

// INDEX
app.get('/', (req, res) => {
  res.render('events-index', { events: events });
})
```

Now navigate to `localhost:3000`.

# Now Commit

```bash
$ git add .
$ git commit -m 'Users can see all mock events'
$ git push
```

# An Extra Challenge

Can you make changes to your `events` array in `app.js` and see it reflected in your `events-index` template?
