* Using new solr schema
  The new solr schema is written to be a more efficient (smaller document size) and easier to configure
  in your OmDatastream or RDFDatastreams. 

  You need to put in place the new schema.xml and solrconfig.xml.  Then restart solr and reindex using:
  ```ActiveFedora::Base.reindex_everything ```

  You're going to want to search your project to make sure you don't have any fields coded like: *_t or *_s 
  These field names have changed to *_tesim and *_ssim respectively.  

  If you want to lookup the name of a field, you can pass a type to the solr_name method (by default it assumes text):

  >> ActiveFedora::SolrService.solr_name('foo', :stored_sortable, :type=>:date)
  => "foo_dtsi"

  Finally change your config/initializer/hydra_config.rb to look something like this:
  https://github.com/projecthydra/hydra-head/blob/master/hydra-core/lib/generators/hydra/templates/config/initializers/hydra_config.rb


  For more information about the new index see the Solrizer 3.0.0 docs:
  https://github.com/projecthydra/hydra-head/wiki/Solr-Schema
  https://github.com/projecthydra/solrizer/blob/master/README.textile


* Added a Hydra::Controllers::DownloadBehavior which allows you to easily create 
  a controller to serve the content of datastreams.  

```
  class DownloadsController < ApplicationController
    include Hydra::Controller::DownloadBehavior
  end
```
  then in config/routes.rb add ```resources :downloads```

  then you can do a GET /downloads/{pid}?datastream_id=thumbnail  to get the thumbnail datastream
  
  By default this allows any user with read access to the object identified by pid to download any
  of the datatreams.  If you want to control which datastreams a user has access to, overwrite the
  can_download? method.
  
  This example allows anyone to download a 'thumbnail' datastream:
```
  def can_download?
    datastream.dsid == 'thumbnail'
  end
```
* The default control group changed from Inline (X) to Managed (M).  If you want to retain the current configuration add: ```:control_group=>'X'``` to any ```has_metadata``` declarations.
