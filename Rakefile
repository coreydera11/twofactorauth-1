require 'html-proofer'
require 'rubocop/rake_task'
require 'jekyll'
require 'jsonlint/rake_task'

task default: %w[proof verify jsonlint rubocop]

task :build do
  config = Jekyll.configuration(
    'source' => './',
    'destination' => './_site'
  )
  site = Jekyll::Site.new(config)
  Jekyll::Commands::Build.build site, config
end

task :proof, %i[target opts] => 'build' do |_t, args|
  args.with_defaults(target: './_site', opts: '{}')
  opts = { assume_extension: true, \
           check_html: true, \
           disable_external: true, \
           cache: { timeframe: '1w' }, \
           check_sri: true, \
           url_ignore: [
             # see https://github.com/google/fonts/issues/473#issuecomment-331329601
             %r{https:\/\/fonts.googleapis.com\/css\/*}
           ] }.merge!(YAML.safe_load(args.opts, [Symbol]))
  HTMLProofer.check_directory(args.target, opts).run
end

JsonLint::RakeTask.new do |t|
  t.paths = %w[_site/data.json]
end

task :verify do
  ruby './verify.rb'
end

RuboCop::RakeTask.new
