This lesson is known to work with hydra-head version 6.0.0.   
_Please update this wiki to reflect any other versions that have been tested._

# Goals
* Add Blacklight functionality to your Rails Application

# Explanation

Hydra builds on and extends the features provided by Blacklight, so it's important that we first put those Blacklight features into our application.  To do this, we run the custom [Rails generator](http://guides.rubyonrails.org/generators.html) provided by Blacklight.  The generator creates a number of files in your application that will allow you to use and modify Blacklight's features in your application.

**Tip:** If you want to see clearly what changes that the generator has made, make sure that before you run the generator all of your current changes have been checked into git -- so before running the generator running 'git status' should report that there are no changes ("nothing to commit").  Then when you run the generator you will be able to see all of the changes it's made by runing 'git status'.

# Steps

**Note:** You must have completed the steps in [[Lesson: Add the Hydra Dependencies]] in order for the following steps to work.


### Step 1: Run the code generator provided by the Blacklight gem.  

We do this by typing 

```bash
$> rails generate blacklight --devise
```

The `--devise` flag tells the generator to install the [devise](https://github.com/plataformatec/devise) gem, which takes care of authentication.

The blacklight generator has created a few database migrations that store users, saved searches and bookmarks. Additionally, it's installed the devise gem and the bootstrap gem.  It's created an important file in our application `app/controllers/catalog_controller.rb`.  This is the primary place where you configure the blacklight search.

### Step 2: Run the generated database migrations

At this point we should run the migrations, so that the database tables are built.  To do this enter the following command: 

```bash
$> rake db:migrate
```

# Next Step
Go on to [[Lesson: Run the Hydra-Head generator]] or return to the [[Dive into Hydra]] page.