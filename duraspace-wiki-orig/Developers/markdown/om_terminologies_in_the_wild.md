Hydra has released a Hydra Head tutorial. As you step through the tutorial it actually builds a hydra head on your local machine.

In more detail, the tutorial:

-   installs basic application prerequisites
-   generates a new rails application
-   walks through building a Hydra model:
    -   first, as a AF::Base object with an XML datastream
    -   then, with a simple OM terminology for a basic, contrived schema
    -   finally, with a simple MODS-based terminology

-   wires the model into Rails using standard Rails scaffolding
-   adds blacklight and hydra-head gems for object discovery
-   adds a basic rspec/capybara test

Throughout the process, there are a number of prompts to poke around the rails console and/or in a browser. At the end of the tutorial, you have a working Hydra Head for adding MODS-based metadata records.

The tutorial can be found in the projecthydra git repository: [https://github.com/projecthydra/hydra-tutorial](https://github.com/projecthydra/hydra-tutorial)

and it is a simple as

\$ gem install hydra-tutorial

\$ hydra-tutorial

…to get going! Many thanks to Chris Beer for this wonderful addition to Hydra’s armoury.
