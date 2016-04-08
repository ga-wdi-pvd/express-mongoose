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

![Connect to Mongoose](/img/connect-to-mongoose.png)

![Add Seed Data to DB 1](/img/add-seed-to-db-1.png)

![Add Seed Data to DB 2](/img/add-seed-to-db-2.png)

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
