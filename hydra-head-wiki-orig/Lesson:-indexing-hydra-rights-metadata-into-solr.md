This Tutorial is known to work with hydra-head version 6.0.0..  
_Please update this wiki to reflect any other versions that have been tested._

# Steps

Given a book whose permissions look like this:
```text
puts book.permissions
{:type=>"group", :access=>"discover", :name=>"group2"}
{:type=>"group", :access=>"read", :name=>"group3"}
{:type=>"group", :access=>"read", :name=>"group9"}
{:type=>"group", :access=>"read", :name=>"group8"}
{:type=>"user", :access=>"read", :name=>"user3"}
{:type=>"user", :access=>"edit", :name=>"bob1"}
{:type=>"user", :access=>"edit", :name=>"sally2"}
```

By default, the permissions will be written into the a solr document like so

```
pp book.rightsMetadata.to_solr
{"discover_access_group_ssim"=>["group2"],
 "read_access_group_ssim"=>["group3", "group9", "group8"],
 "edit_access_person_ssim"=>["bob1", "sally2"],
 "read_access_person_ssim"=>["user3"]}
```

**Note:** The suffix `_ssim` on the solr fields is using the Hydra solr schema designed by Naomi Dushay.  It means that the values should be treated at **s**trings, should be both **s**tored and **i**ndexed, and are allowed to be **m**ultivalued.

As you can see, the arrays of group ids and user ids returned by methods like `discover_access_group`, `discover_access_person`, `edit_access_person`, etc. are all being indexed into solr.  This makes it possible for us to filter search results based on a user's id and group memberships.  That filtering of search results based on user-specific access permissions is often called _gated discovery_.  

# Next Step
Go on to [[Lesson: Gated Discovery - Filter search results based on permissions]] or return to the [[Access Controls with Hydra]] tutorial.