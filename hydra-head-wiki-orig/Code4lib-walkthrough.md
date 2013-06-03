This tutorial is know to work with hydra-head version 6.0.0.rc4.   
Please update this wiki to reflect any other versions that have been tested.

# System Requirements
Your system should have the following installed before beginning the walkthrough
+ Ruby 1.9.3
+ Rails  ~>3.2.9
+ Java runtime >= 6.0 

# Generate a rails application
Once you have installed the rails gem, begin by generating a new rails application
```
rails new hydra-demo
```

This generates the file structure for an empty rails application. And it runs 'bundler' which loads in all of the external dependencies for rails.

Enter the directory for the new rails app: ```cd hydra-demo```, then you should see a file structure like this:

```
$> ls
Gemfile		Rakefile	config.ru	lib		script		vendor
Gemfile.lock	app		db		log		test
README.rdoc	config		doc		public		tmp
```

# Create a git repository
Now, let's turn this directory into a git repository.  Type the following:
```
git init .
```

Then you should see something like this:

```
Initialized empty Git repository in /Users/justin/hydra-demo/.git/
```

Next, we'll add all the files rails created into the repository.  This way we can jump back to this state later if the need arises.

```
git add .
git commit -m "Initial rails application"
```


# Install hydra-jetty

In order to use blacklight and hydra-head, you need an installation of Solr.  Additionally, hydra-head requires a copy of the Fedora repository.  We have created a package called hydra-jetty, which provides both of these within a Jetty Java application server.  You can install this package by running:

```
git submodule add git://github.com/projecthydra/hydra-jetty.git jetty
```

This can be very slow (over 100Mb of download). So if you've got the hydra-jetty distribution already on a disk, you can instead type something like the following.  You'll have to put in the absolute path to hydra-jetty on your own machine.

```
git submodule add /Users/justin/hydra-jetty jetty
```

This will take a few moments.  When it's done you'll see the directory named ```jetty``` has been created.  We will ```cd jetty``` and then type ```git checkout new-solr-schema``` if it worked you should see:

```text
Branch new-solr-schema set up to track remote branch new-solr-schema from origin.
Switched to a new branch 'new-solr-schema'
```

Then we'll ```cd ..``` back into the project directory.

We should tell git to ignore any files added into the jetty directory. We do that by editing ```.gitmodules``` and adding the line ```  ignore = dirty```.  It should look like this:

```text
[submodule "jetty"]
    path = jetty
    url = /Users/justin/workspace/hydra-jetty
    ignore = dirty
```

# Add the hydra dependencies

Open up ```Gemfile``` in your editor.   We're going to add the following lines:

```ruby
gem 'blacklight'
gem 'hydra-head', '6.0.0.rc4'
gem 'jettywrapper'
```

This declares our application to have a dependency on ```blacklight``` and on the 6.0.0.pre7 version of hydra-head.

Now we save the file and install the dependencies by running ```bundle install```

# Run the Blacklight generator

Hydra is built on top of Blacklight, so it's important that we first run the Blacklight generator.

We do this by typing ```rails generate blacklight --devise```.  The ```--devise``` flag tells the generator to install the 'devise' gem, which takes care of authentication.

The blacklight generator has created a few database migrations that store users, saved searches and bookmarks. Additionally, it's installed the devise gem and the bootstrap gem.  It's created an important file in our application ```app/controllers/catalog_controller.rb```.  This is the primary place where you configure the blacklight search.

At this point we should run the migrations, so that the database tables are built.  To do this enter the following command: ```rake db:migrate```

# Run the Hydra-Head generator

```
rails generate hydra:head -f
```

This overwrites the ```app/controller/catalog_controller.rb``` from blacklight, with one that is appropriate for the solr schema that we are using.  

At this point it's a good idea to commit the changes:
```
git add .
git commit -m "Ran blacklight and hydra-head generators"
```

# Start jetty

At the project root, type ```rake jetty:start``` You should see output like this:
```
Initializing jettywrapper
Starting jetty with these values: 
jetty_home: /Users/justin/hydra-demo/jetty
solr_home: /Users/justin/hydra-demo/jetty/solr
jetty_command: java -Djetty.port=8983 -Dsolr.solr.home=/Users/justin/hydra-demo/jetty/solr -Xmx256m -XX:MaxPermSize=128m -jar start.jar
Logging jettywrapper stdout to /Users/justin/hydra-demo/jetty/jettywrapper.log
Wrote pid file to /Users/justin/hydra-demo/tmp/pids/_Users_justin_hydra-demo_jetty.pid with value 8315
Waited 5 seconds for jetty to start, but it is not yet listening on port 8983. Continuing anyway.
jetty started at PID 8315
```

hydra-jetty has a fair amount of stuff in it, so it may take up to a minute to start.  You can check to see if it's started by going to [[http://localhost:8983/solr]]

