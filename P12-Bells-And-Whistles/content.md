---
title: "Bells and Whistles"
slug: bells-and-whistles
---

Now there are a few more things we can do to tighten up our website and make it look more like the big leagues. Let's look at a few.

# Displaying "Created At" Time

Let's display a "timestamp" for when a Review was created that looks like this: "Created on Nov 3, 2018".

First we have to update our model to track when documetns are created and updated. We is so common, that there is an established convention in Mongoose called "timestamps" to accomplish it. We just have to add the additional option of `{ timestamps: true }` to the schema of our model and then all documents will have a `createdAt` and `updatedAt` Date fields added and set correctly by default.

```js
const Review = mongoose.model('Review', {
  ...
}, {
  timestamps: true
});
```

Now we can display that `createdAt` timestamp in our html.

> [action]
>
```html
<!-- views/reviews-index.handlebars -->

<h1>Reviews</h1>

<div class="row">
  {{#each reviews}}
    <div class="col-sm-3">
      <h2><a href="/reviews/{{this._id}}">{{this.title}}</a></h2>
      <small>{{this.movieTitle}}</small>
      <small class="text-right text-muted">{{this.createdAt}}</small>
    </div>
  {{/each}}
</div>
```
> Now create a new review and see what is displayed.

Uh-oh - what you see there is called a Unix timestamp.

```
Mon Nov 26 2018 12:57:54 GMT-0800 (Pacific Standard Time)
```

It technically says the date and time when the review was created, but it isn't very readable for humans! We could parse it manually, but let's use a neat and very common js library called [moment](https://momentjs.com/) to parse that time into something more readable.

Buuuuuuuuut, handlebars is a strictly **logicless** templating engine, meaning that it does not allow any logical functions to be performed in the templates. Ever.

Thankfully someone wrote a helper module that wraps moment in handlebars, allowing you to call a `moment()` sort of helper inside of a handlebars template.

We're going to install `helper-moment` then make it available in our project using the `app.locals` method that sets local variables across your whole application. Anything in `app.locals` will be available in your controllers and templates.

> [action]
> Install helper-moment
```bash
$ npm i helper-moment --save
```
> Now In your middleware section of your `app.js` file add moment and put it into a variable called `app.locals.momemt`.
```js
// app.js
...

const moment = require('helper-moment');
app.locals.moment = moment;

...
```

Now we can use helper-moment to parse the date.

```html
<small class="text-right text-muted">{{moment this.createdAt 'MM DD YYYY'}}</small>
```

> [action]
> Now use the [moment documentation](https://momentjs.com/) to tweek the **format string** until you are happy with how the text displays.

# Adding a Footer

Now let's add a footer (Brought to you by mdbootstrap.com). Add the following code after the `{{{body}}}` tag but before the `</body>` tag or any `<script>` tags.

```html
<!-- main.handlebars -->
...

<!-- Footer -->
<footer class="page-footer font-small blue pt-4">
  <div class="container-fluid text-center text-md-left">
    <div class="row">
      <div class="col-md-6 mt-md-0 mt-3">
        <h5 class="text-uppercase">Footer Content</h5>
        <p>Here you can use rows and columns here to organize your footer content.</p>
      </div>
      <hr class="clearfix w-100 d-md-none pb-3">
      <div class="col-md-3 mb-md-0 mb-3">
          <h5 class="text-uppercase">Links</h5>
          <ul class="list-unstyled">
            <li><a href="#!">Link 1</a></li>
            <li><a href="#!">Link 2</a></li>
            <li><a href="#!">Link 3</a></li>
            <li><a href="#!">Link 4</a></li>
          </ul>
        </div>
        <div class="col-md-3 mb-md-0 mb-3">
          <h5 class="text-uppercase">Links</h5>
          <ul class="list-unstyled">
            <li><a href="#!">Link 1</a></li>
            <li><a href="#!">Link 2</a></li>
            <li><a href="#!">Link 3</a></li>
            <li><a href="#!">Link 4</a></li>
          </ul>
        </div>
    </div>
  </div>
  <div class="footer-copyright text-center py-3">© 2018 Copyright:
    <a href="https://mdbootstrap.com/education/bootstrap/"> MDBootstrap.com</a>
  </div>
</footer>

...
```

Beautiful. Make any visual changes you like.


# Adding a Bootstrap Theme

Now if we want to customize our style a little bit, we can add a free bootstrap theme. One popular website for finding these is called [Bootswatch](https://bootswatch.com/).

You can pick whichever you like, but for these instructions we'll use the "flatly" theme.

> [action]
> Go to the flatly theme and select the download the boostrap.min.css. [link](https://bootswatch.com/4/flatly/bootstrap.min.css)
>
> Save that code into a file in the location: public/vendor/flatly.min.css
>
> Now add a link to that theme in your `<head>` tag. Make sure that this theme comes AFTER your link to bootstrap itself. Otherwise it won't work.

```html
<head>
  ...

  <link rel="stylesheet" href="/vendor/flatly.min.css">
</head>
```

Now you should see that all the bootstrap styles are updated to follow the theme that you chose.

# Initialize, Commit, and Push

Now that you have a basic initialized Express.js project, let's commit to github.

```bash
$ git init
$ git add .
$ git commit -m 'init'
```

Now go to github and create a public repository called `Rotten-Potatoes-Tutorial`, and now associate it as a remote for your local git project and then push to it.

```bash
$ git remote add origin GITHUB-REPO-URL
$ git push origin master -u
```
