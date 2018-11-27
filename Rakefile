require 'html-proofer'
require 'rubocop/rake_task'
require 'jekyll'
require 'jsonlint/rake_task'

# rubocop:disable Metrics/LineLength
URLS = []
  # returning 403s
  %r{https:\/\/www.aircanada.com},
  %r{https:\/\/www.alaskaair.com},
  %r{https:\/\/www.costco.com},
  %r{https:\/\/www.dell.com},
  %r{https:\/\/www.delta.com},
  %r{https:\/\/www.gilt.com},
  %r{https:\/\/www.rakuten.com},
  %r{https:\/\/www.wayfair.com},
  %r{https:\/\/jet.com},
  %r{https:\/\/aternos.org},
  %r{https:\/\/www.boxed.com},
  %r{https:\/\/www.eprice.it},
  %r{https:\/\/www.lowes.com},
  %r{https:\/\/www.thebay.com},
  %r{https:\/\/www.udemy.com},
  %r{https:\/\/www.zazzle.com},
  # 302s
  %r{https:\/\/www.optionsxpress.com},
  %r{https:\/\/help.ea.com\/en-us\/help\/account\/origin-login-verification-information\/},
  %r{https:\/\/docs.connectwise.com\/ConnectWise_Control_Documentation\/Get_started\/Administration_page\/Security_page\/Enable_two-factor_authentication_for_host_accounts},
  %r{https:\/\/www.ankama.com},
  %r{https:\/\/socialclub.rockstargames.com},
  # timeout
  %r{https:\/\/www.united.com},
  %r{https:\/\/pogoplug.com},
  # SSL errors
  %r{https:\/\/www.overstock.com},
  %r{https:\/\/www.macys.com},
  %r{https:\/\/www.kohls.com},
  %r{https:\/\/www.inexfinance.com},
  %r{https:\/\/docs.cloudmanager.mongodb.com\/core\/two-factor-authentication},
  %r{https:\/\/support.snapchat.com\/en-US\/article\/enable-login-verification},
  %r{https:\/\/www.openprovider.co.uk},
  # other
  %r{https:\/\/mega.nz},
  %r{https:\/\/www.fasttech.com},
  %r{https:\/\/www.domaintools.com},
  %r{https:\/\/maxemail.emailcenteruk.com},
  # uses DDoS protection
  %r{https:\/\/coinone.co.kr*},
  %r{https:\/\/www.btcmarkets.net},
  # see https://github.com/google/fonts/issues/473#issuecomment-331329601
  %r{https:\/\/fonts.googleapis.com\/css\/*}
].freeze
# rubocop:enable Metrics/LineLength

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
           disable_external: false, \
           cache: { timeframe: '1w' }, \
           check_sri: true, \
           url_ignore: URLS }.merge!(YAML.safe_load(args.opts, [Symbol]))
  HTMLProofer.check_directory(args.target, opts).run
end

JsonLint::RakeTask.new do |t|
  t.paths = %w[_site/data.json]
end

task :verify do
  ruby './verify.rb'
end

RuboCop::RakeTask.new
