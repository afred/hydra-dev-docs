See the "[Rails Guides](http://guides.rubyonrails.org/form_helpers.html#uploading-files)" for the basics of file uploads.

```ruby
file = params[:file_data] # An instance of Rack::Multipart::UploadedFile
file_asset = FileAsset.new
file_asset.add_file_datastream(file, :label=>file.original_filename, :mimeType=>file.content_type, :dsid=>'content')
#the model instance (extending ActiveFedora::Base) you want to attach the file to.
file_asset.container = @holding_asset
file_asset.save
```