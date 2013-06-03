This Tutorial is known to work with hydra-head version 6.0.0.  
_Please update this wiki to reflect any other versions that have been tested._

# Goals
* Use Hydra Access Controls and its built-in CanCan integration to decide whether to render a page

# Explanation

All of the methods described in this lesson are actually provided by a gem called CanCan.  Hydra simply implements the ability for those CanCan methods to consult Hydra rightsMetadata to make decisions about access controls.  The actual methods come from CanCan, so for more information you should consult the CanCan documentation.

# Steps

### Step: Make sure you have generated the Rails Scaffolding for Books

Before starting this lesson, you need to have Controller, Views and Routes set up for Books.  The easiest way to do this is by using the rails scaffold generator.  See [[Lesson: Generate Rails Scaffolding for Creating and Editing Books]] in the [[Dive into Hydra]] tutorial for step-by-step instructions.

### Step:

In `app/controllers/books_controller.rb` find the lines that define the `show` method and add `authorize! :show, params[:id]` as the first thing the method does.

```ruby
  # GET /books/1
  # GET /books/1.json
  def show
    authorize! :show, params[:id]
    @book = Book.find(params[:id])

    respond_to do |format|
      format.html # show.html.erb
      format.json { render json: @book }
    end
  end
```

Now when you try to view a Book's `show` page in the browser, you will get an error page.

```text
CanCan::AccessDenied in BooksController#show

You are not authorized to access this page.
```

If you go into the `rails console` and give yourself read access, then the page renders normally.

Likewise, you can add `authorize! :edit, params[:id]` to the `BooksController#edit`, `BooksController#update` and `BooksController#destroy` methods.  Then save the file and see what happens in the browser. When you try to access the `edit` page for an object that you don't have edit access to, you get an error, but if you go into the `rails console` and give yourself edit access, then the page renders normally.


### Step: Alternatively use load_and_authorize_resource

Instead of calling `authorize!` in each of your methods, there's a one-line option you can use.  At the top of app/controllers/books_controller.rb add a line that simply says `load_and_authorize_resource` 
```ruby
class BooksController < ApplicationController

  load_and_authorize_resource

  # GET /books
  # GET /books.json
  def index
    @books = Book.all

    respond_to do |format|
      format.html # index.html.erb
      format.json { render json: @books }
    end
  end

  [...]

end
```

This `load_and_authorize_resource` method does two things for you.  It loads a copy of the Book from Fedora and it runs `authorize!` for you.  So, if you're using this method you can also delete the lines that create `@book` for you, for example:

```ruby
@book = Book.find(params[:id])
```

If you're using `load_and_authorize_resource`, it does that loading for you.  This allows you to have less lines of code in your Controller.

**Note:** A drawback of using `load_and_authorize_resource` is that it always loads a copy of the object from Fedora on every request.  By contrast, calling `authorize!` only reads the permissions metadata out of Solr, which is much faster than loading an object from Fedora.  Either method works equally well.  There is just the tradeoff: one method lets your application run faster while the other lets you have less lines of code in your Controller.

# Next Step
Go on to [[Lesson: Using Hydra Access Controls and CanCan to conditionally render part of a page]] or return to the [[Access Controls with Hydra]] tutorial.