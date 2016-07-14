# Redis

Redis is a key-value data store. In their website they say that they are a "data structure store".
This is because Redis can store more than just strings. It can also store hashes, lists, sets, among others.

## Redis as a Rails Cache

Using as a Rails Cache, there are gems that we could use:

- [readthis](https://github.com/sorentwo/readthis)
- [redis-rails](https://github.com/redis-store/redis-store)

# readthis

## Installation

~~~~~~~~~~ruby
gem 'readthis'
gem 'hiredis'
~~~~~~~~~~

- The other gem `hiredis` is a driver for redis.

## Configuration

- This gem is for caching purposes only. To configure Rails cache to use Redis:

~~~~~~~~~~ruby
config.cache_store = :readthis_store, {
  expires_in: 2.weeks.to_i,
  namespace: 'cache',
  redis: { url: ENV.fetch('REDIS_URL'), driver: :hiredis }
}
~~~~~~~~~~

## Usage

- Usage is similar to using the standard Rails caching methods

## Sidekiq and Redis

- Sidekiq uses Redis by default.
- It would be best to configure logging in the sidekiq initializer.

~~~~~~~~~ruby
Sidekiq.configure_server do |config|
  config.redis = {url: 'redis://localhost:6379/4'}
  Sidekiq::LOgging.logger.level = Logger::DEBUG
  Rails.logger = Sidekiq::Logging.logger
end
~~~~~~~~

## redis-browser

- This is a Web GUI for viewing the contents of a redis database.

### Installation

`gem install redis-browser`

### Usage

`redis-browser --url redis://host:port`

# Facebook Messenger Platform

## Template Responses

- Text Message
- Generic Template
- Button Template

## Text Message

- This is used to send simple text replies
~~~~~~~ruby
{:text => "MESSAGE HERE"}
~~~~~~~

## Generic Template

- Looks like cards
- Can contain images, buttons

~~~~~~~ruby
{
  :title => "String",
  :image_url => "string",
  :subtitle => "String",
  :buttons => [button_templates]
}
~~~~~~~

## Button Template

{
  :type => 'phone_number | web_url | postback',
  :title => "String",
  :url => 'Specific to web_url type',
  :payload => 'data to be sent back to the serber (postback) | phone_number (+<country><area>local_number)'
}


# Wit.ai

## Installation

`gem install wit`

## Configuration

This is the older syntax
`bot = Wit.new <actions_hash>, <app_id>`

Newer Syntax
`bot = Wit.new :access_token => <id>, :actions: <actions_hash>`

Actions (Older Syntax)

~~~~~~~ruby
actions = {
  :say => ->(session_id, context, msg){
    # Do Stuff HERE
  },
  :merge => ->(session_id, context, entities, msg){
  },
  :error => ->(session_id, context, entities, msg){
  },
  :customAction => ->(session_id, context, error){
  }
}
~~~~~~

Actions (New Syntax)

~~~~~~ruby
actions = {
  :send -> (request, response){
    # For sending text replies
  },
  :custom_action -> (request, response){}
}
~~~~~~

- Response Object:
~~~~~~~~~~~ruby
request = {
  'session_id' => session_id,
  'context' => context.clone,
  'text' => msg,
  'entities' => json['entities']
}
~~~~~~~~~~~

- Merge may still be part of the action hash where you update the context
- For errors, Wit just sends a message saying that it doesn't know what to do

## Stories creation
- "There is no need to have one story per possible path though —- that would be a nightmare; these stories train Wit and then Wit can combine aspects from different stories and behave well even in uncharted paths."
