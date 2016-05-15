# Express & Mongoose

## Learning Objectives

* Identify how an Express app fits within the MVC framework.
* Connect an Express app to a MongoDB database.
* Implement CRUD functionality in an Express app using Mongoose.

## Framing (5 minutes / 0:05)

So far in this unit you've learned about a number of tools - Node, Express, MongoDB and Mongoose - that developers can use to build a server-side Javascript application. You have yet, however, to use them all together. We'll be spending the bulk of today's lesson connecting everything and creating an application that can receive HTTP requests, retrieve data/make changes to a database and send information back to the end-user.

### Starter/Solution Code

The starter and solution code are branches in the WhenPresident repo: https://github.com/ga-wdi-exercises/whenpresident/.

If you haven't already, fork and clone the repo and setup the upstream remote.  Your homework will be submitted to the whenpresident repo.

Setup Upstream
  ```bash
  $ git remote add upstream https://github.com/ga-wdi-exercises/whenpresident.git
  ```
Get the latest from upstream.
  ```bash
  $ git fetch upstream
  ```

#### Starter (express-mongoose-starter)

```bash
$ git checkout express-mongoose-starter
```

Create your homework branch
```bash
$ git checkout -b MyName-express-mongoose
```

#### And While You're At It...

Install the modules listed in `package.json` and get Mongo running.

Install dependencies
  ```bash
  $ npm install
  ```

Start mongo  CLI in another tab/window
  ```bash
  $ mongo
  ```

If you need to, start the mongo server - in **another** tab/window
  ```bash
  $ mongod  # Do this one in a separate tab or window.
  ```

## Express Review (25 minutes / 0:30)

> 10 minutes exercise. 15 minutes review.

Take 10 minutes to review the Express application as it stands. As you're going through it, do the following...

### 1. MVC Chart

As you're reviewing the app, try to fill in the blanks in the below Rails-to-Express MVC table. Your job here is to find the ME(A)N equivalents of components in a Rails application. Keep in mind, the answers for many of these are portions of files rather than entire files themselves.

|                       | Rails                      | Express |
|-----------------------|----------------------------|---------|
| **Language**          | Ruby                       |         |
| **Database**          | PostgreSQL                 |         |
| **ORM**               | Active Record              |         |
| **Database (Config)** | `database.yml`             |         |
| **Route**             | `routes.rb`                |         |
| **Controller**        | `candidates_controller.rb` |         |
| **Model**             | `candidate.rb`             |         |
| **View**              | `index.html.erb`           |         |

### 2. Questions

Write down **up to three questions** on topics you would like further clarification on. We will spend 15 minutes going over this (and the MVC chart) afterwards.

## Before We Continue!

You are welcome to code along during the "I Do's" and "We Do's" in this lesson plan. We do ask, however, that **if you fall behind, do not attempt to catch up during those sections**. Instead, tilt down your screen and watch / take notes.

You are more than welcome to catch up when we get to the "You Do's," during which the instructors are available to help.

-----
# Walkthrough

# npm install --save mongoose

