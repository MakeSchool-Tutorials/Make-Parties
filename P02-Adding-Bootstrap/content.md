---
title: "Styling with Bootstrap"
slug: adding-bootstrap
---

Now that you have the user's experience mapped out and the functionality built, you can style your pages using Twitter's project [Bootstrap](http://getbootstrap.com/). Bootstrap is the web's most popular **Design System**.

The main elements we will be using are the...

* Grid
* Navbar
* Forms
* Inputs
* Buttons

There is a lot more that you can explore and even extend and modify.

# Adding Bootstrap with a Content Delivery Network (CDN)

It is easy to get started with bootstrap quickly by using the CDN links bootstrap maintains.

We'll add the `<link>` to bootstrap's css in our `<head>` tag in the `main.handlebars` file. And although we aren't going to use any of bootstrap's JavaScript's components, for completeness's sake, we'll put the JavaScript `<script>` tag just before the closing `</body>` tag.

```html
<!-- views/layouts/main.handlebars -->

<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  Make Parties
  <!-- Latest compiled and minified CSS -->
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
</head>
<body>

  {{{body}}}

  <!-- Latest compiled and minified JavaScript -->
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</body>
</html>
```

Just after doing that, refresh your page. The typography and layout should change just a tiny bit if it worked.

# Adding a Navbar

Add the most common navigational component - a top navbar. We'll have it have a button to create a new event so that anywhere a user navigates to they can always make a new event. If you shrink and grow the size of your browser window you'll notice that this is **responsive** navbar, meaning it responds to the size of the screen.

Make a new partial. Create a new file `navbar.handlebars` in the `views/partials` directory.

```html
<!-- views/partials/navbar.handlebars -->

<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <a class="navbar-brand" href="#">Make Parties</a>

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
```
Include this in `views/layouts/main.handlebars`

```html
...
<body>

  {{> navbar}}

  {{{body}}}
 ...
```

# Adding a Grid Container

People argue about the usefulness of some of the more complex parts of Bootstrap, but everyone agrees that to build websites you need a grid system. Bootstrap ships with a 12-column grid. Meaning you can break up the whole page, or any element into 12 columns and that way construct an appealing layout.

We'll start by wrapping the `{{{body}}}` with a `container` class.

```html
<!-- views/layouts/main.handlebars -->
...
<body>
  ...
  <div class="container">
    {{{body}}}
  </div>
...
```

Refresh and see that this gives us some satisfying gutters on the sides of the page. If we change the screensize by resizing the window, we'll see that these gutters vanish when our screen shrinks beyond a certain size. This is because Bootstrap's grid system is als **Responsive**—meaning it changes depending on the size of the screen that is being shown on. Pretty neat!

# Adding a Responsive Grid with Cards

Let's add some bootstrap to make these look better. First we'll wrap things in a row (with a margin-top utility class) and some responsive columns so the events sit in the middle 6 columns of the page when the browser window is `lg` (large).

```html
<div class="row mt-4">
  <div class="col-lg-6 offset-lg-3">

  </div>
</div>
```

Next we'll add some Bootstrap Card enhanced html inside:

```html
{{#each events}}
  <div class="card mt-3">
    <img class="card-img-top" src="{{this.imgUrl}}" alt="Card image cap">
    <div class="card-body">
      <h5 class="card-title">{{this.title}}</h5>
      <p class="card-text">{{this.desc}}</p>
      <div class="text-right">
        <a href="#" class="btn btn-info">Rsvp</a>
        <a href="#" class="btn btn-primary">View</a>
      </div>
    </div>
  </div>
{{/each}}
```

How does she look?

# Now Commit

```bash
$ git add .
$ git commit -m 'Added vanilla bootstrap'
$ git push
```

# Onward! Almost Done!

Almost done! Let's finish it off.
