Backend CCNQ4 messaging using Axon
----------------------------------

The package includes two modules:
- `backend`, which is meant to run (for example) inside an `abrasive-ducks` process to provide endpoints for multiple clients to connect to;
- `index`, which is meant to run on a CCNQ4 application client.

Here an "application client" is an application such as `huge-play`, `black-metal`, etc., which typically needs to receive SUBSCRIBE messages and send NOTIFY responses to a proxy such as `abrasive-ducks`.
The `.receive` method of the `index` modules relies on Axon's subscriber's and therefor accepts `*` as a pattern.
