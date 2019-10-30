Simple application to test the Heroku deployment with Nginx and Unicorn

## Versions
- Ruby 2.6.5
- Rails 5.2.3

## Preperation
#### Add unicorn gem
```ruby
gem 'unicorn'
```

#### Update Unicorn Config
```ruby
worker_processes Integer(ENV["WEB_CONCURRENCY"] || 3)
timeout 15
preload_app true
worker_processes 4
listen 'unix:///tmp/nginx.socket', backlog: 1024

before_fork do |server,worker|
    FileUtils.touch('/tmp/app-initialized')
end
```

#### Create Procfile directly under directory and edit
`web: bin/start-nginx bundle exec unicorn -c config/unicorn.rb`

## Set up (before deploy)
#### Create Heroku app url
`$ heroku create`
#### Update buildpack for Ruby
`$ heroku buildpacks:add heroku/ruby`
#### Update buildpack for Nginx
`$ heroku buildpacks:add heroku-community/nginx`
#### Push to Heroku
`$ Heroku push origin master`
#### Run datebase migration
`$ heroku run rails db:migrate`