#This file is generated by ModuleSync, do not edit.

source ENV['GEM_SOURCE'] || "https://rubygems.org"

# Determines what type of gem is requested based on place_or_version.
def gem_type(place_or_version)
  if place_or_version =~ /^git:/
    :git
  elsif place_or_version =~ /^file:/
    :file
  else
    :gem
  end
end

# Find a location or specific version for a gem. place_or_version can be a
# version, which is most often used. It can also be git, which is specified as
# `git://somewhere.git#branch`. You can also use a file source location, which
# is specified as `file://some/location/on/disk`.
def location_for(place_or_version, fake_version = nil)
  if place_or_version =~ /^(git[:@][^#]*)#(.*)/
    [fake_version, { :git => $1, :branch => $2, :require => false }].compact
  elsif place_or_version =~ /^file:\/\/(.*)/
    ['>= 0', { :path => File.expand_path($1), :require => false }]
  else
    [place_or_version, { :require => false }]
  end
end

# Used for gem conditionals
supports_windows = true
ruby_version_segments = Gem::Version.new(RUBY_VERSION.dup).segments
minor_version = "#{ruby_version_segments[0]}.#{ruby_version_segments[1]}"

# The following gems are not included by default as they require DevKit on Windows.
# You should probably include them in a Gemfile.local or a ~/.gemfile
#gem 'pry' #this may already be included in the gemfile
#gem 'pry-stack_explorer', :require => false
#if RUBY_VERSION =~ /^2/
#  gem 'pry-byebug'
#else
#  gem 'pry-debugger'
#end

group :development do
  gem "puppet-module-posix-default-r#{minor_version}",    :require => false, :platforms => "ruby"
  gem "puppet-module-win-default-r#{minor_version}",      :require => false, :platforms => ["mswin", "mingw", "x64_mingw"]
  gem "puppet-module-posix-dev-r#{minor_version}",        :require => false, :platforms => "ruby"
  gem "puppet-module-win-dev-r#{minor_version}", '0.0.7', :require => false, :platforms => ["mswin", "mingw", "x64_mingw"]
  gem "json_pure", '<= 2.0.1',                            :require => false if Gem::Version.new(RUBY_VERSION.dup) < Gem::Version.new('2.0.0')
  gem "fast_gettext", '1.1.0',                            :require => false if Gem::Version.new(RUBY_VERSION.dup) < Gem::Version.new('2.1.0')
  gem "fast_gettext",                                     :require => false if Gem::Version.new(RUBY_VERSION.dup) >= Gem::Version.new('2.1.0')
end

group :system_tests do
  gem "puppet-module-posix-system-r#{minor_version}",                            :require => false, :platforms => "ruby"
  gem "puppet-module-win-system-r#{minor_version}",                              :require => false, :platforms => ["mswin", "mingw", "x64_mingw"]
  gem "beaker", *location_for(ENV['BEAKER_VERSION'] || '>= 3')                  
  gem "beaker-pe",                                                               :require => false
  gem "beaker-rspec", *location_for(ENV['BEAKER_RSPEC_VERSION'])                
  gem "beaker-hostgenerator", *location_for(ENV['BEAKER_HOSTGENERATOR_VERSION'])
  gem "beaker-abs", *location_for(ENV['BEAKER_ABS_VERSION'] || '~> 0.1')        
  gem "puppet-blacksmith", '~> 3.4',                                             :require => false
end

gem 'puppet', *location_for(ENV['PUPPET_GEM_VERSION'])

# Only explicitly specify Facter/Hiera if a version has been specified.
# Otherwise it can lead to strange bundler behavior. If you are seeing weird
# gem resolution behavior, try setting `DEBUG_RESOLVER` environment variable
# to `1` and then run bundle install.
gem 'facter', *location_for(ENV['FACTER_GEM_VERSION']) if ENV['FACTER_GEM_VERSION']
gem 'hiera', *location_for(ENV['HIERA_GEM_VERSION']) if ENV['HIERA_GEM_VERSION']

# For Windows dependencies, these could be required based on the version of
# Puppet you are requiring. Anything greater than v3.5.0 is going to have
# Windows-specific dependencies dictated by the gem itself. The other scenario
# is when you are faking out Puppet to use a local file path / git path.
explicitly_require_windows_gems = false
puppet_gem_location = gem_type(ENV['PUPPET_GEM_VERSION'])
# This is not a perfect answer to the version check
if puppet_gem_location != :gem || (ENV['PUPPET_GEM_VERSION'] && Gem::Version.correct?(ENV['PUPPET_GEM_VERSION']) && Gem::Requirement.new('< 3.5.0').satisfied_by?(Gem::Version.new(ENV['PUPPET_GEM_VERSION'].dup)))
  if Gem::Platform.local.os == 'mingw32'
    explicitly_require_windows_gems = true
  end
  if puppet_gem_location == :gem
    # If facterversion hasn't been specified and we are
    # looking for a Puppet Gem version less than 3.5.0, we
    # need to ensure we get a good Facter for specs.
    gem "facter",">= 1.6.11","<= 1.7.5",:require => false unless ENV['FACTER_GEM_VERSION']
    # If hieraversion hasn't been specified and we are
    # looking for a Puppet Gem version less than 3.5.0, we
    # need to ensure we get a good Hiera for specs.
    gem "hiera",">= 1.0.0","<= 1.3.0",:require => false unless ENV['HIERA_GEM_VERSION']
  end
end

if explicitly_require_windows_gems
  # This also means Puppet Gem less than 3.5.0 - this has been tested back
  # to 3.0.0. Any further back is likely not supported.
  if puppet_gem_location == :gem
    gem "ffi", "1.9.0",                          :require => false
    gem "win32-eventlog", "0.5.3","<= 0.6.5",    :require => false
    gem "win32-process", "0.6.5","<= 0.7.5",     :require => false
    gem "win32-security", "~> 0.1.2","<= 0.2.5", :require => false
    gem "win32-service", "0.7.2","<= 0.8.8",     :require => false
    gem "minitar", "0.5.4",                      :require => false
  else
    gem "ffi", "~> 1.9.0",                       :require => false
    gem "win32-eventlog", "~> 0.5","<= 0.6.5",   :require => false
    gem "win32-process", "~> 0.6","<= 0.7.5",    :require => false
    gem "win32-security", "~> 0.1","<= 0.2.5",   :require => false
    gem "win32-service", "~> 0.7","<= 0.8.8",    :require => false
    gem "minitar", "~> 0.5.4",                   :require => false
  end

  gem "win32-dir", "~> 0.3","<= 0.4.9", :require => false
  gem "win32console", "1.3.2",          :require => false if RUBY_VERSION =~ /^1\./

  # sys-admin was removed in Puppet 3.7.0+, and doesn't compile
  # under Ruby 2.3 - so restrict it to Ruby 1.x
  gem "sys-admin", "1.5.6",             :require => false if RUBY_VERSION =~ /^1\./

  # Puppet less than 3.7.0 requires these.
  # Puppet 3.5.0+ will control the actual requirements.
  # These are listed in formats that work with all versions of
  # Puppet from 3.0.0 to 3.6.x. After that, these were no longer used.
  # We do not want to allow newer versions than what came out after
  # 3.6.x to be used as they constitute some risk in breaking older
  # functionality. So we set these to exact versions.
  gem "win32-api", "1.4.8",             :require => false
  gem "win32-taskscheduler", "0.2.2",   :require => false
  gem "windows-api", "0.4.3",           :require => false
  gem "windows-pr",  "1.2.3",           :require => false
else
  if Gem::Platform.local.os == 'mingw32'
    # If we're using a Puppet gem on windows, which handles its own win32-xxx gem dependencies (Pup 3.5.0 and above), set maximum versions
    # Required due to PUP-6445
    gem "win32-dir", "<= 0.4.9",        :require => false
    gem "win32-eventlog", "<= 0.6.5",   :require => false
    gem "win32-process", "<= 0.7.5",    :require => false
    gem "win32-security", "<= 0.2.5",   :require => false
    gem "win32-service", "<= 0.8.8",    :require => false
  end
end

# Evaluate Gemfile.local if it exists
if File.exists? "#{__FILE__}.local"
  eval(File.read("#{__FILE__}.local"), binding)
end

# Evaluate ~/.gemfile if it exists
if File.exists?(File.join(Dir.home, '.gemfile'))
  eval(File.read(File.join(Dir.home, '.gemfile')), binding)
end

# vim:ft=ruby
