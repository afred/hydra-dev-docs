This Tutorial is known to work with hydra-head version 6.0.0.  
_Please update this wiki to reflect any other versions that have been tested._

# Goals

# Explanation

# Steps


### Step: Set up Rails Scaffolding for creating and editing Books

We will use the Rails scaffold generator to set up the routes, Controller and Views we need in order to CRUD books.  

**Note:** If you are not familiar with the ideas of Controllers, Views and routes, or aren't familiar with the Rails scaffold generator, go through the [Railsbridge Curriculum](http://curriculum.railsbridge.org/curriculum/curriculum) and then come back to this lesson.  You might also want to read the [Getting Started with Rails](http://guides.rubyonrails.org/getting_started.html) guide.

Tell the generator to build scaffolding for the Book model and make it assume books have `title` and `author` attributes like the ones we set up in the [[Dive into Hydra]] tutorial.
```text
rails generate scaffold Book title:string author:string
```
When it asks you whether to overwrite `app/models/book.rb`, enter `n` and hit enter.

The generator creates a few things that we don't want in the `test` and `db` directories.  Delete them with the `git clean` command.
```text
$> git clean -df test db
Removing db/migrate/20130417230046_create_books.rb
Removing test/fixtures/books.yml
Removing test/functional/books_controller_test.rb
Removing test/unit/book_test.rb
Removing test/unit/helpers/
```

### Step: Commit your work

```text
git add .
git commit -m"Ran scaffold generator"
```

### Step: Run the server & Explore

Run the `rails server` and visit [[http://localhost:3000/books]]

Explore the pages for creating, editing and showing Books.  You will notice that the Title and Author are being displayed strangely.

```text
Title: ["Anna Karenina"]
Author: ["Tolstoy, Leo"]
```

This is because the scaffold generator assumed that tile and author were single-valued while Hydra defaults to making them multi-valued.  There are two ways to fix this.  The first way is to tell Hydra to make the field single-valued.  The other way is to make your views handle multivalued fields.  Let's try both.

### Step: Make Title single-valued
In `app/models/book.rb` find the line that delegates the `title` field to the `descMetadata` datastream
```ruby
   delegate :title, to: 'descMetadata'
```

Add an extra argument of `unique: true` at the end of the line.
```ruby
   delegate :title, to: 'descMetadata', unique: true
```

Save the file and reload the pages in your browser.  The Title will now show up properly, as a single-valued field.

### Step: Make the Show page display Authors as a multi-valued field

In `app/views/books/show.html.erb` find the lines that display the author field.
```erb
<div class="field">
    <%= f.label :author %><br />
    <%= f.text_field :author %>
</div>
```

We want to make these lines iterate over the values returned by `@book.author` and put them in a list
```erb
<p>
  <b>Author(s):</b>
  <ul>
    <%- @book.author.each do |author|%>
      <li><%= author %></li>
    <%- end %>
  </ul>
</p>
```

Save the file and refresh the Show view for a book.  Now authors show up as a list of values.

### Step: Make the _form view partial display Authors as a multi-valued field

The `_form` partial defines the guts of the form that's used in both the `new` view and the `edit` view.  That makes our lives simpler because we only have to update that one file to fix both pages!

In `app/views/books/_form.html.erb` find the lines that display the author field.

```erb
  <p>
    <b>Author:</b>
    <%= @book.author %>
  </p>
```

Replace those lines with something that iterates over the values from `@book.author` and displays a text_field tag for each of them.  Note that the @name attribute will be set to "book[author][]".  The trailing `[]` in the name tells Rails that this is a multivalued field that should be parsed as an Array.
```erb
  <%= f.label :author, "Authors" %>
  <% @book.author.each do |author| %>
    <div class="field">
      <%= text_field_tag "book[author][]", author %>
    </div>
  <% end %> 
```

This handles displaying existing author values, but what about setting the author value in the first place?  If there are no values in the array, no fields are going to be displayed.  As a stop-gap, we can add a conditional clause that displays an empty text_field when the array is empty.

```erb
  <%= f.label :author, "Authors" %>
  <% @book.author.each do |author| %>
    <div class="field">
      <%= text_field_tag "book[author][]", author %>
    </div>
  <% end %> 
  <%- if @book.author.empty? %>
    <div class="field">
      <%= text_field_tag "book[author][]", nil %>
    </div>
  <%- end %>
```

*Note:* This still doesn't cover the case where you want to _add_ more than one Author to a Book.  That goes beyond the scope of this tutorial because it requires javascript (or a multi-page workflow).

### Step: Commit your Work

```text
git add .
git commit -m"Handling multivalued fields"
```

# Next Step
Return to the [[Dive into Hydra]] tutorial.