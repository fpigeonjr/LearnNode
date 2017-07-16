# LearnNode

## Sample Data

To load sample data, run the following command in your terminal:

```bash
npm run sample
```

If you have previously loaded in this data, you can wipe your database 100% clean with:

```bash
npm run blowitallaway
```

That will populate 16 stores with 3 authors and 41 reviews. The logins for the authors are as follows:

|Name|Email (login)|Password|
|---|---|---|
|Wes Bos|wes@example.com|wes|
|Debbie Downer|debbie@example.com|debbie|
|Beau|beau@example.com|beau|

## Lesson 1

1st step is to run `npm install` or `yarn install`to install all the depenencies.

## Lesson 2

Setup mongo database using mLab and then installing local database gui MongoDB Compass.

Setup `variables.env` file that contains database credentials.

## Lesson 3

**Express** = fast minimal framework for Node <br>
**Mongoose** = interact with MongoDB

`start.js` file contains:

* brings in Mongoose
* env variables for sensitive information
* .env files should not be checked into version control

`yarn start` to kick off server at `localhost:7777`

## Lesson 4

Learned about routes and how express uses req and respond.

`index.js` contains routes code <br>
routes are futher broken down in the `app.js` <br>

* `req` has all the information
* `resond` has all the methods for sending data back
* `query` has all the query params from url
* `params` can access items from url
* `json` displays array in JSON format

To get the date from the url

`url?name=frank`

You could use this

`res.send(req.query.name)`

Learn more on the [Express Docs][express docs]

## Lesson 5

* `res.render()` renders out a template
* template language we are using is called Pug (formerly named Jade)
* `views` directory contains all our Pug files

### Pug

* is tab-based
* don't need a closing tag
* attributes inside () like this and js template literals

```html
img(src="dog.jpg", alt=`Dog ${dog}`)
```

### Layouts

Our main layout file is `layout.pug` and we can extend it to our other pages.

We can inject our content to override the layout by looking for the `block` sections in our layout files

```js
extends layout

block content
    p some new content
```

## Lesson 6 : Template Helpers

`helpers.js` holds data that we'll use in every template. Middleware is in our `app.js` file and its purpose is to allow on every single request to put information in our locals.

This is done by exporting our `helpers.js` and importing them in our `app.js`

Helpers can include:

* arrays
* strings
* libraries like [Moment.js][moment]

Then in our pug file we can access it like this:

```html
p.sale Sale in ends in #{h.moment().endOf('day').fromNow()}!
```

## Lesson 7: Controllers

Controllers are the traffic cops between the Model and the View.

`controllers` dirctory contains all controller files and should create one for each area of the page.

Routes should shell off to seperate controller file to do the actual work.

## Lesson 8: Middleware Intro

* Express's middleware is similar to React's lifecycle hooks.
* Helpful when you want to run code after `req` but before the `res`
* there is also ES6 global middleware
* middleware for our app is stored in the `app.js`
* global middleware is noted by `app.use`

```js
// The flash middleware example
app.use(flash());
```

## Lesson 9: Models

MongoDB is a loose database so you don't need to specify data types 

* it is strict by default so we will need to define the schema
* models are stored in the `models` directory and are uppercase
* `models.Store.js` for our Store model
* we're using **Mongoose** to interface with MongoDB using the built-in ES6 promises 
* **Slug** library to make url friendly names
* we let MongoDB know about the model by adding it to our `start.js`

```js
// Import all of our models
require('./models/Store')
```

Slugs will be auto-generated into the `Store.js` model by using a pre-save hook

```js
storeSchema.pre('save', function(next) {
    if ( !this.isModified('name') ) {
        next() // skip it
        return // stop this function from running
    }
    this.slug = slug (this.name)
    next()
    // TODO: Make more resiliant so slugs are unique 
})
```

[express docs]: https://expressjs.com/en/guide/routing.html
[moment]: http://momentjs.com/