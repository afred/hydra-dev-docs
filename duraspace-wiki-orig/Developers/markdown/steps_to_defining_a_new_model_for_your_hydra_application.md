To Define the Model

1.  Choose what Metadata Schemas you will be using
2.  Decide what your objects will look like (datastream names, etc.)
3.  Create Fixture XML with sample values in all of the relevant places
4.  Write RSpec tests for the Datastreams that will handle your XML
5.  Define Datastreams & OM Terminologies to satisfy your tests
6.  Create Fixture Object(s) that have all of the necessary datastreams populated with sample xml
7.  Write RSpec tests for your model. Use the fixture object(s) as necessary.
8.  Define the model to satisfy your tests
9.  Document your model [Here](https://github.com/projecthydra/active_fedora/wiki/Models-in-the-Wild)

If you will be displaying these objects in a Hydra Head

1.  Create the "Add" button for this Model
2.  Write Cucumber tests that describe how your fixture objects should be displayed in show, edit, new, and index views
3.  Create show, edit, new, and index view templates to satisfy the cucumber tests

