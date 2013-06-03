This Tutorial is known to work with hydra-head version 6.0.0..  
_Please update this wiki to reflect any other versions that have been tested._

# Goals


# Explanation

# Steps

All these steps are on the `rails console`.  

```text
book = Book.new
```

### Step 1: List the current permissions

```text
book.permissions
 => []
```

### Step 2: Grant a variety of permissions all at once

Permissions are basically tracked as an array of Hashes that each specifies 
* `:name` - the user id or group id
* `:access` - the access type (usually `discover`, `read` or `edit`)
* `:type` - either `group` or `person`

One way to set these is to manually modify the whole array all at once.

```text
book.permissions = [{:name=>"bob1", :access=>"edit", :type=>'person'}, 
  {:name=>"sally2", :access=>"edit", :type=>'person'}, {:name=>"group1", :access=>"read", :type=>'group'}, 
  {:name=>"group2", :access=>"discover", :type=>'group'}]
```

### Step 3: Use the API methods to inspect the permissions

Getting the whole array of permissions is a bit unwieldy.  Modifying it is unwieldy too.

```text
book.permissions
=> [{:name=>"bob1", :access=>"edit", :type=>"person"}, {:name=>"sally2", :access=>"edit", :type=>"person"}, {:name=>"group1", :access=>"read", :type=>"group"}, {:name=>"group2", :access=>"discover", :type=>"group"}] 
```

To get around this, Hydra provides some methods that let you get directly at the juicy bits.

```text
book.edit_users 
 => ["bob1", "sally2"] 
book.read_users 
 => [] 
book.discover_users 
 => [] 
book.edit_groups 
 => [] 
book.read_groups 
 => ["group1"] 
book.discover_groups 
 => ["group2"] 
```

### Step 4: Use the API methods to modify permissions

```
book.read_groups 
 => ["group1"] 
book.read_groups = ["group3", "group4", "group5"]
 => ["group3", "group4", "group5"]
book.read_users = ["user3"]
 => ["user3"]
```

Notice that this _removes_ any group that was not in the array you provided.  In other words, it is _replacing_ the previous list of groups with a new definitive list.  

### Step 5: Use the API set_ methods to modify permissions

If you only want your changes to impact certain groups, use `set_read_groups`, which takes a second argument that is a list of which groups are eligible to be updated.

#### Examples of set_read_groups

In this example, no groups are "eligible", so the new group will be added but none of the others will be removed.  This would be how you add a group to the list without removing anyone else.
```
book.read_groups 
 => ["group3", "group4", "group5"]
book.set_read_groups( ["group8"], [])
 => {}
book.read_groups
 => ["group3", "group4", "group5", "group8"]
```

In this example, groups 4, 5 and 8 are all eligible while group 3 is not, so group3 is left alone while all the others get wiped out because they're not in the list of groups being granted read access.
```
book.set_read_groups( ["group9"], ["group8", "group4", "group5"])
 => {} 
book.read_groups
 => ["group3", "group9"]
```

### Step 6: Save the Object

Now save the book to Fedora.  In the next lesson we will see how the access info is persisted in the rightsMetadata XML datastream.

```text
book.save
```

# Next Step
Go on to [[Lesson: Reading Hydra rightsMetadata XML]] or return to the [[Access Controls with Hydra]] tutorial.