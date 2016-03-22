
# Basics


Træfɪk is a modern HTTP reverse proxy and load balancer made to deploy microservices with ease.
It supports several backends ([Docker :whale:](https://www.docker.com/), [Mesos/Marathon](https://mesosphere.github.io/marathon/), [Consul](https://consul.io/), [Etcd](https://coreos.com/etcd/), Rest API, file...) to manage its configuration automatically and dynamically.

Basically, Træfɪk is an http router, which sends traffic from frontends to http backends, following rules you have configured.

## Frontends

Frontends can be defined using the following rules:

- `Headers`: Headers adds a matcher for request header values. It accepts a sequence of key/value pairs to be matched. For example: `application/json`
- `HeadersRegexp`: Regular expressions can be used with headers as well. It accepts a sequence of key/value pairs, where the value has regex support. For example: `application/(text|json)`
- `Host`: Host adds a matcher for the URL host. It accepts a template with zero or more URL variables enclosed by `{}`. Variables can define an optional regexp pattern to be matched: `www.traefik.io`, `{subdomain:[a-z]+}.traefik.io`
- `Methods`: Methods adds a matcher for HTTP methods. It accepts a sequence of one or more methods to be matched, e.g.: `GET`, `POST`, `PUT`
- `Path`: Path adds a matcher for the URL path. It accepts a template with zero or more URL variables enclosed by `{}`. The template must start with a `/`. For exemple `/products/` `/articles/{category}/{id:[0-9]+}`
- `PathStrip`: Same as `Path` but strip the given prefix from the request URL's Path.
- `PathPrefix`: PathPrefix adds a matcher for the URL path prefix. This matches if the given template is a prefix of the full URL path.
- `PathPrefixStrip`: Same as `PathPrefix` but strip the given prefix from the request URL's Path.


 A frontend is a set of rules that forwards the incoming http traffic to a backend.
 You can optionally enable `passHostHeader` to forward client `Host` header to the backend.

## HTTP Backends

A backend is responsible to load-balance the traffic coming from one or more frontends to a set of http servers.
Various methods of load-balancing is supported:

- `wrr`: Weighted Round Robin
- `drr`: Dynamic Round Robin: increases weights on servers that perform better than others. It also rolls back to original weights if the servers have changed.

A circuit breaker can also be applied to a backend, preventing high loads on failing servers.
Initial state is Standby. CB observes the statistics and does not modify the request.
In case if condition matches, CB enters Tripped state, where it responds with predefines code or redirects to another frontend.
Once Tripped timer expires, CB enters Recovering state and resets all stats.
In case if the condition does not match and recovery timer expries, CB enters Standby state.

It can be configured using:

- Methods: `LatencyAtQuantileMS`, `NetworkErrorRatio`, `ResponseCodeRatio`
- Operators:  `AND`, `OR`, `EQ`, `NEQ`, `LT`, `LE`, `GT`, `GE`

For example:

- `NetworkErrorRatio() > 0.5`: watch error ratio over 10 second sliding window for a frontend
- `LatencyAtQuantileMS(50.0) > 50`:  watch latency at quantile in milliseconds.
- `ResponseCodeRatio(500, 600, 0, 600) > 0.5`: ratio of response codes in range [500-600) to  [0-600)
