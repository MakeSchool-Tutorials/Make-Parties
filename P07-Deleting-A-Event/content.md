---
title: "Delete Route: Destroying a Resource"
slug: deleting-a-review
---

So we've come to the end of our RESTful and Resourceful routes. Only one to go: Delete.

| URL              | HTTP Verb | Action  |
|------------------|-----------|---------|
| /                | GET       | index   |
| /reviews/new     | GET       | new     |
| /reviews         | POST      | create  |
| /reviews/:id     | GET       | show    |
| /reviews/:id/edit     | GET       | edit    |
| /reviews/:id     | PUT/PATCH | update  |
| /reviews/:id     | DELETE    | Destroy |

# What the User Sees

As always, we start with what the users sees and does. So let's make a link to delete a review.

We can't set an `<a>` tag's method (it is always GET) so we are going to use a form to submit a DELETE request to our delete action path.

```html
<!-- views/reviews-show.handlebars -->

<h1>{{review.title}}</h1>
<h2>{{review.movieTitle}}</h2>
<p>{{review.description}}</p>
<p><a href="/reviews/{{review._id}}/edit">Edit</a></p>
<!-- Delete button -->
<p><form method="POST" action="/reviews/{{review._id}}?_method=DELETE">
    <button type="submit">Delete</button>
</form></p>
```

Now we need a delete action route. After deleting the review, it should redirect to the home page (`reviews-index`).

```js
// app.js
...
// DELETE
app.delete('/reviews/:id', function (req, res) {
  console.log("DELETE review")
  Review.findByIdAndRemove(req.params.id).then((review) => {
    res.redirect('/');
  }).catch((err) => {
    console.log(err.message);
  })
})
```

We did it! All **Resourceful Routes** for the `Review` resource are complete!

# Now Commit

```bash
$ git add .
$ git commit -m 'Users can destroy reviews'
$ git push
```

# But Wait!

But there is still one problem. All the review routes are all hanging out in the `app.js` file. This breaks the **Separation of Concerns** principle of clean and **Good Smelling** code. Let's pull out these routes into their own reviews **Controller**.

# Refactoring and Adding a Controller

First make a folder called `controllers`, now add the file `reviews.js` to this folder.

Our `app.js` file is not aware that there is a controllers folder or a reviews controller yet. We have to connect them. There are basically two ways to connect this controller to our app. Either we use the Express Router or JavaScript's modules system. In this case we are going to use the JavaScript ES5 modules system both to introduce it and to avoid getting into the extra and, at this stage, unnecessary details of the Express Router.

`require` can let you connect any file to any other in JavaScript. All you have to do is put `module.exports` into one file, and then `require(name-of-file)` in the other. Let's look at an example in our controller.

```js
//reviews.js

module.exports = function(app, Review) {

  app.get('/', (req, res) => {
    Review.find()
      .then(reviews => {
        res.render('reviews-index', {reviews: reviews});
      })
      .catch(err => {
        console.log(err);
      });
  });

}

```

Now let's import our reviews.js file into our app.js file. We'll pass the `app` and our model `Review` into the file as well.

```js
// app.js

const reviews = require('./controllers/reviews')(app, Review);
```

Test that that worked.

Now migrate the rest of the reviews routes into the `reviews.js` controller file.

# Now The Model

By the end of this chapter we'll have a `views`, `controllers`, and a `models` folders, finally getting a clean `app.js` file and the proper separation of concerns!

Let's move the `Review` model from the `app.js` file into the `models/review.js` file. First make the `models` folder, and then create the `review.js` file inside. Note that model files are singular, while controller files are plural.

Inside the `review.js`, we have to have `mongoose` initialized, and the `Review` code. At the end we want to export it so we can require the model in other files, like our controller.

```js
// models/review.js

const mongoose = require('mongoose');

const Review = mongoose.model('Review', {
  ...
});

module.exports = Review;
```

Instead of including our model `Review` through the `require` statement in our `app.js`, let's put it into the controller via its own require statement.

```js
// app.js

const reviews = require('./controllers/reviews')(app);
```

```js
// controllers/reviews.js

const Review = require('../models/review');
...
```


# Next Step - Making Things Pretty

So now it is refactored and all working It ain't pretty though... let's work on that next.
