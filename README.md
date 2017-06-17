# DHT Search

This is a basic DHT crawler and even more basic searcher of the indexed data.

Distributed Hash Table (DHT) is a distributed system storing key/value pairs,
in this case it is specifically Mainline DHT, the type used by BitTorrent
clients. The crawler also implements a number of extensions which enable it to
get the metadata for the torrent enabling indexing and later searching.

The crawler joins the DHT network and listens to the conversations between
nodes, keeping track of interesting packets. The most interesting packets are
those where another node announces they have a torrent available.

BitTorrent clients usually raise red flags regarding their legality. This
client utilises a number of protocols to stay in the DHT network and to
download the torrent metadata. The actual files hosted by the other node are
not retrieved.

## Installation

You will need the dependencies. They have been vendored using the
[dep](https://github.com/golang/dep) tool so installation with a recent Go
version should be as simple as:

```shell
$ go build
```

## Usage

You will need to create a PostgreSQL database using the `schema.sql` file
provided. You are going to need to sort out any port forwarding if you are
behind NAT so remote nodes can get to yours.

The following options are available:

    Usage of ./dht-search:
      -debug
            provide debug output
      -dsn string
            DB DSN (default "postgres://dht:dht@localhost/dht?sslmode=disable")
      -http string
            HTTP listen address:port (default "localhost:6880")
      -no-http
            no HTTP service
      -nodes int
            number of nodes to start (default 1)
      -port int
            listen port (and first of multiple ports) (default 6881)

These options enable you to start a number of DHT nodes thus implementing a
small scale [Sybil attack](https://en.wikipedia.org/wiki/Sybil_attack). The
first DHT node will take the port specified and each subsequent port is for the
following nodes.

## TODO

- Manage downloading threads to reduce memory consumption
- Add tests!
- Enable rate limiting
- Enable configuration of tags and reject patterns.
- Enable FTS on database queries.
- Improve our manners on the DHT network (replies etc.).
- Improve the routing table implementation.
- Improve the interface.
