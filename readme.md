# Express MVC and the Big Picture

## Learning Objectives

* Identify how an Express app fits within the MVC framework.
* Connect an Express app to a MongoDB database.
* Implement CRUD functionality in an Express app using Mongoose.
* Refactor an express app into multiple files.
* Deploy a "MEN" app.

## Express Review

### Exercise: Build On What You Know - MVC Diagram

### New Details
* What is new in Express that is not covered in the MVC overview?

### Exercise: 99 Bottles with Views (Optional)

> Is there time for this exercise? Does it make sense to include?  

## Mongoose

### Connect to Mongoose

* Why are we using Mongoose?

In order for us to use Mongoose to communicate with our database, we need to link it up to our Express application.

* Do we need Mongo and Mongoose installation instructions here?

![Connect to Mongoose](/img/connect-to-mongoose.png)

> **`var mongoose = require("mongoose")`** - In order to reference Mongoose, we need to require its corresponding node module and save it in a variable we can reference later.  
>  
> **mongoose.Schema( )** - We use Mongoose's schema method to define a blueprint for our Candidate model (i.e., what attributes it will have and what data types they will be).  
>  
> **mongoose.model( )** - We attach our schema to our model by passing in two arguments to this method: (1) the desired name of our model ("Candidate") and (2) the pre-defined schema.  
>  
> **mongoose.connect** - We also need to link Mongoose to our `whenpresident` Mongo database.  

Mongoose is now connected to our Express application. Now let's seed some data into our database using Mongoose.

In `connection.js` we need to...
  1. Remove any references to seed data from `connection.js`.  
  2. Set `module.exports = mongoose`.

Now, create a new `db/seeds.js` file. In it we will...
  1. Require `connection.js` and `seeds.json`, saving them to their own `mongoose` and `seedData` variables respectively.  
  2. Define a `Candidate` variable that will contain our Mongoose model definition.
  3. Write Mongoose code that...
  - Clears the database of all data, and `.then`...
  - Inserts our seed data into the database, and `.then`...
  - Calls `process.exit()`.

![Add Seed Data to DB 1](/img/add-seed-to-db-1.png)

> Notice that `connection.js` no longer contains any reference to seed data. It now only serves as a connection between our application and database.  

![Add Seed Data to DB 2](/img/add-seed-to-db-2.png)

> **var mongoose = require("./connection")** - Note that this time `var mongoose` is not set to `require("mongoose")`. Instead, it represents a connection to our database.  
>  
> **var Candidate = mongoose.model("Candidate")** - Because we defined our model in `connection.js`, we can reference it like so.  
>  
> **Candidate.remove({})** - This clears our entire database. We're not passing in any parameters, so Mongoose interprets this command as delete anything!  
>  
> **Candidate.collection.insert(seedData)** - Create a collection using the JSON contained in our seed file.  

### Index

![Index](/img/index.png)

### Show

![Index](/img/show.png)

### Express Forms
* Review & Deep Dive: Files, Layouts

### Feature: New/Create
* Discuss steps (i.e., 2 requests)

![Install body-parser](/img/body-parser.png)


#### You Do: New
* Scaffolded: Identify each MVC component via T&T, then implement.
* Bonuses

![New non-functional 1](/img/new-non-functional-1.png)

![New non-functional 2](/img/new-non-functional-2.png)

#### We Do: Create
* Cover body-parser
* Bonuses?

![Create in DB](/img/new-db.png)

### Feature: Edit/Update


#### You-Do: Edit
* Hint: POST / PUT / PATCH  

> How much of a hint should we give?  

![Edit](/img/update-1.png)

![Update](/img/update-2.png)

### Feature: Delete

#### You Do: Delete

![Delete 1](/img/delete-1.png)

![Delete 2](/img/delete-2.png)

### Refactor: Extract to Files

> Should this be a You Do? An I Do or We Do if there is time at end of lesson? Do we expect to get to this point in the lesson?  


--------------  

## Skills

### MVC in Express
* Introduce
* Build
* Polish

### Express & Mongoose
* Retrieve
* Create
* Update

### Mongoose Associations / Relationships

### Routes

### Extracting to files: require/exports (who?)

### Views
* Review: config, file location, layout
* New: form to controller (params/bodyParser), post vs. put/patch requests
* Optional: partials, json requests (bodyParser)
