#!/usr/bin/env rake
require 'rubygems'
require 'bundler'

Bundler.require
require 'rake'
require 'rake/testtask'
require 'bundler/gem_tasks'
require 'appraisal'
begin
  require 'cucumber/rake/task'
rescue LoadError
  $stderr.puts "Please install cucumber: `gem install cucumber`"
  exit 1
end

#########################################
### TESTS
#########################################

desc 'Default: run unit tests.'
task :default => [:test]
#task :default => [:test, "cucumber:rails:all"]

desc "Clean out the tmp directory"
task :clean do
  exec "rm -rf tmp/*"
end

desc 'Test unit'
Rake::TestTask.new(:test) do |test|
  test.libs << 'lib'
  test.test_files = ['test/unit/mongodb_logger_test.rb']
  test.verbose = true
end

namespace :test do
  desc "Run replica set tests (not for CI)"
  Rake::TestTask.new(:replica_set) do |test|
    test.libs << 'lib'
    test.pattern = 'test/unit/mongodb_logger_replica_test.rb'
    test.verbose = true
  end
end

def cucumber_opts
  opts = "--tags ~@wip --format progress "

  opts << ENV["FEATURE"] and return if ENV["FEATURE"]

  case ENV["BUNDLE_GEMFILE"]
  when /rails/
    opts << "features/rails.feature"
  when /rack/
    opts << "features/rack.feature"
  end
end

Cucumber::Rake::Task.new(:cucumber) do |t|
  t.fork = true
  t.cucumber_opts = cucumber_opts
end

begin
  require 'jasmine'
  load 'jasmine/tasks/jasmine.rake'
rescue LoadError
  task :jasmine do
    abort "Jasmine is not available. In order to run jasmine, you must: (sudo) gem install jasmine"
  end
end
