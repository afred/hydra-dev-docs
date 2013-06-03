This Tutorial is known to work with hydra-head version 6.0.0..  
_Please update this wiki to reflect any other versions that have been tested._

# Goals

* Use Hydra's RoleMapper to list a User's roles (aka. group memberships)
* Read the YAML files that the default RoleMapper uses to look up roles associated with a User ID

# Steps

### Step: List the roles associated with your email address

In the console, call `RoleMapper.roles` and pass in your email address as the argument.  For example:
```ruby
RoleMapper.roles("my-email-address@gmail.com")
[]
```
The method returned an empty Array, meaning that your email address isn't associated with any roles.

### Step: List the roles associated with archivist1@example.com

Now pass "archivist1@example.com" into `RoleMapper.roles`
```ruby
RoleMapper.roles("archivist1@example.com")
["archivist", "admin_policy_object_editor", "registered"]
```

Why does RoleMapper return three roles for that email address?  Because it's listed in the config files that the default RoleMapper loads role information from.

### Step: Look at the `role_map` YAML files

The Hydra RoleMapper is designed to be overridden.  It provides the one spot in your code that has to be overridden in order to make your Hydra Head retrieve user role info from the source of your choice (ie. an LDAP server, Shibboleth, etc.).  For cases where you have not (yet) overridden it, the default RoleMapper simply looks in YAML files in your application's `config` directory to figure out which users belong to which groups.

Open up `config/role_map_development.yml`. The contents should look like this
```text
uva-only:
  - uva-only
archivist:
  - archivist1@example.com
donor:
  - donor1@example.com
researcher:
  - researcher1@example.com
patron:
  - patron1@example.com
admin_policy_object_editor:
  - archivist1@example.com
```

There are also role_map YAML files for each of the other Rails environments (production, test, etc.), but since the rails console runs in the development environment by default role_map_development.yml is the one we want to look at.

As you can see, archivist1@example.com is listed under two different groups/roles:  "archivist" and "admin_policy_object_editor".  That's why those two roles are returned when you call `RoleMapper.roles("archivist1@example.com")`.  The third role, "registered" is automatically included because it refers to all users who are logged into the application.

You rarely need to call `RoleMapper.roles` directly.  Instead, you will rely on methods in Hydra API that are using that method to figure out which groups a user is a member of.  In the next few lessons, you will see examples of this group-membership-aware functionality.

# Next Step
Go on to [[Lesson: Gated Discovery - Filter search results based on permissions]] or return to the [[Access Controls with Hydra]] tutorial.
