require 'rubocop/rake_task'
require 'jekyll'

RuboCop::RakeTask.new

task default: %w(build verify rubocop)

task :verify do
  ruby './verify.rb'
end

task :build do
  config = Jekyll.configuration(
    'source' => './',
    'destination' => './_site'
  )
  site = Jekyll::Site.new(config)
  Jekyll::Commands::Build.build site, config
end
