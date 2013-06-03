This Tutorial is known to work with hydra-head version 6.0.0.  
_Please update this wiki to reflect any other versions that have been tested._

# Goals
* Enable Gated Discovery in Blacklight Searches
* Read and understand the search logic that is injected into the solr queries
* Grant permission for users and groups to have `discover` access to an object

# Explanation
In [[Lesson: Indexing Hydra Rights Metadata into Solr]] we saw that Hydra indexes your access permissions assertions into the solr document for each object.  This allows us to filter search results based on that information.  The way Hydra does this is by injecting filter queries into the solr query specifying that it should only return documents that grant either `discover`, `read`, or `edit` permission to the currently logged in user or to any of the groups that the current user is a member of.  

The following steps will show you how this works.

# Steps

### Step 1: Make sure you're not logged into the Hydra Tutorial application

Make sure you're running a copy of the application that you built in the [[Dive into Hydra]] tutorial.  Remember that you need to have jetty running with `rake jetty:start` and the command to start the server is `rails server`.  

First we will look at how the application behaves when the user is not logged in, so if you already have the application running, make sure you've logged out.  

### Step 2: Confirm that you see Books in your search results

In your browser, visit [[http://localhost:3000]] and make sure you see at least one Book in your search results.  If you don't see any Books in your search results, go back to the [[Dive into Hydra]] tutorial and follow the Lessons to create a few books and make them appear in search results.

### Step 3: Look at the Solr query reported in the Rails server output

After running a search and seeing books in the results, inspect the the server's log output.  By default, when you run `rails s` it outputs the logs directly to the console window where you started the server.  When it first launched, you should have seen something like this

```text
=> Booting WEBrick
=> Rails 3.2.13 application starting in development on http://0.0.0.0:3000
=> Call with -d to detach
=> Ctrl-C to shutdown server
[2013-04-17 15:44:06] INFO  WEBrick 1.3.1
[2013-04-17 15:44:06] INFO  ruby 1.9.3 (2012-04-20) [x86_64-darwin10.8.0]
[2013-04-17 15:44:06] INFO  WEBrick::HTTPServer#start: pid=2929 port=3000
```

Now that you've visited the run a search in your browser the last section of text in that window should look like this

```text
Started GET "/?utf8=%E2%9C%93&search_field=all_fields&q=" for 127.0.0.1 at 2013-04-17 15:45:00 -0500
Processing by CatalogController#index as HTML
  Parameters: {"utf8"=>"✓", "search_field"=>"all_fields", "q"=>""}
Solr query: {:qf=>"title_tesim author_tesim", :qt=>"search", :rows=>10, :"facet.field"=>["object_type_sim", "pub_date_sim", "subject_topic_sim", "language_sim", "lc1_letter_sim", "subject_geo_sim", "subject_era_sim"], :q=>"", "spellcheck.q"=>"", :"f.subject_topic_sim.facet.limit"=>21, :sort=>"score desc, pub_date_dtsi desc, title_tesi asc", :fq=>["-has_model_ssim:\\"info:fedora/afmodel:FileAsset\\""]}
Solr fetch: CatalogController#query_solr (394.7ms)
   (0.1ms)  begin transaction
  SQL (63.7ms)  INSERT INTO "searches" ("created_at", "query_params", "updated_at", "user_id", "user_type") VALUES (?, ?, ?, ?, ?)  [["created_at", Wed, 17 Apr 2013 20:45:01 UTC +00:00], ["query_params", "---\n:utf8: ✓\n:search_field: all_fields\n:controller: catalog\n:action: index\n"], ["updated_at", Wed, 17 Apr 2013 20:45:01 UTC +00:00], ["user_id", nil], ["user_type", nil]]
   (2.5ms)  commit transaction
  Rendered /Users/matt/.rvm/gems/ruby-1.9.3-p194/gems/blacklight-4.1.0/app/views/catalog/_facets.html.erb (0.8ms)
  Rendered /Users/matt/.rvm/gems/ruby-1.9.3-p194/gems/blacklight-4.1.0/app/views/catalog/_opensearch_response_metadata.html.erb (1.2ms)

... many more lines starting with 'Rendered' ...

  Rendered /Users/matt/.rvm/gems/ruby-1.9.3-p194/gems/blacklight-4.1.0/app/views/shared/_footer.html.erb (0.4ms)
Completed 200 OK in 1456ms (Views: 795.8ms | ActiveRecord: 115.1ms)
```

This is the log/debug information that the CatalogController spit out when you ran that search.  We know this because the first lines tell us that it's information from a `GET` request to `"/?utf8=%E2%9C%93&search_field=all_fields&q="` which was processed by `CatalogController#index`

```
Started GET "/?utf8=%E2%9C%93&search_field=all_fields&q=" for 127.0.0.1 at 2013-04-17 15:45:00 -0500
Processing by CatalogController#index as HTML
  Parameters: {"utf8"=>"✓", "search_field"=>"all_fields", "q"=>""}
```

Following those opening lines of information, Blacklight tells us which Solr query it used

```
Solr query: {:qf=>"title_tesim author_tesim", :qt=>"search", :rows=>10, :"facet.field"=>["object_type_sim", "pub_date_sim", "subject_topic_sim", "language_sim", "lc1_letter_sim", "subject_geo_sim", "subject_era_sim"], :q=>"", "spellcheck.q"=>"", :"f.subject_topic_sim.facet.limit"=>21, :sort=>"score desc, pub_date_dtsi desc, title_tesi asc", :fq=>["-has_model_ssim:\\"info:fedora/afmodel:FileAsset\\""]}
```

For this lesson, the important part of that query is the `:fq` parameter

```
:fq=>["-has_model_ssim:\\"info:fedora/afmodel:FileAsset\\""]
```

In the Dive into Hydra tutorial, we turned off access controls in [[Lesson: Turn Off Access Controls]], so the :fq simply says to filter out anything with the model FileAsset.  We'll get back to that, but for now let's see how that `:fq` parameter changes when we re-enable gated discovery in our CatalogController.

### Step: Enable Gated Discovery in CatalogController

Now we will uncomment one of the lines that we commented out in [[Lesson: Turn Off Access Controls]].

Open `app/controllers/catalog_controller.rb` and find these lines

```ruby
# This applies appropriate access controls to all solr queries
#CatalogController.solr_search_params_logic += [:add_access_controls_to_solr_params]
```
Uncomment the second of those lines so it looks like this
```ruby
# This applies appropriate access controls to all solr queries
CatalogController.solr_search_params_logic += [:add_access_controls_to_solr_params]
```
This is telling CatalogController to run the `add_access_controls_to_solr_params` method when it's running all of its other `solr_search_params_logic` steps.  

Now that the line is uncommented, save the file.  You do not need to restart the server in order for this change to take effect.

**Aside:** Notice that the next two lines are also modifying the `solr_search_params_logic`.  This `exclude_unwanted_models` method is the source of that `:fq` filtering out `info:fedora/afmodel:FileAsset` objects.

```ruby
# This filters out objects that you want to exclude from search results, like FileAssets
  CatalogController.solr_search_params_logic += [:exclude_unwanted_models]
```

### Step 4: Re-run your search
Now re-run the same search.  The book that you saw in your search results moments ago has disappeared!  This is because you don't have permission to access it.  Now that we've enabled gated discovery, you will only be able to see things that you have been granted access to.

### Step 5: Look at how the Solr query is different with Gated Discovery Enabled

Go back to your server logs and scroll up to the last entry that begins with the lines 
```
Started GET "/?utf8=%E2%9C%93&search_field=all_fields&q=" for 127.0.0.1 at 2013-04-17 16:18:01 -0500
Processing by CatalogController#index as HTML
```
The full entry should include a much larger set of information about the solr query.  
```
Started GET "/?utf8=%E2%9C%93&search_field=all_fields&q=" for 127.0.0.1 at 2013-04-17 16:18:01 -0500
Processing by CatalogController#index as HTML
  Parameters: {"utf8"=>"✓", "search_field"=>"all_fields", "q"=>""}
  User Load (23.4ms)  SELECT "users".* FROM "users" WHERE "users"."email" = '' LIMIT 1
Usergroups are ["public"]
Solr parameters: {:qf=>"title_tesim author_tesim", :qt=>"search", :rows=>10, :"facet.field"=>["object_type_sim", "pub_date_sim", "subject_topic_sim", "language_sim", "lc1_letter_sim", "subject_geo_sim", "subject_era_sim"], :q=>"", "spellcheck.q"=>"", :"f.subject_topic_sim.facet.limit"=>21, :sort=>"score desc, pub_date_dtsi desc, title_tesi asc", :fq=>["edit_access_group_ssim:public OR discover_access_group_ssim:public OR read_access_group_ssim:public OR edit_access_group_ssim:public OR discover_access_group_ssim:public OR read_access_group_ssim:public"]}
Solr query: {:qf=>"title_tesim author_tesim", :qt=>"search", :rows=>10, :"facet.field"=>["object_type_sim", "pub_date_sim", "subject_topic_sim", "language_sim", "lc1_letter_sim", "subject_geo_sim", "subject_era_sim"], :q=>"", "spellcheck.q"=>"", :"f.subject_topic_sim.facet.limit"=>21, :sort=>"score desc, pub_date_dtsi desc, title_tesi asc", :fq=>["edit_access_group_ssim:public OR discover_access_group_ssim:public OR read_access_group_ssim:public OR edit_access_group_ssim:public OR discover_access_group_ssim:public OR read_access_group_ssim:public", "-has_model_ssim:\"info:fedora/afmodel:FileAsset\""]}
```
Now the `:fq` parameter in the solr query has more filters in it.

```
:fq=>["edit_access_group_ssim:public OR discover_access_group_ssim:public OR read_access_group_ssim:public OR edit_access_group_ssim:public OR discover_access_group_ssim:public OR read_access_group_ssim:public", "-has_model_ssim:\\"info:fedora/afmodel:FileAsset\\""]
```

Because you're not logged in (Right?  You logged out in Step 1?), the query is being filtered down to _only_ things that the `public` group has `edit` OR `discover` OR `read` access to.  Everything else will be filtered out.  Remember that the `public` group is a stand-in meaning _everyone and anyone, including people who are not logged in_

### Step 6: Log in, rerun search, look at Solr query again

Now click [login](http://localhost:3000/users/sign_in) and sign into the application.  If you have not created a user yet, click [Sign up](http://localhost:3000/users/sign_up) and create an account with your email address.  

For all of the following steps and lessons, we will treat `contact@curationexperts.com` as the stand-in for your email address.

Once you've signed in, rerun your search and look at the server logs again.

Now the Solr query from the last request has more information in the `:fq` field
```
Solr query: {:qf=>"title_tesim author_tesim", :qt=>"search", :rows=>10, :"facet.field"=>["object_type_sim", "pub_date_sim", "subject_topic_sim", "language_sim", "lc1_letter_sim", "subject_geo_sim", "subject_era_sim"], :q=>"", "spellcheck.q"=>"", :"f.subject_topic_sim.facet.limit"=>21, :sort=>"score desc, pub_date_dtsi desc, title_tesi asc", :fq=>["edit_access_group_ssim:public OR discover_access_group_ssim:public OR read_access_group_ssim:public OR edit_access_group_ssim:public OR discover_access_group_ssim:public OR read_access_group_ssim:public OR edit_access_group_ssim:registered OR discover_access_group_ssim:registered OR read_access_group_ssim:registered OR edit_access_person_ssim:contact@curationexperts.com OR discover_access_person_ssim:contact@curationexperts.com OR read_access_person_ssim:contact@curationexperts.com", "-has_model_ssim:\\"info:fedora/afmodel:FileAsset
\""]}
```

Because you're logged in, it's adding filters to include anything that grants permission for an individual user (aka. `person`) with your user id to edit, discover or read it. (Remember devise uses email address as user id by default).  It has also added filters to include anything that grants access to the `registered` group. That `registered` group is the special group hydra users to refer to _anyone who is logged into the application_.

### Step 7: Grant yourself `discover` permissions on a Book
In the `rails console`, find a Book and give yourself discover permissions.  Remember to save the book after setting the permissions.

```
book = Book.all.last
 => #<Book pid:"changeme:1", title:["Anna Karenina"], author:["Tolstoy, Leo"]> 
book.discover_users = ["contact@curationexperts.com"]
 => ["contact@curationexperts.com"] 
book.save
 => true 
```


### Step 8: Rerun Search 

Rerun the search in the browser.  The book should appear in your search results again.

### Step 9: Log in as an archivist

Now we will see how a user's group memberships are incorporated into searches.  [Log out](http://localhost:3000/users/sign_out) of the application and [create a user](http://localhost:3000/users/sign_up) with email address `archivist1@example.com`.  

Once you've created the account, re-run your search and look at the log output for that request.

As we saw in a previous lesson,  `archivist1@example.com` is listed as a member of two groups - "archivist" and "admin_policy_object_editor".  In the Solr query, you should see that extra filter queries have been added for each of the groups that archivist1@example.com belongs to.  For each of the groups, the query now specifies that solr should include results for any documents that grant `discover`, `read`, or `edit` access to that group.

```
:fq=>["edit_access_group_ssim:public OR discover_access_group_ssim:public OR read_access_group_ssim:public OR edit_access_group_ssim:public OR discover_access_group_ssim:public OR read_access_group_ssim:public OR edit_access_group_ssim:archivist OR discover_access_group_ssim:archivist OR read_access_group_ssim:archivist OR edit_access_group_ssim:admin_policy_object_editor OR discover_access_group_ssim:admin_policy_object_editor OR read_access_group_ssim:admin_policy_object_editor OR edit_access_group_ssim:registered OR discover_access_group_ssim:registered OR read_access_group_ssim:registered OR edit_access_person_ssim:archivist1@example.com OR discover_access_person_ssim:archivist1@example.com OR read_access_person_ssim:archivist1@example.com", "-has_model_ssim:\\"info:fedora/afmodel:FileAsset\\""]
```
A few lines above the Solr query in the log output, there is also a line that tells you which groups the current user belongs to.

```
Usergroups are ["public", "archivist", "admin_policy_object_editor", "registered"]
```

When you weren't logged in at all, that line said `Usergroups are ["public"]`. When you logged in as yourself, it read `Usergroups are ["public", "registered"]` and now that you're logged in as a user with group membership, the list is even longer.

Notice that the book has disappeared from your search results again because neither archivist1@example.com nor any of its groups has permission to discover the book.

### Step 10: Allow the archivist group to see the Book

Back on the `rails console`, give the `archivist` group discover access to the book.

```
book.discover_groups = ["archivist"]
 => ["archivist"] 
book.save
 => true 
```

Rerun the search in your browser.  The book should be there.

# Next Step
Go on to [[Lesson: Use Hydra Access Controls to Control Access to Blacklight Show Pages]] or return to the [[Access Controls with Hydra]] tutorial.