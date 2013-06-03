This lesson is known to work with hydra-head version 6.0.0.   
_Please update this wiki to reflect any other versions that have been tested._

# Goals
* Add the Hydra software to your application's list of dependencies
* Use bundler to install dependencies

# Explanation 

In order to take advantage of the Hydra code and features in your application, you need to tell the application where to find that code and which versions of that code to use.  Rails uses a tool called bundler to track dependencies.  Bundler looks in the file called Gemfile to know what you want installed.

# Steps

Open up ```Gemfile``` in your editor.   We're going to add the following lines:

```ruby
gem 'blacklight'
gem 'hydra-head', '6.0.0'
gem 'jettywrapper'
```

This declares our application to have a dependency on the 6.0.0 release version of hydra and it tells your application to explicitly require blacklight (which otherwise is automatically installed by hydra)

Now we save the file and install the dependencies by running ```bundle install```

# Next Step
Go on to [[Lesson: Run the Blacklight generator]] or return to the [[Dive into Hydra]] page.