# Examine the catalog
If you open ```app/controllers/catalog_controller.rb``` and look at lines 11-13 you should see this:
```ruby
  # These before_filters apply the hydra access controls
  before_filter :enforce_show_permissions, :only=>:show
  # This applies appropriate access controls to all solr queries
  CatalogController.solr_search_params_logic += [:add_access_controls_to_solr_params]
```

This code tells blacklight to enforce access controls on the search and result view pages.  For the time being we will turn this off by commenting out lines 12 and 14 so that it looks like this:

```ruby
  # These before_filters apply the hydra access controls
  #before_filter :enforce_show_permissions, :only=>:show
  # This applies appropriate access controls to all solr queries
  #CatalogController.solr_search_params_logic += [:add_access_controls_to_solr_params]
```
Then, save the file.

# Build a Book model 
In fedora an object can have many 'datastreams' which are either content for the object or metadata about the object. We are going to create a 'book' object.  This object will have a metadata datastream which will contain some XML that describes the properties of the book.  We'll call this datastream 'descMetadata'.  You are free to call it whatever you like, but 'descMetadata' is a loose convention that stands for 'descriptive metadata'.

First we'll create a Ruby class that represents this desriptive metadata.  Make a new directory for our datastreams by typing in ```mkdir app/models/datastream```.   Now we'll create a file called ```app/models/datastream/book_metadata.rb```

Paste the following code into that file:

```
class Datastream::BookMetadata < ActiveFedora::OmDatastream

  set_terminology do |t|
    t.root(path: "fields")
    t.title
    t.author
  end

  def self.xml_template
    Nokogiri::XML.parse("<fields/>")
  end
end
```

This class extends from OmDatastream. OM is a gem that allows us to describe the format of an xml file and access properties. We are using OM by calling the ```set_terminology``` method.  The xml_template method tells OM how to create a new xml document for this class. 

Let's take a look at how this class works.  We'll start the rails console by typing ```rails console```.  You should see ```Loading development environment (Rails 3.2.11)```.  Now you're in a REPL. Let's create a new BookMetadata instance. I've shown the expected output after each command:

```
d = Datastream::BookMetadata.new
 => #<Datastream::BookMetadata @pid="" @dsid="" @controlGroup="X" changed="false" @mimeType="text/xml" > 
d.title = "ZOIA! Memoirs of Zoia Horn, Battler for the People's Right to Know."
 => "ZOIA! Memoirs of Zoia Horn, Battler for the People's Right to Know." 
d.author = "Horn, Zoia"
 => "Horn, Zoia" 
d.to_xml
 => "<fields>\n  <title>ZOIA! Memoirs of Zoia Horn, Battler for the People's Right to Know.</title>\n  <author>Horn, Zoia</author>\n</fields>"
```

Now let's, create a model that uses this datastream.  Create a new file at ```app/models/book.rb``` we'll paste in this code:

```ruby
class Book < ActiveFedora::Base
  has_metadata 'descMetadata', type: Datastream::BookMetadata

  delegate :title, to: 'descMetadata'
  delegate :author, to: 'descMetadata'

end
```

We've defined our Book model to use the Datastream::BookMetadata for it's datastream called 'descMetadata'.  We're telling the model to use the descMetadata as the delegate for the properties 'title' and 'author'

Now we'll open the ```rails console``` again and see how to work with our book.

```
b = Book.create(title: 'Anna Karenina', author: 'Tolstoy, Leo')
 => #<Book pid:"changeme:1", title:["Anna Karenina"], author:["Tolstoy, Leo"]> 
```

We've created a new Book object in the repository.  You can see that it has a pid of 'changeme:1', if we go to [[http://localhost:8983/fedora/objects/changeme:1]] we should see what it looks like in fedora.  Note especially that the xml datastream has been ingested [[http://localhost:8983/fedora/objects/changeme:1/datastreams/descMetadata/content]]

Let's also see that this book has been ingested into the Solr search index. [[http://localhost:8983/solr/select?q=changeme:1]].  However at this point, the title and author have not been stored in solr.  We should modify our BookMetadata datastream to provide those instructions.

Reopen ```app/models/datastream/book_metadata.rb``` and change the terminology section to look like this:

```ruby
  set_terminology do |t|
    t.root(path: "fields")
    t.title(index_as: :stored_searchable)
    t.author(index_as: :stored_searchable)
  end
```

Now, restart the rails console and we can load the object we previously created:

```
b = Book.find('changeme:1')
 => #<Book pid:"changeme:1", title:["Anna Karenina"], author:["Tolstoy, Leo"]> 
```

Now we'll call the ```update_index``` method, which republishes the Solr document using the changes we've made.

```
b.update_index
 => {"responseHeader"=>{"status"=>0, "QTime"=>25}} 
```

If you refresh the document result from solr you should see that these fields have been added to the solr_document:

