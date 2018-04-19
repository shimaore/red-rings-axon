Axon backend compatible with abrasive-ducks
-------------------------------------------

This backend is meant to be integrated in an abrasive-ducks project or similar.

    axon_backend = (options) ->
      pub = Axon.socket 'pub'
      sub = Axon.socket 'sub'

      pub.bind options.pub
      sub.bind options.sub

      (sources) ->

Messages from the bus to the clients.

        sources
        .continueWith ->
          pub.close()
          most.empty()
        .filter has_key

Note: filtering for `SUBSCRIBE` is an early optimization that might be removed. Client code should not rely on it (and should still dispatch based on the operation).

        .filter operation SUBSCRIBE
        .forEach (msg) ->
          pub.send msg

Messages from the clients to the bus.

        most
        .fromEvent 'message', sub

    module.exports = axon_backend
    {operation,has_key} = require 'abrasive-ducks-transducers'
    {SUBSCRIBE} = require 'red-rings/operations'
    Axon = require 'axon'
