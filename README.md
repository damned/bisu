# bisu
bundle install speed up

bundler is great in lots of ways, but it seems to me that if your workflow doesn't match its workflow, you can spend a lot of time waiting for bundle install.

as i see it there are a couple of difficulties:
* different bundle commands or config effectively uninstall gems
* caching is only performed at the installable gem level - so reinstall is necessary for any uninstalled gems
* bundler likes to reinstall on each machine/env - workflows that create different executable artifacts for different environments tend to cause a lot of re-installing (because you either need to clean non-env gems or install multiple times to different paths)

implementation idea - use git as installed-gem cache:
* install the superset of gems to (e.g.) vendor/all folder (no without configured) as initial commit
* copy Gemfile.lock (+ at some point OS info manifest) to repo too, so can check installed gem cache is applicable 
* copy repo to known shared location (e.g. /var/cache/bisu) and/or push to github
* to install gems into fresh project, checkout git repo into bundle path, e.g. git clone --shared /var/cache/bisu/myproject.bundle vendor/bundle
* to get env-specific installed gems do bundle install --local with --clean and appropriate --without
* to install another env first resurrect the cleaned gems with git -C vendor/bundle checkout .
