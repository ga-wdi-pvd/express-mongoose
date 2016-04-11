# Express & Mongoose

## Learning Objectives

* Identify how an Express app fits within the MVC framework.
* Connect an Express app to a MongoDB database.
* Implement CRUD functionality in an Express app using Mongoose.
* Refactor an express app into multiple files.

## Framing (5 minutes / 0:05)

So far in this unit you've learned about a number of tools - Node, Express, MongoDB and Mongoose - that developers can use to build a server-side Javascript application. You have yet, however, to use them all together. We'll be spending the bulk of today's lesson connecting everything and creating an application that can receive HTTP requests, retrieve data/make changes to a database and send information back to the end-user.

### Starter/Solution Code

The starter and solution code are branches in the WhenPresident repo: https://github.com/ga-wdi-exercises/whenpresident/.

#### Starter (express-mongoose-starter)

```bash
$ git checkout express-mongoose-starter
```

#### Solution (express-mongoose-solution)

```bash
$ git checkout express-mongoose-solution
```

#### And While You're At It...

Install the modules listed in `package.json` and get Mongo running.

```bash
$ npm install
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

### ME(A)N Glossary

> Insert definitions for `require`, `module.exports`, `middleware`, `next`, `env`, `seed`, `process.exit`, `app.use`, `req/res`.

## Before We Continue!

You are welcome to code along during the "I Do's" and "We Do's" in this lesson plan. We do ask, however, that **if you fall behind, do not attempt to catch up during those sections**. Instead, tilt down your screen and watch / take notes.

You are more than welcome to catch up when we get to the "You Do's," during which the instructors are available to help.

## Mongoose

### Why are we using Mongoose?

Like ActiveRecord for Rails, Mongoose is an ORM we can use to represent data from a Mongo database as models in a Javascript back-end.

### Connect to Mongoose (10 minutes / 0:45)

In order for us to use Mongoose to communicate with our database, we need to link it up to our Express application.

Q. We need to connect to and seed our database.  What, at a high-level, is needed for that?
---

> A. Database connection, Mongoose model (with schema), seed data.

#### Steps

1. Install Mongoose via the command line: `npm install --save mongoose`.
2. In `connection.js`, require "mongoose" and save it to a `mongoose` variable.
3. Define a `CandidateSchema` using mongoose's `.Schema()` method.
4. Define a "Candidate" model built off `CandidateSchema` with `mongoose.model()`.
5. Connect to our `whenpresident` database using `mongoose.connect()`.

#### Questions

* What argument do we pass into `mongoose.connect()`?
* Does `.Schema()` modify our database? What about `.model()`?
* What does `module.exports` do?

![Connect to Mongoose](/img/connect-to-mongoose.png)

> **`var mongoose = require("mongoose")`** - In order to reference Mongoose, we need to require its corresponding node module and save it in a variable we can reference later.  
>  
> **`mongoose.Schema( )`** - We use Mongoose's schema method to define a blueprint for our Candidate model (i.e., what attributes it will have and what data types they will be).  
>  
> **`mongoose.model( )`** - We attach our schema to our model by passing in two arguments to this method: (1) the desired name of our model ("Candidate") and (2) the existing schema.  
>  
> **`mongoose.connect`** - We also need to link Mongoose to our `whenpresident` Mongo database.  

### Seed the Database (10 minutes / 0:55)

Mongoose is now connected to our Express application. Now let's seed some data into our database using Mongoose.

#### Steps

In `connection.js` we need to...
  1. Remove any references to seed data from `connection.js`.
  2. Set `module.exports = mongoose`.

Now, create a new `db/seed.js` file. In it we will...
  1. Require `connection.js` and `seeds.json`, saving them to their own `mongoose` and `seedData` variables respectively.  
  2. Define a `Candidate` variable that will contain our Mongoose model definition.
  3. Write Mongoose that...
    - Clears the database of all data, and `.then`...
    - Inserts our seed data into the database, and `.then`...
    - Calls `process.exit()`.

We can test this by...
  1. Running `$ node db/seed.js` in the Terminal.
  2. Then run `$ mongo` in the Terminal and enter the following commands via the Mongo CLI interface...
    ```bash
    > use whencandidate
    > db.candidates.find()
    ```

    > You should see data that matches the content of `seeds.json`.

#### Questions

* Why are we able to write out `mongoose.model("Candidate")` in `seed.js`?
* What does it mean to pass `{}` as an argument into `.remove()`?
* What does `process.exit()` do?

![Add Seed Data to DB 1](/img/add-seed-to-db-1.png)

> Notice that `connection.js` no longer contains any reference to seed data. It now only serves as a connection between our application and database.  

![Add Seed Data to DB 2](/img/add-seed-to-db-2.png)

> **`var mongoose = require("./connection")`** - Note that this time `var mongoose` is not set to `require("mongoose")`. Instead, it represents a connection to our database.  
>  
> **`var Candidate = mongoose.model("Candidate")`** - Because we defined our model in `connection.js`, we can reference it like so.  
>  
> **`Candidate.remove({})`** - This clears our entire database. We're not passing in any parameters, so Mongoose interprets this command as delete anything!  
>  
> **`Candidate.collection.insert(seedData)`** - Create a collection using the JSON contained in our seed file.  

### Break (10 minutes / 1:05)

### We Do: Index (10 minutes / 1:15)

First order of business: display all candidates stored in the database.

Q. What has to change (route, controller, model, view)?
---

> A. Just the controller.  We will render the view, after the candidates are returned from the db.


#### Steps

In `index.js`, let's make some changes to our variable definitions...
  1. Rename `db` to `mongoose`. We will be calling Mongoose methods on this variable - this makes more sense semantically!  
  2. Define a `Candidate` model in the exact same way as in `seed.js`.

Now let's move down to our index route...
  1. Use Mongoose to retrieve all Candidates from our database, and `.then`...
  2. Render our existing index view, making sure to set `candidates` (the variable we will be accessing in the view) to the response of our Mongoose method.

#### Questions

* What does `res.render` mean? What do we need to pass into it as arguments?
* What does it mean to pass `{}` as an argument into `.find()`?
* Why does our `res.render` statement need to be wrapped in a callback?

![Index](/img/index.png)

> **`Candidate.find({})`** - Retrieves all candidates in the database since we are not passing in any parameters to the method.  
>  
> **`.then(function(candidates){ ... })`** - `candidates` represents the all the Candidates pulled from the database. We can then reference this inside of `.then`.  
>  
> **`candidates: candidates`** - A little confusing, but the `candidates` we will be referencing in our view are now set to the `candidates` that are returned by Mongoose.  

### You Do: Show (15 minutes / 1:30)

> 10 minutes exercise. 5 minutes review.

So we can show all candidates. You know what's cooler than all candidates? **ONE** candidate.

Q. Again, what has to change?
---

> A. The show route/controller uses our Mongoose model to find and render the requested record.

#### Steps

Let's make changes to our existing show route...
  1. Use a Mongoose method to retrieve the candidates whose name is located in the browser URL. (Hint: use `req.params`). `.then`...
  2. Render the existing show view, making sure to pass in the retrieved candidate as the value to the `candidate` key.

#### Questions

* What's the difference between `.find` and `.findOne`?

![Index](/img/show.png)

### Forms & `body-parser` (10 minutes / 1:40)

In NodeJS, in order to process user input received through a form we will need to install and implement the `body-parser` middleware.  

Install it via the command line -- `npm install --save body-parser` -- then make the following changes to your `index.js` file...  

![Install body-parser](/img/body-parser.png)

> **`var parser = require("body-parser")`** - Require `body-parser` so we can reference it later.  
>  
> **`app.use(parser.urlencoded({extended: true}))`** - configure the parser to support html forms

### You Do: New (10 minutes / 1:50)

> 5 minutes exercise. 5 minutes review.

Let's create a new candidate form. We'll add it to our existing index view...

Q. What did we use in Rails to create an input form?
---

> A. `form_for` helper.  

```
form_for @candidate do |f|
  f.input :name
  f.input :year
  f.submit
