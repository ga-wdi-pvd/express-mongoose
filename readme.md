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

<details>
  <summary><strong>What argument do we pass into `mongoose.connect()`?</strong></summary>

  > The location of the Mongo database.

  <br/>

</details>

<details>
  <summary><strong>Does `.Schema()` modify our database? What about `.model()`?</strong></summary>

  > No. It's only once we start querying the database that it changes.

  <br/>

</details>

<details>
  <summary><strong>What does `module.exports` do?</strong></summary>

  > It allows us to export code from one file to another. We could export only portions of the code or all of it if we wanted to.

  <br/>

</details>

![Connect to Mongoose](http://i.imgur.com/g1LnWzx.png)

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
    ```mongo
    > use whencandidate
    > db.candidates.find()
    ```

    > You should see data that matches the content of `seeds.json`.

#### Questions

<details>
  <summary><strong>Why are we able to write out `mongoose.model("Candidate")` in `seed.js`?</strong></summary>

  > (CHECK ANSWER) Because we defined that candidate in `connection.js`, which has been required.

  <br/>

</details>

<details>
  <summary><strong>What does it mean to pass `{}` as an argument into `.remove()`?</strong></summary>

  > To remove everything. An empty object means that we're not going to restrict removal to certain key-value pairs.

  <br/>

</details>

<details>
  <summary><strong>What does it mean to pass `{}` as an argument into `.remove()`?</strong></summary>

  > Closes the file and prevents it from running any further.

  <br/>

</details>

![Add Seed Data to DB 1](http://i.imgur.com/zWHSpRO.png)

> Notice that `connection.js` no longer contains any reference to seed data. It now only serves as a connection between our application and database.  

![Add Seed Data to DB 2](http://i.imgur.com/qzFr8AI.png)

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

<details>
  <summary><strong>What does `res.render` mean? What do we need to pass into it as arguments?</strong></summary>

  > `res.render` is used to render the server response back to the browser. In this example we pass it (a) the view we want to render and (b) the data that should be available to it (i.e., candidates).

  <br/>

</details>

<details>
  <summary><strong>What does it mean to pass `{}` as an argument into `.find()`?</strong></summary>

  > Like `.remove`, it means to find everything. The search is not limited to certain key-value pairs.

  <br/>

</details>

<details>
  <summary><strong>Why does our `res.render` statement need to be wrapped in a callback?</strong></summary>

  > We want to wait until the Mongoose query has been completed before we render anything, especially if the render is dependent on data returned from the database.

  <br/>

</details>

![Index](http://i.imgur.com/QBz4ikv.png)

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

<details>
  <summary><strong>What's the difference between `.find` and `.findOne`?</strong></summary>

  > `.find` returns multiple items. `.findOne` only returns one.

  <br/>

</details>

![Show](http://i.imgur.com/OCIL9H5.png)

### Forms & `body-parser` (10 minutes / 1:40)

In NodeJS, in order to process user input received through a form we will need to install and implement the `body-parser` middleware.  

Install it via the command line -- `npm install --save body-parser` -- then make the following changes to your `index.js` file...  

![Install body-parser](http://i.imgur.com/ZWSc01J.png)

> **`var parser = require("body-parser")`** - Require `body-parser` so we can reference it later.  
>  
> **`app.use(parser.urlencoded({extended: true}))`** - configure the parser to support html forms

### You Do: New (10 minutes / 1:50)

> 5 minutes exercise. 5 minutes review.

Let's create a new candidate form. We'll add it to our existing index view...

#### Before You Start Coding...

<details>
  <summary><strong>What did we use in Rails to create an input form?</strong></summary>

  > `form_for` helper.  

  ```erb
  form_for @candidate do |f|
    f.input :name
    f.input :year
    f.submit
  end
  ```

  <br/>

</details>

<details>
  <summary><strong>What attributes are important for a form tag?  Why?</strong></summary>

  > `action` and `method`.  This defines what route we will submit the form contents to.  

  <br/>

</details>

<details>
  <summary><strong>What params do we need to access in the route/controller?</strong></summary>

  > `{ candidate: { name: "Al Gore", year: 2000 }`  

  <br/>

</details>

<details>
  <summary><strong>What must be be in the form tag to create those params?</strong></summary>

  > Input tags will contain `name="candidate[year]"`.

  <br/>

</details>

#### Steps

1.  Using the `action` attribute, the form should direct to `/candidates`.

#### Questions

<details>
  <summary><strong>Why do we set the `name` attribute to something like `candidate[name]`? How does this impact how we access this information in `index.js`?</strong></summary>

  > All candidate information will be available to us inside of a `candidate` object on the back-end.

  <br/>

</details>

![New non-functional 1](http://i.imgur.com/JqhY57R.png)

Before we actually create a new candidate in the database, let's make sure we can access the user input submitted through the form.

#### Steps

1. In `index.js`, create an express `POST` route that corresponds with `/candidates`.
2. The route's only content should be a `res.json()` statement that returns the user input. (Hint: this is stored somewhere in `req`).

#### Questions

<details>
  <summary><strong>How are `<form>` and `req.body` related?</strong></summary>

  > (CHECK ANSWER) The values a user submits through the form can be found in `req.body` on the back-end.

  <br/>

</details>

<details>
  <summary><strong>What does `res.json` do?</strong></summary>

  > It's sends a response back to the browser in JSON form. This functions similarly to `format.json` in Rails.

  <br/>

</details>

<details>
  <summary><strong>Why are we accessing `req.body` instead of `res.body`?</strong></summary>

  > Because we want to render whatever the user sent through the form as JSON.

  <br/>

</details>

![New non-functional 2](http://i.imgur.com/iyxCyZC.png)

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

<details>
  <summary><strong>What is `res.redirect`? How is it different from `res.send`, `res.render` and `res.json`?</strong></summary>

  > (CHECK ANSWER) `res.redirect` initializes a new request-response cycle and usually is not passed any data from the controller.

  <br/>

</details>

![Create in DB](http://i.imgur.com/hqKzbWa.png)

> **`Candidate.create(req.body.candidate)`** - Pass in the name stored in `req.body` as an argument to `.create`.  
>  
> **`res.redirect()`** - Redirect the user to the new candidate's show view. In the callback, `candidate` represents the new candidate in our database.  

### Break (5 minutes / 2:05)

### You Do: Edit/Update (15 minutes / 2:20)

Onto editing and updating candidates. We'll set up a form in our show view to allow users to submit updated candidate information.

#### Steps

1. In `views/candidates-show.hbs`, the form's `action` attribute should direct to our application's show URL.

#### Questions

<details>
  <summary><strong>Why does `method="post"` even though we are updating (vs. creating) something?</strong></summary>

  > HTML does not support `PUT` or `PATCH`. That being said, we can make a `POST` request and define behavior on the back-end that will actual update something instead of create.

  <br/>

</details>

![Edit](http://i.imgur.com/74vYqMa.png)

> **`method="post"`** - Wait, why is this a `POST` method? Aren't we supposed to send a `PUT` or `PATCH` request?  

#### Steps

1. In `index.js`, create a `.post` route in `index.js` that corresponds to our new form.
2. In it, use a Mongoose method to find and update the candidate in question. (Hint: Refer to the Mongoose [lesson plan](https://github.com/ga-wdi-lessons/mongoose-intro#update-5-min) or [documentation](http://mongoosejs.com/docs/api.html#query_Query-findOneAndUpdate)).
3. `.then`, redirect the user to the updated candidate's show page.

#### Questions

<details>
  <summary><strong>How come `.findOneAndUpdate` has 3 arguments while `.create` has only 2?</strong></summary>

  > Because we need to identify the thing we are updating **AND** what it's going to be updated with.

  <br/>

</details>

![Update](http://i.imgur.com/rtQGmQi.png)

> **`.findOneAndUpdate()`** - This method takes three arguments: (1) the new params, (2) the candidate to be updated and (3) `new: true`, which causes the modified candidate to be returned in the callback.

### You Do: Delete (10 minutes)

> We may not get to this during the lesson, but you should be able to implement this yourselves with the instructions below.

We're almost there! Last bit of CRUD functionality we need to implement is `DELETE`. Let's start by adding a delete button to our show view...

#### Steps

1. Using the `action` attribute, the form should direct to `/candidates/:name/delete`.

#### Questions

<details>
  <summary><strong>Why can't we use `app.delete` for a `DELETE` route?</strong></summary>

  > Again, because HTML only supports `GET` and `POST`, not `PUT` `PATCH` or `DELETE`.

  <br/>

</details>

<details>
  <summary><strong>Why shouldn't you use `app.get` for `DELETE` routes?</strong></summary>

  > (NEED ANSWER).)

  <br/>

</details>

![Delete 1](http://i.imgur.com/76mp0U4.png)

> Again, **`method="post"`**. What's up with that?  

#### Steps

1. In `index.js`, create a route that corresponds to our delete button.
2. In it, use Mongoose to find and delete the candidate in question. (Hint: Refer to the Mongoose [lesson plan](https://github.com/ga-wdi-lessons/mongoose-intro#delete-5-min) or [documentation](http://mongoosejs.com/docs/api.html#query_Query-findOneAndRemove)).

![Delete 2](http://i.imgur.com/1qF64Tf.png)

## Conclusion

1. What does `module.exports` do?
- Why does `method="post"` even though we are updating (vs. creating) something?
- Why do we use promises and callbacks when calling methods on a Mongoose model?


## Homework

## TODOS

* Add deployment info from most recent lesson.
* Grab updated conclusion from most recent lesson.
* Update homework so it says that students can now complete the second part of YUM.
* Notes for students before reviewing WhenPresident
  - Promises vs. Callbacks
  - Different ways of making Mongoose queries
* Prompt: if you want a challenge, only follow the steps (i.e., don't look at the screenshots).
