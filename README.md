lua-resty-dogstatsd
====

A client for DogStatsD, an extension of the StatsD metric server for Datadog.
Using nginx cosocket API.

## Installation

```
$ luarocks install lua-resty-dogstatsd
```

## Usage

```lua
-- Require Library
local resty_dogstatsd = require('resty_dogstatsd')

-- Initialize
local dogstatsd = resty_dogstatsd.new({
  statsd = {
    host = "127.0.0.1",
    port = 8125,
    namespace = "nginx_lua",
  },
  tags = {
    "environment:develop",
    "application:lua",
  },
})

-- DogStatsd Functions
dogstatsd:event('Anything happened', ngx.req.raw_header())
dogstatsd:service_check('lua', 0)

-- Statsd Functions
dogstatsd:gauge('lua.users', 100)
dogstatsd:counter('lua.events', 5)
dogstatsd:increment('lua.events', 1)
dogstatsd:decrement('lua.events', 3)
dogstatsd:timer('lua.page_render', 105)
dogstatsd:histogram('lua.page_render_time', 105)
dogstatsd:meter('lua.page_load', 1)
```

## Initialize Functions

* dogstatsd.new(config)
  * `config` (Table) -- Optional.
    * `statsd` -- [lua-resty-statsd config](https://github.com/mediba-system/lua-resty-statsd)
    * `tags` -- Default tags. Merge the tags of the function.

## DogStatsd Functions

* dogstatsd:event(title, text, meta)
  * `title` (String) -- Event title.
  * `text` (String) -- Event text. Supports line breaks.
  * `meta` (Table) -- Optional.
    * `date_happened` (Time) -- Assign a timestamp to the event. (Example: 1469106024)
    * `hostname` (String) -- Assign a hostname to the event. (Example: example.com)
    * `aggregation_key` (String) -- Assign an aggregation key to the event.
    * `priority` (String) -- Can be "normal" or "low".
    * `source_type_name` (String) -- Assign a source type to the event.
    * `alert_type` (String) -- Can be "error", "warning", "info" or "success".
    * `tags` (Table[String]) -- An table of tags (Example: {'file:sample.lua','line:100'})

* dogstatsd:service_check(name, status, meta)
  * `name` (String) -- Service check name string.
  * `status` (Integer) -- OK = 0, WARNING = 1, CRITICAL = 2, UNKNOWN = 3.
  * `meta` (Table) -- Optional.
    * `timestamp` (Time) -- Assign a timestamp to the event. (Example: 1469106024)
    * `hostname` (String) -- Assign a hostname to the event. (Example: example.com)
    * `message` (String) -- A message describing the current state of the service check.

## Statsd Functions

* dogstatsd:gauge(stat, value, sample_rate, tags)
  * `stat` (String) -- An identifier for the metric.
  * `value` (Integer) -- The value is a number that is associated with the metric.
  * `sample_rate` (Float) -- Sample Rate.

* dogstatsd:counter(stat, value, sample_rate, tags)
* dogstatsd:increment(stat, value, sample_rate, tags)
* dogstatsd:decrement(stat, value, sample_rate, tags)
* dogstatsd:timer(stat, ms, tags)
  * `ms` (Integer) -- Timers measure the amount of time an action took to complete, in milliseconds.

* dogstatsd:histogram(stat, value, tags)
* dogstatsd:meter(stat, value, tags)
* dogstatsd:set(stat, value, tags)

## Licence

[GPL v3](https://github.com/mediba-system/lua-resty-dogstatsd/blob/master/LICENCE)

## Author

* [mediba-system](https://github.com/mediba-system)
* [yoshida-mediba](https://github.com/yoshida-mediba)
