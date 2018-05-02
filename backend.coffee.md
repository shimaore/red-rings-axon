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
        .forEach (msg) ->
          pub.send msg

Messages from the clients to the bus.

        most
        .fromEvent 'message', sub

    module.exports = axon_backend
    Axon = require 'axon'
    most = require 'most'
