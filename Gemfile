source 'https://rubygems.org'

git_source(:github) do |repo_name|
  repo_name = "#{repo_name}/#{repo_name}" unless repo_name.include?("/")
  "https://github.com/#{repo_name}.git"
end

gem 'rails', '~> 5.1.6'
gem 'sqlite3'
gem 'puma', '~> 3.7'
gem 'sass-rails', '~> 5.0'
gem 'uglifier', '>= 1.3.0'
gem 'coffee-rails', '~> 4.2'
gem 'turbolinks', '~> 5'
gem 'jbuilder', '~> 2.5'
gem 'administrate', '~> 0.8.1'
gem 'devise', '~> 4.4.0'
gem 'devise-bootstrapped', github: 'excid3/devise-bootstrapped', branch: 'bootstrap4'
gem 'devise_masquerade', '~> 0.6.0'
gem 'font-awesome-sass', '~> 4.7'
gem 'gravatar_image_tag', github: 'mdeering/gravatar_image_tag'
gem 'jquery-rails', '~> 4.3.1'
gem 'bootstrap', '~> 4.0.0.beta'
gem 'webpacker', '~> 3.0'
gem 'sidekiq', '~> 5.0'
gem 'foreman'
gem 'omniauth-facebook', '~> 4.0'
gem 'omniauth-twitter', '~> 1.4'
gem 'omniauth-github', '~> 1.3'

gem 'acts_as_list'

group :development, :test do
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
  gem 'capybara', '~> 2.13'
  gem 'selenium-webdriver'
end

group :development do
  gem 'web-console', '>= 3.3.0'
  gem 'listen', '>= 3.0.5', '< 3.2'
  gem 'spring'
  gem 'spring-watcher-listen', '~> 2.0.0'
end

gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]
