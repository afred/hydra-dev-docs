## About the Bundled Jetty {#Hydra'sBundledJetty-AbouttheBundledJetty}

When you download the Hydrangea project, there should be a folder in\
 there called "jetty." That's actually linked to another github\
 project:

[https://github.com/projecthydra/hydra-jetty](https://github.com/projecthydra/hydra-jetty)

hydra-jetty contains a copy of jetty with Fedor and Solr pre-installed for you. All you need to do is grab that code and run it.

In most cases, you grab this code by running these commands at the root of your working directory:

[?](#)

`git submodule init`{.java .plain}

`git submodule update`{.java .plain}

### Application-specific Configuration files for Jetty {#Hydra'sBundledJetty-Application-specificConfigurationfilesforJetty}

Each Hydra head is likely to require some application-specific changes to the solr configuration files. If you save those config files in "solr/config", the hydra:jetty rake tasks will help you load them into jetty. You will also know where to grab them when you're spinning up your production solr instance on a server.

### Running Jetty {#Hydra'sBundledJetty-RunningJetty}

If you have pulled the code into the "jetty" directory, you can now use the hydra:jetty rake tasks to run it.

hydra:jetty:start will start jetty for you\
 hydra:jetty:stop will stop jetty for you\
 hydra:jetty:load will load your application-specific solr configuration files into jetty before starting it

## Tips & Tricks {#Hydra'sBundledJetty-Tips&Tricks}

### An alternative way to use hydra-jetty {#Hydra'sBundledJetty-Analternativewaytousehydra-jetty}

When my copy of jetty got messed up, I removed the entire jetty folder and re-\
 downloaded the whole hydra-jetty project from github and made a\
 separate subproject within my hydrangea app. So from your hydrangea\
 RAILS\_ROOT:

[?](#)

`rm -Rf jetty`{.java .plain}

`mkdir jetty`{.java .plain}

`cd jetty`{.java .plain}

`git init`{.java .plain}

`git remote add upstream https:`{.java .plain}`//github.com/projecthydra/hydra-jetty`{.java .comments}

`git pull`{.java .plain}

Or something to that effect... Now I can treat the entire jetty\
 folder as it's own project and make local branches within it if I need\
 to test out changes to any of the xml config files. Although, at the\
 moment I'm not changing any of it because I'm not that savvy with Solr\
 yet.

Make sure you're not running any other outside Fedora or Solr apps,\
 and when you run rake hydra:jetty:load it should startup the Fedora\
 and Solr webapps from within the jetty folder of your hydrangea app.\
 If it's working correctly, you'll be able to access both Fedora and\
 Solr:

<http://localhost:8983/solr/> <http://localhost:8983/fedora>
