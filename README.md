## Fresh setup

```
# check ruby version
ruby --version # must be 2.1.0 or higher

# Install bundler gem to your home directory
gem install --user-install bundler
```

Add the gem folder in your home directory to the path. Add the following line to ``$HOME/.bash_profile``
```
PATH="$(ruby -e 'puts Gem.user_dir')/bin:$PATH"
```

```
# Pick up the changes to $HOME/.bash_profile
source $HOME/.bash_profile

# Had some issues with the dependency nokogiri (https://www.nokogiri.org/tutorials/installing_nokogiri.html)
brew install libxml2
bundle config build.nokogiri --use-system-libraries --with-xml2-include=$(brew --prefix libxml2)/include/libxml2

# Install Jekyll and other dependencies from GitHub Pages gem. You might get permission error if you just use ``bundle install``.
bundle install --path vendor/bundle

# Server the site locally
bundle exec jekyll serve --incremental
```

## Installing new Gems
If you added a new gem to ``Gemfile``, run ``bundle install --path vendor/bundle`` to install it.

## Updating existing Gems
Run ``bundle update`` to update all the gems to the latest versions. Perform this as often as possible.
[jekyll docs - upgrading](https://jekyllrb.com/docs/upgrading/)
