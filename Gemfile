source "https://rubygems.org"
ruby RUBY_VERSION

# 指定 Jekyll 4.0 版本
gem "jekyll", "~> 4.0"

# 可选插件
group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-sitemap"
  gem "jekyll-redirect-from"
  gem "jekyll-seo-tag"
  gem "kramdown-parser-gfm"
  gem 'bootstrap', '~> 5.0'
  
  # 使用最新版本的 sassc，确保兼容性
  gem 'sassc', '~> 2.4'
end

# Windows-specific gems
install_if -> { RUBY_PLATFORM =~ %r!mingw|mswin|java! } do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# 提高 Windows 目录监听性能
gem "wdm", "~> 0.1.1", :install_if => Gem.win_platform?
