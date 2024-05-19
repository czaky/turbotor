# turbotor
Tor on steroids abused in the wrong way. Plus some tools.

## Intro

The purpose of this project is to provide a fast, not-necessary private,
access to internet over TOR. This is achieved by abusing TOR in a specific **wrong** way.

The `docker-compose` with spawn 10 instances of TOR proxies that have been modified
to create 2 hop circuits by default. The 10 instances are hidden behind a `haproxy`,
which distributes the traffic in a round-robin fashion.

On top of this, we also provide here a [SearXNG](https://github.com/searxng/searxng/)
container using the 10 TOR proxies in parallel to search on multiple of engines "privately",
without the need to solve the CAPTCHA. Having the requests issued in parallel, makes this
approach bearable over TOR. It results in much faster response times and way less
engines being timed-out or being "suspended".

## Access ports

 * TOR is available as `tubotor-hostname`:9050
 * SearXNG is available as `turbotor-hostname`:8080
 * HAProxy stats are available on `turbotor-hostname`:8404

 Here `turbotor-hostname` will most likely be "localhost".

## Installation

Install docker-compose if not yet done.

*Note if running on WSL, and using docker-desktop,
enable docker integration
(Settings -> Resources -> WSL Integration),
instead of directly installing docker-compose into WSL.*

```bash
sudo apt-get install docker-compose
```

In this directory:

```bash
docker-compose build

docker-compose up -d
```

## Configuration

Configuration for each component are stored in:

* ./tor - currently sets Entry and Exit nodes to *US*.
* ./haproxy.cfg - enables HAProxy stats as well
* ./searxng - disables some engines not working over TOR
and sets up parallel TOR proxy access using `proxy_request_redundancy` argument.

## Images

The docker-compose and docker files build several images:
 * tor - compiled from (modified) latest tor source
 * searxng - compiled from https://github.com/czaky/searxng, which adds ability to issue parallel requests over multiple proxies

## Containers

Following containers are created:

 * 10 tor containers
 * 1 haproxy
 * 1 redis
 * 1 searxng
