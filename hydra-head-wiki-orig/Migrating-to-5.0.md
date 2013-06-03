More important:

* IMPORTANT - Be aware that solrizer 2.0.0 now only indexes those fields you explicitly tell it to index.  If you were expecting automatic indexing, adjust your OM terminologies to set: ```index_as: [:searchable]```

* IMPORTANT - Do NOT create ```update()``` methods in the catalog controller.  Blacklight uses ```Catalog#update``` method to store pagination information into the session.  Thus hydra is no longer enforcing any access controls on ```Catalog#update```.

* Hydra-head drops support for ruby 1.8.  We support versions 1.9.3+ and the upcoming 2.0.0 release 

* The generated configs are for Solr 4.0 (upgraded from 1.4).  It ought to work with other versions of solr if you provide your own configs.

* remove ```User.send(:include, Hydra::GenericUserAttributes)``` from config/initializers/hydra_config.rb

* replace any instance of ```include Hydra::Catalog``` with ```include Hydra::Controller::ControllerBehavior```

* Methods in Ability class have ceased passing around user and session in the method signature (they're now relying on instance variables). If you've overridden any of these methods, you may receive an error like: `ArgumentError: wrong number of arguments (0 for 2)`  You ought to be able to just remove these parameters from the signature.

* If you have a line like this in your catalog controller: ```CatalogController.solr_search_params_logic << :add_access_controls_to_solr_params```, change it to ```CatalogController.solr_search_params_logic += [:add_access_controls_to_solr_params]``` otherwise your controller permissions may bleed over into other blacklight queries. 


Less important: 

* index and show templates (for the catalog controller) used to be in app/views/%{format}/_%{action_name} but now are just the Blacklight default locations (e.g. catalog/_%{action_name}_partials/%{format})
* If you previously ran the hydra generator it created a method: ApplicationController#layout_name.  Ensure it is returning a value corresponding to a template in your app/views/layouts directory, or remove it altogether to use the default blacklight template.

* hydra-core no longer overrides the blacklight views in any way. If you want to keep the old views copy them from a copy of the hydra-head 4.x gem into your application

* Hydra::Controller no longer includes Hydra::Controller::RepositoryControllerBehavior by default as all it's methods are deprecated.  If you still require RepositoryControllerBehavior, include it manually on your controller.

* the hydra-file-access gem is not required by default.  If you need those behaviors, add ```gem 'hydra-file-access'``` to your Gemfile.

* ```get_file_asset_count(solr_doc)``` has been replaced by ```fedora_doc.file_asset_count``` (or just ```fedora_doc.parts.length``` if you prefer)

* Hydra::AssetsControllerHelper moved to hydra-file-access. Do not use this unless you are certain you need it.
* If you include Hydra::AssetsControllerHelper in models you created to get the apply_depositor_metadata method, youll want to change that to include Hydra::ModelMethods