```xml
<arr name="fields_title_tesim">
  <str>Anna Karenina</str>
</arr>
<arr name="fields_0_title_tesim">
  <str>Anna Karenina</str>
</arr>
<arr name="fields_author_tesim">
  <str>Tolstoy, Leo</str>
</arr>
<arr name="fields_0_author_tesim">
  <str>Tolstoy, Leo</str>
</arr>
<arr name="title_tesim">
  <str>Anna Karenina</str>
</arr>
<arr name="author_tesim">
  <str>Tolstoy, Leo</str>
</arr>
```

Now that we've got our model working, it's a great time to commit to git:

```
git add .
git commit -m "Created a book model and a datastream"
```

# Start application, search for results.

Now, let's see our model on the web.  You can start 'webrick', a development server that comes with rails by typing ```rails server```.  Then you can see the application running at [[http://localhost:3000/]]

If it worked you should see a page with the text: "Welcome aboard. You’re riding Ruby on Rails!".  This is the default page, but now that we know it works, we can delete it.  Open a new terminal (so we can keep the server running) and type ```rm public/index.html```
, then reload the page at [[http://localhost:3000/]].  You should see something that looks like a default blacklight install.  If you search for 'Anna' however, you don't see any results.  This is because we haven't told solr which fields we want to search in.  Let's fix that by setting the default 'qf' solr parameter.  Open ```app/controllers/catalog_controller.rb``` and set the ```default_solr_params``` section (around line 18) to this:

```
    config.default_solr_params = { 
      :qf => 'title_tesim author_tesim',
      :qt => 'search',
      :rows => 10 
    }
```

Save the file, and refresh your web browser. You should now see a result for "Anna Karenina" when you search for "Anna"


# Build page model

Next we're going to add a Page model.  We'll start by creating another simple metadata datastream.  This time open ```app/models/datastream/page_metadata.rb``` and add this content:
```ruby
class Datastream::PageMetadata < ActiveFedora::OmDatastream

  set_terminology do |t|
    t.root(path: "fields")
    t.number index_as: :stored_searchable, type: :integer
    t.text index_as: :stored_searchable

  end

  def self.xml_template
    Nokogiri::XML.parse("<fields/>")
  end
end

```

Then we'll build a Page model that uses the datastream. Open ```app/models/page.rb``` and paste this in:

```ruby
class Page < ActiveFedora::Base
  has_metadata 'descMetadata', type: Datastream::PageMetadata

  belongs_to :book, :property=> :is_part_of

  delegate :number, to: 'descMetadata'
  delegate :text, to: 'descMetadata'
end
```

This is very similar to how our Book class looks, with the exception of the line ```belongs_to :book```.  This establishes a relationship between the book and page model nearly the same as you would do with ActiveRecord.  In ActiveFedora, we have to add the :property attribute as well.  Relationships are stored as RDF within the RELS-EXT datastream.  The :property is actually an RDF predicate. Let's edit the Book class and add the other half of the relationship:

```ruby
  # within app/models/book.rb
  has_many :pages, :property=> :is_part_of
```

Save that file and then reopen the rails console. Now we ought to be able to create some associations.

```ruby
b = Book.find("changeme:1")
 => #<Book pid:"changeme:1", title:["Anna Karenina"], author:["Tolstoy, Leo"]> 
p = Page.new(number: 1, text: "Happy families are all alike; every unhappy family is unhappy in its own way.")
 => #<Page pid:"", number:[1], text:["Happy families are all alike; every unhappy family is unhappy in its own way."]> 
p.book = b
 => #<Book pid:"changeme:1", title:["Anna Karenina"], author:["Tolstoy, Leo"]> 
p.save
 => true 
b.pages
 => [#<Page pid:"changeme:3", number:[1], text:["Happy families are all alike; every unhappy family is unhappy in its own way."]>] 
```

# Adding Content Datastreams

So far, we've only added datastreams that bear metadata.  Let's add a datastream that has some content to it.  For example, this could be a content datastream in our book model that is an image of the book's cover, or a datastream added to the page model that is an image or pdf of that actual page.

In this case, we'll add a file datastream where we can store a pdf of a page.

In our page model, we'll add the following line underneath our descMetadata datastream:

```
has_file_datastream :name => "pageContent", :type=>ActiveFedora::Datastream
```

Now we have a datastream called "pageContent" that can hold any kind of file we wish to add to it.  To add the file to one of our page objects, open up the console again:

```ruby
 > p = Page.find("changeme:2")
 => #<Page pid:"changeme:2", number:[1], text:["Happy families are all alike; every unhappy family is unhappy in its own way."]> 
 > p.datastreams.keys
 => ["DC", "RELS-EXT", "descMetadata", "pageContent"] 
 > p.datastreams["pageContent"].content = File.new("/Users/adamw/Desktop/page1.pdf")
 => #<File:/Users/adamw/Desktop/page1.pdf> 
 > p.save
 => true
```

Now if you go to [[http://localhost:8983/fedora/objects/changeme:2/datastreams]], you'll see the pageContent datastream in Fedora.  Following the links to it will allow us to view or download the file we've added.