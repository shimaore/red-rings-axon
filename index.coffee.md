Client for a Axon red-ring (typically used on a backend application)
---------------------------

This client is meant to be embedded in a backend application such as `huge-play`, `black-metal`, `well-groomed-feast`, the traces code, etc.

    {RedRing} = require 'red-rings'

    class RedRingAxon extends RedRing

- `options.publish_to` are the connection options for our local publishers; in other words it is a list of remote subscribers.
- `options.subscribe_to` are the connection options for our local subscribers; in other words it is a list of remote publishers.

      constructor: (options) ->
        super()
        @pubs = options.publish_to.map (o) ->
          s = Axon.socket 'pub'
          s.connect o
          s
        @subs = options.subcribe_to.map (o) ->
          s = Axon.socket 'sub'
          s.connect o
          s
        return

      disconnect: ->
        @pubs.forEach (s) -> s.close()
        @subs.forEach (s) -> s.close()

Messages from the client to the bus.

      send: (msg) ->
        @pubs.forEach (p) -> p.send msg

Messages from the bus to the client.

      source_of: (key) ->
        most
        .from @subs
        .map (s) -> most.fromEvent 'message', s
        .merge()

msg.key? should always be true because the `backend` filter those

        .filter (msg) -> msg.key is key

    module.exports = RedRingAxon
    Axon = require 'axon'
