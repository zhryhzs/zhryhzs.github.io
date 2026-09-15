source "https://rubygems.org"

# 与 GitHub Pages 线上环境保持一致（本地预览和线上构建结果就会一样）
gem "github-pages", group: :jekyll_plugins

group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-seo-tag"
  gem "jekyll-sitemap"
end

# Windows 平台需要的时区数据
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# Windows 下的文件监听加速（可选）
gem "wdm", "~> 0.1.1", platforms: [:mingw, :x64_mingw, :mswin]

# JRuby 平台
gem "http_parser.rb", "~> 0.6.0", platforms: [:jruby]