end
```

Q. What attributes are important for a form tag?  Why?
---

> A.  action and method.  This defines what route we will submit the form contents to.

Q. What params do we need to access in the route/controller?
---

> A. `{ candidate: { name: "Al Gore", year: 2000 }`

Q. What must be be in the form tag to create those params?
---

> A. input tags will contain `name="candidate[year]"`

#### Steps

1.  Using the `action` attribute, the form should direct to `/candidates`.

#### Questions

* Why do we set the `name` attribute to something like `candidate[name]`? How does this impact how we access this information in `index.js`?

![New non-functional 1](/img/new-non-functional-1.png)

Before we actually create a new candidate in the database, let's make sure we can access the user input submitted through the form.

#### Steps

1. In `index.js`, create an express `POST` route that corresponds with `/candidates`.
2. The route's only content should be a `res.json()` statement that returns the user input. (Hint: this is stored somewhere in `req`).

#### Questions

* How are `<form>` and `req.body` related?
* What does `res.json` do?
* Why are we accessing `req.body` instead of `res.body`?

![New non-functional 2](/img/new-non-functional-2.png)

> **`res.json(req.body)`** - The server will respond with JSON that contains the user input, which is stored in `req.body`. This should look just like the output of Rails APIs you have created in this course.

#### Bonus:

1. Why do we configure body-parser with `{ extended: true}`?
- Is there a form helper library for Express, similar to Rails?
- Is there a library, like SimpleForm, that generates forms for Bootstrap or Zurb Foundation?


### We Do: Create (10 minutes / 2:00)

Let's modify this post route so that it creates a candidate in our database.

#### Steps

1. In `index.js`, use a Mongoose method to create a new candidate. Pass in an argument that contains **only** the candidate's name. (Hint: Again, this is stored somewhere in `req`). `.then`...  
2. Redirect the user to the show view for the newly-created candidate.

#### Questions

* What is `res.redirect`? How is it different from `res.send`, `res.render` and `res.json`?

![Create in DB](/img/new-db.png)

> **`Candidate.create(req.body.candidate)`** - Pass in the name stored in `req.body` as an argument to `.create`.  
>  
> **`res.redirect()`** - Redirect the user to the new candidate's show view. In the callback, `candidate` represents the new candidate in our database.  

### Break (5 minutes / 2:05)

### You Do: Edit/Update (15 minutes / 2:20)

Onto editing and updating candidates. We'll set up a form in our show view to allow users to submit updated candidate information.

#### Steps

1. In `views/candidates-show.hbs`, the form's `action` attribute should direct to our application's show URL.

#### Questions

* Why does `method="post"` even though we are updating (vs. creating) something?

![Edit](/img/update-1.png)

> **`method="post"`** - Wait, why is this a `POST` method? Aren't we supposed to send a `PUT` or `PATCH` request?  

#### Steps

1. In `index.js`, create a `.post` route in `index.js` that corresponds to our new form.
2. In it, use a Mongoose method to find and update the candidate in question. (Hint: Refer to the Mongoose [lesson plan](https://github.com/ga-wdi-lessons/mongoose-intro#update-5-min) or [documentation](http://mongoosejs.com/docs/api.html#query_Query-findOneAndUpdate)).
3. `.then`, redirect the user to the updated candidate's show page.

#### Questions

* How come `.findOneAndUpdate` has 3 arguments while `.create` has only 2?

![Update](/img/update-2.png)

> **`.findOneAndUpdate()`** - This method takes three arguments: (1) the new params, (2) the candidate to be updated and (3) `new: true`, which causes the modified candidate to be returned in the callback.

#### Bonus

1. How would we support PUT/PATCH in Express?


### You Do: Delete (10 minutes)

> We may not get to this during the lesson, but you should be able to implement this yourselves with the instructions below.

We're almost there! Last bit of CRUD functionality we need to implement is `DELETE`. Let's start by adding a delete button to our show view...

#### Steps

1. Using the `action` attribute, the form should direct to `/candidates/:name/delete`.

#### Questions

* Why can't we use `app.delete` for a `DELETE` route?
* Why shouldn't you use `app.get` for `DELETE` routes?

![Delete 1](/img/delete-1.png)

> Again, **`method="post"`**. What's up with that?  

#### Steps

1. In `index.js`, create a route that corresponds to our delete button.
2. In it, use Mongoose to find and delete the candidate in question. (Hint: Refer to the Mongoose [lesson plan](https://github.com/ga-wdi-lessons/mongoose-intro#delete-5-min) or [documentation](http://mongoosejs.com/docs/api.html#query_Query-findOneAndRemove)).

![Delete 2](/img/delete-2.png)

## Homework

???
