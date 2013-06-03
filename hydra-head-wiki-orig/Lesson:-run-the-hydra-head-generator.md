This lesson is known to work with hydra-head version 6.0.0.   
_Please update this wiki to reflect any other versions that have been tested._

# Goals
* Add Hydra functionality to your Rails Application

# Explanation
As with Blacklight, in order to add Hydra's features to your application we need to run the custom [Rails generator](http://guides.rubyonrails.org/generators.html) provided by Hydra.  This generator will create and/or modify a handful of files in our application, setting everything up so that we can begin using Hydra's features right away.

# Steps

### Step 1: Run the generator 
```bash
$> rails generate hydra:head -f
```

This overwrites the `app/controller/catalog_controller.rb` from blacklight, with one that is appropriate for the solr schema that we are using.  

### Step 2: Commit your changes
At this point it's a good idea to commit the changes:

```bash
$> git add .
$> git commit -m "Ran blacklight and hydra-head generators"
```

# Next Step
Go on to [[Lesson: Install hydra-jetty]] or return to the [[Dive into Hydra]] page.