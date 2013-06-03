## Types of Access: Discover, Read, Edit 

The three default types of access handled by rightsMetadata are Discover, Read, and Edit.  

* **Discover Access** allows a user to see the object in search results, but does not allow the user to see the detail view or download the object's content
* **Read Access** allows a user to Discover the object, see the detail view and download the object's content
* **Edit Access** allows a user to Discover and Read the object as well as edit its metadata and modify its file content

You can add your own access types if you find them necessary.  Also, the actions allowed by each type of access is a convention and a guideline.  Within your specific application you can override/extend/constrict which actions to allows or prevent based on each access type.

## Who can be granted Access: Users & Groups
In the rightsMetadata, you can grant these access permissions to either individual users or groups of users.  User permissions and Group permissions are tracked separately, meaning that granting edit access to a user whose ID is rlg2 will only grant access to that user.  It will not accidentally also grant access to members of a group whose group ID is rlg2.

## Two Special Types of Groups: registered and public
It is up to you to decide how the Hydra Head will look up group memberships for users.  Some adopters pull this info from LDAP.  Others pull it from Shibboleth.  Others track it all in a SQL database that's managed directly by their Hydra Head.  The two exceptions to this are the `registered` and `public` groups, which Hydra uses to refer to the general cases of 

* `public` refers to everyone, regardless of whether they are even logged into your application  
* `registered` refers to anyone who is logged into the application -- if a user can log in, then "registered" is automatically added to their list of group memberships  

To grant discover/read/edit access to these groups, simply add them to the list of groups with that type of permission.  For example, to allow anyone in the world to read and download a book object, you add "public" to the `read_access_groups`.  

## Where the information is Stored

This information about which groups & users should be granted which types of access is all written to XML datastreams that use the Hydra rightsMetadata schema.  We will look at an example of this XMl in [[Lesson: Reading Hydra rightsMetadata XML]]

# Next Step
Go on to [[Lesson: Set Permissions on an Object]] or return to the [[Access Controls with Hydra]] tutorial.