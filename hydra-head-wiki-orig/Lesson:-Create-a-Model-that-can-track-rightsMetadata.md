This Tutorial is known to work with hydra-head version 6.0.0..  
_Please update this wiki to reflect any other versions that have been tested._

# Goals


# Explanation

# Steps

### Step 1: Add the Datastream to your Model

Add these two lines to an ActiveFedora model
```ruby
include Hydra::ModelMixins::RightsMetadata
has_metadata :name => "rightsMetadata", :type => Hydra::Datastream::RightsMetadata
```

By convention, the include statements should be at the very beginning of your class definition, so if you're using the Book model from [[Dive into Hydra]] it might look like this with the two lines added
```ruby
class Book < ActiveFedora::Base
  # This is the include statement
  include Hydra::ModelMixins::RightsMetadata

  has_metadata 'descMetadata', type: Datastream::BookMetadata
  # This defines the rightsMetadata datastream
  has_metadata :name => "rightsMetadata", :type => Hydra::Datastream::RightsMetadata

  delegate :title, to: 'descMetadata'
  delegate :author, to: 'descMetadata'

end
```

Save the file.

### Step 2: Confirm that it worked

Restart the `rails console` and run

```text
book = Book.new
book.rightsMetadata.class
 => Hydra::Datastream::RightsMetadata 
book.permissions
 => [] 

```

As you can see, rightsMetadata is a Hydra::Datastream::RightsMetadata Datastream and the methods provided by Hydra::ModelMixins::RightsMetadata, such as `.permissions` have been mixed into the Book model.  Now you're ready to use these things to assert permissions on objects.

# Next Step
Go on to [[Lesson: Set permissions on an object]] or return to the [[Access Controls with Hydra]] tutorial.