> [e3ac57c](https://www.github.com/ga-wdi-exercises/whenpresident/commit/e3ac57c)

Like ActiveRecord for Rails, Mongoose is an ORM we can use to represent data from a Mongo database as models in a Javascript back-end.

### [npm install --save mongoose: `package.json`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/e3ac57c/package.json)
![npm install --save mongoose, package.json](_DIFFSHOTS/npm-install-save-mongoose.package-json.png)

# Connected to Mongoose

> [1b8ff4f](https://www.github.com/ga-wdi-exercises/whenpresident/commit/1b8ff4f)

#### Questions

- We need to connect to and seed our database.  What, at a high-level, is needed for that?
- Fill in the blanks: Rails is to ActiveRecord is to Postgres, as Express is to `_____` is to `_____`.
- What's the difference between `$ mongod` and `$ mongo`? What are they similar to in the Rails world?
- True or false: When you run `mongoose.Schema` it adds new columns to a table in your database.

### [Connected to Mongoose: `db/connection.js`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/1b8ff4f/db/connection.js)
![Connected to Mongoose, db/connection.js](_DIFFSHOTS/connected-to-mongoose.db-connection-js.png)

> **`var mongoose = require("mongoose")`** - In order to reference Mongoose, we need to require its corresponding node module and save it in a variable we can reference later.  

> **`mongoose.Schema( )`** - We use Mongoose's schema method to define a blueprint for our Candidate model (i.e., what attributes it will have and what data types they will be).  

> **`mongoose.model( )`** - We attach our schema to our model by passing in two arguments to this method: (1) the desired name of our model ("Candidate") and (2) the existing schema.  

> **`mongoose.connect`** - We also need to link Mongoose to our `whenpresident` Mongo database.  

# Added seed data

> [f8df0b1](https://www.github.com/ga-wdi-exercises/whenpresident/commit/f8df0b1)

- `connection.js` has `mongoose.model("Candidate", CandidateSchema)`. `seeds.js` has `mongoose.model("Candidate")`. What's the difference between these two statements? (Bonus: What's it similar to in Angular?)
- What does it mean to pass `{}` as an argument into `.remove()`?
- What does `process.exit()` do?

### [Added seed data: `db/connection.js`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/f8df0b1/db/connection.js)
![Added seed data, db/connection.js](_DIFFSHOTS/added-seed-data.db-connection-js.png)

> Notice that `connection.js` no longer contains any reference to seed data. It now only serves as a connection between our application and database.  

### [Added seed data: `db/seed.js`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/f8df0b1/db/seed.js)
![Added seed data, db/seed.js](_DIFFSHOTS/added-seed-data.db-seed-js.png)

> **`var mongoose = require("./connection")`** - Note that this time `var mongoose` is not set to `require("mongoose")`. Instead, it represents a connection to our database.  

> **`var Candidate = mongoose.model("Candidate")`** - Because we defined our model in `connection.js`, we can reference it like so.  
> **`Candidate.remove({})`** - This clears our entire database. We're not passing in any parameters, so Mongoose interprets this command as delete anything!  

> **`Candidate.collection.insert(seedData)`** - Create a collection using the JSON contained in our seed file.  

-----
# STOP
-----

# Shows db candidates in candidates#index

> [a849846](https://www.github.com/ga-wdi-exercises/whenpresident/commit/a849846)

- What's the purpose of the callback function in `Candidate.find().then(callback)`? Why is it necessary?
- What does `res.render` mean? What do we need to pass into it as arguments?
- What does it mean to pass `{}` as an argument into `.find()`?
- Why does our `res.render` statement need to be wrapped in a callback?

### [Shows db candidates in candidates#index: `index.js`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/a849846/index.js)
![Shows db candidates in candidates#index, index.js](_DIFFSHOTS/shows-db-candidates-in-candidatesindex.index-js.png)

> **`Candidate.find({})`** - Retrieves all candidates in the database since we are not passing in any parameters to the method.  
> **`.then(function(candidates){ ... })`** - `candidates` represents the all the Candidates pulled from the database. We can then reference this inside of `.then`.  

> **`candidates: candidates`** - A little confusing, but the `candidates` we will be referencing in our view are now set to the `candidates` that are returned by Mongoose.  

# Shows db candidate in candidates#show

> [05715e4](https://www.github.com/ga-wdi-exercises/whenpresident/commit/05715e4)

* What's the difference between `.find` and `.findOne`?

### [Shows db candidate in candidates#show: `index.js`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/05715e4/index.js)
![Shows db candidate in candidates#show, index.js](_DIFFSHOTS/shows-db-candidate-in-candidatesshow.index-js.png)

-----
# STOP
-----

# npm install --save body-parser

> [53ec895](https://www.github.com/ga-wdi-exercises/whenpresident/commit/53ec895)

In NodeJS, in order to process user input received through a form we will need to install and implement the `body-parser` middleware.

- What does `app.use` do?

### [npm install --save body-parser: `package.json`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/53ec895/package.json)
![npm install --save body-parser, package.json](_DIFFSHOTS/npm-install-save-body-parser.package-json.png)

### [npm install --save body-parser: `index.js`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/53ec895/index.js)
![npm install --save body-parser, index.js](_DIFFSHOTS/npm-install-save-body-parser.index-js.png)

> **`var parser = require("body-parser")`** - Require `body-parser` so we can reference it later.  

> **`app.use(parser.urlencoded({extended: true}))`** - configure the parser to support html forms

# Added nonfunctional post route

> [d596426](https://www.github.com/ga-wdi-exercises/whenpresident/commit/d596426)

Let's create a new candidate form. We'll add it to our existing index view...

- How are `<form>` elements and `req.body` related?
- What two attributes does a `<form>` tag need?  Why?
- What params do we need to access in the route/controller?
- What must be be in the form tag to create those params?
- Why are we accessing `req.body` and not `res.body`?

### [Added nonfunctional post route: `index.js`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/d596426/index.js)
![Added nonfunctional post route, index.js](_DIFFSHOTS/added-nonfunctional-post-route.index-js.png)
### [Added nonfunctional post route: `views/candidates-index.hbs`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/d596426/views/candidates-index.hbs)
![Added nonfunctional post route, views/candidates-index.hbs](_DIFFSHOTS/added-nonfunctional-post-route.views-candidates-index-hbs.png)
 
> **`res.json(req.body)`** - The server will respond with JSON that contains the user input, which is stored in `req.body`. This should look just like the output of Rails APIs you have created in this course.

#### Bonus:

1. Why do we configure body-parser with `{ extended: true}`?
- Is there a form helper library for Express, similar to Rails?
- Is there a library, like SimpleForm, that generates forms for Bootstrap or Zurb Foundation?

# Saves new candidate to database

> [49498dd](https://www.github.com/ga-wdi-exercises/whenpresident/commit/49498dd)

- What's the difference between `res.send`, `res.render`, `res.json`, and `res.redirect`?
- Why would `res.json("hello")` throw an error?

### [Saves new candidate to database: `index.js`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/49498dd/index.js)
![Saves new candidate to database, index.js](_DIFFSHOTS/saves-new-candidate-to-database.index-js.png)

> **`Candidate.create(req.body.candidate)`** - Pass in the name stored in `req.body` as an argument to `.create`.  
>  
> **`res.redirect()`** - Redirect the user to the new candidate's show view. In the callback, `candidate` represents the new candidate in our database.  

-----
# STOP
-----

# Updates a candidate in the database

> [d29bac6](https://www.github.com/ga-wdi-exercises/whenpresident/commit/d29bac6)

- Why can't we use `app.put` or `app.patch` for an "update" route? How is the answer to this question related to HTML forms?
- How come `.findOneAndUpdate` has 3 arguments while `.create` has only 2?
- Why does `method` equal `post` even though we are updating (vs. creating) something?
- How come `.findOneAndUpdate` has 3 arguments while `.create` has only 2?

### [Updates a candidate in the database: `views/candidates-show.hbs`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/d29bac6/views/candidates-show.hbs)
![Updates a candidate in the database, views/candidates-show.hbs](_DIFFSHOTS/updates-a-candidate-in-the-database.views-candidates-show-hbs.png)
### [Updates a candidate in the database: `index.js`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/d29bac6/index.js)
![Updates a candidate in the database, index.js](_DIFFSHOTS/updates-a-candidate-in-the-database.index-js.png)

> **`method="post"`** - Wait, why is this a `POST` method? Aren't we supposed to send a `PUT` or `PATCH` request?  

> **`.findOneAndUpdate()`** - This method takes three arguments: (1) the new params, (2) the candidate to be updated and (3) `new: true`, which causes the modified candidate to be returned in the callback.

#### Bonus

1. How would we support PUT/PATCH in Express?

# Deletes candidate from database

> [cd45919](https://www.github.com/ga-wdi-exercises/whenpresident/commit/cd45919)

- Why can't we use `app.delete` for a `DELETE` route?
- Why shouldn't you use `app.get` for `DELETE` routes?

### [Deletes candidate from database: `index.js`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/cd45919/index.js)
![Deletes candidate from database, index.js](_DIFFSHOTS/deletes-candidate-from-database.index-js.png)
### [Deletes candidate from database: `views/candidates-show.hbs`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/cd45919/views/candidates-show.hbs)
![Deletes candidate from database, views/candidates-show.hbs](_DIFFSHOTS/deletes-candidate-from-database.views-candidates-show-hbs.png)

> Again, **`method="post"`**. What's up with that? 

-----
# STOP
-----

# Added procfile and Mongolab URL for deployment

> [02181c9](https://www.github.com/ga-wdi-exercises/whenpresident/commit/02181c9)

- True or false: the environment variable for your Mongo database's location *must* be `MONGOLAB_URL`.
- What's the value of `process.env.NODE_ENV` on Heroku? On your computer?


### [Added procfile and Mongolab URL for deployment: `db/connection.js`](https://www.github.com/ga-wdi-exercises/whenpresident/blob/02181c9/db/connection.js)
![Added procfile and Mongolab URL for deployment, db/connection.js](_DIFFSHOTS/added-procfile-and-mongolab-url-for-deployment.db-connection-js.png)

#### Steps to get a Mongo database

1. `$ git push heroku master`
- Go to www.mongolab.com and sign up / sign in
- Create a new "Single Node" database with the "Sandbox" tier
- Click on the database
- Click "Users"
- Create a new user. (This is *not* the user with which you logged in to Mongolab.) "User" in this context really means "an app that has access to your database". There's no need for security now; I used the username "test" with a password of "testerson".
- Copy the "To connect using a driver" URL from the top of the Users page.
- Set the URL as an environment variable called MONGOLAB_URL using `heroku config:set` as below, filling in the username and password you just created on the "Users" page. For example:
    ```
    $ heroku config:set MONGOLAB_URL=mongodb://test:testerson@ds015760.mlab.com:15760/yourappname
    ```
- `$ heroku run node db/seed.js`
- `$ heroku open`

## Conclusion

1. What does `module.exports` do?
- Why does `method="post"` even though we are updating (vs. creating) something?
- Why do we use promises and callbacks when calling methods on a Mongoose model?


## Homework

Visit the [WhenPresident repo wiki](https://github.com/ga-wdi-exercises/whenpresident/wiki/Homework) for instructions.

If you get stuck, feel free to review the solution branch (express-mongoose-solution):

```bash
$ git checkout express-mongoose-solution
```
