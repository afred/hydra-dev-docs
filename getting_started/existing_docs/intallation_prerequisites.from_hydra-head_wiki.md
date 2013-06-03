# Installation Prerequisites (Hydra-Head)

-   [ruby](http://www.ruby-lang.org/en/)
    -   ruby 1.9.3 is required by hydra-head gem release 4.0
    -   ruby 1.8.7 is used by release 3.3 and below. RVM (see below) expects ree 1.8.7 ([Ruby Enterprise Edition](http://www.rubyenterpriseedition.com/))

-   [git](http://git-scm.com/)
-   [java](http://www.java.com/en/) NOTE: version 1.6 or higher
-   [sqlite3](http://www.sqlite.org/)
-   [RVM](http://rvm.beginrescueend.com/) (Ruby Version Manager)
    -   We *strongly* suggest using RVM as a means of keeping your different ruby applications with their specific gem requirements from having version clashes. The instructions assume the use of RVM.
    -   If you don’t have the desired ruby version in RVM, e.g. ruby 1.9.3 or ree 1.8.7, then install it. \
         <code>rvm install ruby-1.9.3</code>
    -   NOTE: OS X 10.7 users; XCode no longer has a real gcc, and ruby 1.8.7 needs the real deal. If you are using homebrew, you can get gcc with this command\
         <code>brew install https://raw.github.com/Homebrew/homebrew-dupes/master/apple-gcc42.rb</code>
    -   After installing RVM, set it up to use [.rvmrc files](http://beginrescueend.com/workflow/rvmrc/) when they’re available. This will cause RVM to automatically switch Ruby versions and gemsets when you cd into different code bases. Each component or Hydra Head uses its own gemset (i.e. hydra-head uses a gemset called hyhead, hypatia uses a gemset called hypatia) so that you can use a different set of dependencies for each.

-   [rubygems](http://rubygems.org/pages/download)
-   these ruby gems:
    -   [rails](http://rubyonrails.org/)
        -   rails version 3.0.x (not 3.1) for hydra-head gem releases 3.0 - 4.0
        -   rails 2.3.11 is used by hydra-head release 2.x
            -   [bundler](http://gembundler.com/) must be explicitly installed for rails 2.