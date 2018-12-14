# Styling the Show Template

The show template should make the text relatively narrow, because people don't like reading all the way across the screen. Use an `offset` grid class to push a column over to the right.

```html
<!-- views/events-show.handlebars -->

<a href="/">Back to Home</a>
<div class="row">
  <div class="col-sm-6 col-sm-offset-3">
    <h1>{{event.title}}</h1>
    <h2>{{event.movieTitle}}</h2>
    <p>{{event.description}}</p>
    <p><a href="/events/{{event._id}}/edit">Edit</a></p>
    <p><form method="POST" action="/events/{{event._id}}?_method=DELETE">
        <button class="btn btn-primary" type="submit">Delete</button>
    </form></p>
  </div>
</div>
```

# Styling the Forms

Now let's add some bootstrap styling to our new and edit templates and to our `events-form` template.

```html
<!-- views/events-new.handlebars -->

<div class="row">
  <div class="col-sm-6 col-sm-offset-3">
    <form method="POST" action="/events">
      {{> events-form}}
      <!-- BUTTON -->
      <div class="form-group">
        <button class="btn btn-primary" type="submit">Save Event</button>
      </div>
    </form>
  </div>
</div>
```

Make the same changes to `events-edit.handlebars`:

```html
<!-- views/events-edit.handlebars -->

<div class="row">
  <div class="col-sm-6 col-sm-offset-3">
    <form method="POST" action="/events/{{event._id}}?_method=PUT">
      {{> events-form}}
      <!-- BUTTON -->
      <div class="form-group">
        <button class="btn btn-primary" type="submit">Save Event</button>
      </div>
    </form>
  </div>
</div>

```

Now add some Bootstrap classes to `events-form.handlebars` partial. These class will make your form and form elements look better. Notice that each for element is wrapped in a `div` with the class `form-group` and form elements, like `input` get the class `form-control`.

```html
...
<fieldset>
  <legend>Edit Event</legend>
  <!-- TITLE -->
  <div class="form-group">
    <label for="event-title">Title</label><br>
    <input class="form-control" id="event-title" type="text" name="title" value="{{event.title}}"/>
  </div>

  <!-- MOVIE TITLE -->
  <div class="form-group">
    <label for="movie-title">Movie Title</label><br>
    <input class="form-control" id="movie-title" type="text" name="movieTitle" value="{{event.movieTitle}}" />
  </div>

  <!-- DESCRIPTION -->
  <div class="form-group">
    <label for="event-description">Description</label><br>
    <textarea class="form-control" id="event-description" name="description" rows="10" />{{event.description}}</textarea>
  </div>
</fieldset>
```
