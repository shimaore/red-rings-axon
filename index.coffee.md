Client for a Axon red-ring (typically used on a backend application)
---------------------------

This client is meant to be embedded in a backend application such as `huge-play`, `black-metal`, `well-groomed-feast`, the traces code, etc.

    {RedRing} = require 'red-rings'

    class RedRingAxon extends RedRing

- `options.publish_to` are the connection options for our local publishers; in other words it is a list of remote subscribers (using the `backend` module).
- `options.subscribe_to` are the connection options for our local subscribers; in other words it is a list of remote publishers (using the `backend` module).

      constructor: (options) ->
        super()
        @pub = Axon.socket 'pub'
        options.publish_to?.forEach (o) => @pub.connect o
        @sub = Axon.socket 'sub'
        options.subscribe_to?.forEach (o) => @sub.connect o
        return

      disconnect: ->
        @pub.close()
        @sub.close()

Messages from the client to the bus.

      send: (msg) ->
        @pub.send msg

Messages from the bus to the client.

      source_of: (key) ->

We are copying the filtering code from `Axon/sockets/sub` because we need one stream per pattern, not one stream that might have any number of patterns,
and that having one connection per pattern would be inefficient as well.

        key = toRegExp key
        most
        .fromEvent 'message', @sub
        .filter (msg) ->
          if typeof msg.key is 'string'
            return msg.key.match key
          if typeof msg.id is 'string'
            return msg.id.match key
          false

    module.exports = RedRingAxon
    Axon = require '@shimaore/axon'
    most = require 'most'

This code is from axon/sockets/sub.js

    escape = require 'escape-regexp'
    toRegExp = (str) ->
      return str if str instanceof RegExp
      str = escape str
      str = str.replace /\\\*/g, '(.+)'
      new RegExp "^#{str}$"
