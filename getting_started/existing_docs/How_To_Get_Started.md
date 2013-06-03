# Getting Started Building Your Own Hydra Application

## Before You Begin

This document is a work in progress. We are actively seeking feedback. If you run into anything that is incorrect or confusing, please email the [hydra-tech](http://groups.google.com/group/hydra-tech) mailing list and let us know.

## What you will learn from this document

1.  Set up a Hydra Head (a Rails app that uses the hydra-head plugin) and run it
2.  Understand how Hydra fits into and uses Rails MVC (Model, View, Controller) structures

### If you get stuck

Try asking questions on the [hydra-tech](http://groups.google.com/group/hydra-tech) mailing list, or in the \#projecthydra IRC channel on Freenode.

## Set up a new Hydra Head

Make sure you have looked over the [prerequisites](https://github.com/projecthydra/hydra-head/wiki/Installation-Prerequisites) .

See the [README](https://github.com/projecthydra/hydra-head/blob/master/README.textile) for instructions on creating a new Hydra Head.

We will be using both RSpec and Cucumber in this Tutorial, so make sure to follow the instructions in the “RSpec and Cucumber for Testing” section of [Tools for Developing and Testing Your Application](https://github.com/projecthydra/hydra-head/wiki/Tools-for-Developing-and-Testing-your-Application)

### Starting the App out of the Box

#### (1) Migrate the development databases.

From the rails application root directory, run

    rake db:migrate

Don’t worry if you’ve already run the migrations. It’s safe to re-run rake db:migrate.

#### (2) Start Jetty, pre-configured with Fedora and Solr

*Stop any copies of jetty before running this command.*
(Note that java 1.6 must be invoked by the “java” command or Fedora won’t work.)

You may already have installed jetty as a submodule per the Hydra-Jetty section of [Tools for Developing and Testing Your Application](https://github.com/projecthydra/hydra-head/wiki/Tools-for-Developing-and-Testing-your-Application)

    git clone git://github.com/projecthydra/hydra-jetty.git jetty
    rake hydra:jetty:config
    rake jetty:start

Note: If you are using a pre-5.0 version of hydra-head or a pre-existing code base, your solr\_conf and fedora configs will be incompatible with the copies of Fedora and Solr in the latest version of hydra-jetty. To fix this, you can either update your solr\_conf and fedora configs (grab them from the master branch of hydra-head) or pin your submodule to the 4.x branch of hydra-jetty.

    # Don’t do this if you don’t need to
    cd jetty
    git checkout 4.x
    cd ..
    git add jetty
    git commit ~~m“pinning hydra-jetty to 4.x branch”

This will start up a pre-configured jetty instance running Fedora and Solr on port 8983.

Ensure Solr is running. You should see a Solr admin page here: [http://localhost:8983/solr/development/admin/](http://localhost:8983/solr/development/admin/) . You should be able to click on the Statistics link and there should be more than 0 documents in the index if you loaded any fixtures. Or look for documents in http://localhost:8983/solr/development/select?q={!defType=dismax}**:** if you have any fixtures loaded.

Ensure Fedora is running. You should see the Fedora Repository Information View here: [http://127.0.0.1:8983/fedora/describe](http://127.0.0.1:8983/fedora/describe) should show the Fedora Repository Information View. The following query should return objects: [http://localhost:8983/fedora/objects?pid=true&title=true&terms=&query=](http://localhost:8983/fedora/objects?pid=true&title=true&terms=&query=)

#### Start the Rails Application

    rails server

To ensure the rails app is running, go to [http://localhost:3000/](http://localhost:3000/)

On some systems, the http://localhost:3000 address will be broken because WEBrick binds to 0.0.0.0 rather than 127.0.0.1.  To get around this, you can tell the server which address to bind to using the `-b` flag. ie. `rails server -b 127.0.0.1`

If Rails still does not seem to be working properly, consult the output on the command line to see if there are any errors.

## Tutorial: Code4Lib Walkthrough

The ] is a tutorial originally presented at Code4Lib 3012 in Chicago. In addition to walking through the initial hydra head setup, it shows how to create a couple of simple models with a 1-to-many relationship between them, and how to create datastream objects for them. Very helpful for beginners.

## Hydra and Rails MVC

In an MVC framework, the Model defines the attributes and behaviors of your various objects, allowing you to persist those objects and retrieve them. By default, Rails Models uses ActiveRecord to persist & retrieve objects using SQL databases. With Hydra, we use ActiveFedora to connect with Fedora and Solr instead of a SQL database.

Controllers handle requests from clients , loading the necessary information and rendering the appropriate responses . They use your Models to load the information and they use your Views to render the response. In this way, Controllers are like connectors or coordinators — they coordinate the flow of activity in your application when it receives requests.

## Making local changes to your Hydra Application

In order to make it easy to get any new functionality added to the hydra stack while retaining your Hydra application’s localizations, your local hydra application code should be set up to override the upstream Hydra stack code.

Luckily, rails engines has made this easy~~ the Hydra code is organized so your localizations are kept separate from the core hydra application code.

Moreover, to ensure your localizations won’t be broken by upgrading the Hydra core code, we STRONGLY recommend that you *write tests for every local change you make*. This will allow you to ensure that upgrading the core code doesn’t break any local changes you have made.

### Initial Modifications to Your Own Hydra Application

[YOUR Hydra Application - Initial Modifications](https://github.com/projecthydra/hydra-head/wiki/YOUR-Hydra-Application—Initial-Modifications) explains how to make two basic changes to your hydra rails application and how to write the appropriate tests for them. You should review the Initial App Mods document before continuing this tutorial.

## Adding a new Content Type

[Content Type Example: Journal Article](https://github.com/projecthydra/hydra-head/wiki/Content-Type-Example:-Journal-Article) steps through adding the Journal Article content type to a Hydra Application, including all the parts needed in each of the MVC pieces.

## Hydra Modeling Conventions

See[Hydra Modeling Conventions](https://github.com/projecthydra/hydra-head/wiki/Models—Hydra-Conventions)

## For Further Information

See [Reference](https://github.com/projecthydra/hydra-head/wiki/Reference) for more links.
