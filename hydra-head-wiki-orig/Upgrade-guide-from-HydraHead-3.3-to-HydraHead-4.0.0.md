(Upgrading from previous versions not covered by this document)

## Required changes

* Many of the routes in the application now prepend /hydra (e.g. /permissions is now /hydra/permissions).   Please change these routes in config/initializers/hydra_config.rb (permsissions/permissions_form -> hydra/permissions/permissions_form). Additional changed routes:

    <pre>
    asset_downloads_path        ->  hydra_asset_downloads_path
    asset_contributor_path      ->  hydra_asset_contributor_path
    asset_connect_path          ->  hydra_asset_connect_path
    file_asset_path             ->  hydra_file_asset_path
    new_asset_contributor_path  ->  new_hydra_asset_contributor_path</pre>

* The search partial has been removed from app/catalog/_home_text.html.erb.  Edit your copy of this file and remove the line:

    <code><%= render :partial=>'search_form' %></code>

* When a query is done from solr, there's no need to call the ".hits" method anymore.  We recommend you use the development environment of your choice to search for .hits in the app/ directory and remove any instance you find.
Then don't call .hits on the output, that's already done for you.

* If you're upgrading rails to use the asset pipeline:

  * Move everything in public/stylesheets to app/assets/stylesheets
  * Move everything in public/javascripts to app/assets/javascripts
  * Remove any styles/scripts provided by blacklight/hydra
  * add to gemfile
    <pre>
    group :assets do
      gem 'sass-rails',   '~> 3.2.3'
      gem 'coffee-rails', '~> 3.2.1'
      gem 'uglifier', '>= 1.0.3'
      gem 'compass-rails', '~> 1.0.0'
      gem 'compass-susy-plugin', '~> 0.9.0', :require => 'susy'
      # gem 'therubyracer'
    end
    </pre>
  * add to the top of config/application.rb
    <pre>
     if defined?(Bundler)
       # If you precompile assets before deploying to production, use this line
       Bundler.require(*Rails.groups(:assets => %w(development test)))
       # If you want your assets lazily compiled in production, use this line
       # Bundler.require(:default, :assets, Rails.env)
     end
    </pre>
    And this to the config section of that file:
    <pre>
    # Enable the asset pipeline
    config.assets.enabled = true    
    # Default SASS Configuration, check out https://github.com/rails/sass-rails for details
    config.assets.compress = !Rails.env.development?
    config.assets.debug = Rails.env.development?
    config.sass.line_comments = Rails.env.development?
    # Version of your assets, change this if you want to expire all your assets
    config.assets.version = '1.0'
    </pre>

## Optional changes (deprecations)

* Remove any instance of ActiveFedoraHelper from your code -- it's deprecated.  It would look like this:

    <code>require 'mediashelf/active_fedora_helper'</code>

* Remove any instance of:

    <code>before_filter :require_solr</code>

    This is no longer necessary, solr is automatically configured.


* ActiveFedora::SolrService.instance.conn.query should be changed to ActiveFedora::SolrService.query
  Then don't call .hits on the output, that's already done for you.