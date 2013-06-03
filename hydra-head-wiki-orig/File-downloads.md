```
  class DownloadsController < ApplicationController
    include Hydra::Controller::DownloadBehavior
  end
```
  then in config/routes.rb add ```resources :downloads```

  then you can do a GET /downloads/{pid}?datastream_id=thumbnail  to get the thumbnail datastream
  
  By default this allows any user with read access to the object identified by pid to download any
  of the datatreams.  If you want to control which datastreams a user has access to, override the
  can_download? method.
  
  This example allows anyone to download a 'thumbnail' datastream:
```
  def can_download?
    datastream.dsid == 'thumbnail'
  end
```