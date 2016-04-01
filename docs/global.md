
# Global configuration

## Reference

```toml
# traefik.toml
################################################################
# Global configuration
################################################################

# Timeout in seconds.
# Duration to give active requests a chance to finish during hot-reloads
#
# Optional
# Default: 10
#
# graceTimeOut = 10

# Traefik logs file
# If not defined, logs to stdout
#
# Optional
#
# traefikLogsFile = "log/traefik.log"

# Access logs file
#
# Optional
#
# accessLogsFile = "log/access.log"

# Log level
#
# Optional
# Default: "ERROR"
#
# logLevel = "ERROR"

# Backends throttle duration: minimum duration between 2 events from providers
# before applying a new configuration. It avoids unnecessary reloads if multiples events
# are sent in a short amount of time.
#
# Optional
# Default: "2s"
#
# ProvidersThrottleDuration = "5s"

# If non-zero, controls the maximum idle (keep-alive) to keep per-host.  If zero, DefaultMaxIdleConnsPerHost is used.
# If you encounter 'too many open files' errors, you can either change this value, or change `ulimit` value.
#
# Optional
# Default: http.DefaultMaxIdleConnsPerHost
#
# MaxIdleConnsPerHost = 200

# Entrypoints to be used by frontends that do not specify any entrypoint.
# Each frontend can specify its own entrypoints.
#
# Optional
# Default: ["http"]
#
# defaultEntryPoints = ["http", "https"]

# Enable ACME (Let's Encrypt): automatic SSL
#
# Optional
#
# [acme]

# Email address used for registration
#
# Required
#
# email = "test@traefik.io"

# File used for certificates storage.
# WARNING, if you use Traefik in Docker, don't forget to mount this file as a volume.
#
# Required
#
# storageFile = "acme.json"

# Entrypoint to proxy acme challenge to.
# WARNING, must point to an entrypoint on port 443 
#
# Required
#
# entryPoint = "https"

# Enable on demand certificate. This will request a certificate from Let's Encrypt during the first TLS handshake for a hostname that does not yet have a certificate.
# WARNING, TLS handshakes will be slow when requesting a hostname certificate for the first time, this can leads to DoS attacks.
# WARNING, Take note that Let's Encrypt have rate limiting: https://community.letsencrypt.org/t/quick-start-guide/1631
#
# Optional
#
# onDemand = true

# CA server to use
# Uncomment the line to run on the staging let's encrypt server
# Leave comment to go to prod
#
# Optional
#
# caServer = "https://acme-staging.api.letsencrypt.org/directory"

# Domains list
# You can provide SANs (alternative domains) to each main domain
# WARNING, Take note that Let's Encrypt have rate limiting: https://community.letsencrypt.org/t/quick-start-guide/1631
# Each domain & SANs will lead to a certificate request.
#
# [[acme.domains]]
#   main = "local1.com"
#   sans = ["test1.local1.com", "test2.local1.com"]
# [[acme.domains]]
#   main = "local2.com"
#   sans = ["test1.local2.com", "test2x.local2.com"]
# [[acme.domains]]
#   main = "local3.com"
# [[acme.domains]]
#   main = "local4.com"


# Entrypoints definition
#
# Optional
# Default:
# [entryPoints]
#   [entryPoints.http]
#   address = ":80"
#
# To redirect an http entrypoint to an https entrypoint (with SNI support):
# [entryPoints]
#   [entryPoints.http]
#   address = ":80"
#     [entryPoints.http.redirect]
#       entryPoint = "https"
#   [entryPoints.https]
#   address = ":443"
#     [entryPoints.https.tls]
#       [[entryPoints.https.tls.certificates]]
#       CertFile = "integration/fixtures/https/snitest.com.cert"
#       KeyFile = "integration/fixtures/https/snitest.com.key"
#       [[entryPoints.https.tls.certificates]]
#       CertFile = "integration/fixtures/https/snitest.org.cert"
#       KeyFile = "integration/fixtures/https/snitest.org.key"
#
# To redirect an entrypoint rewriting the URL:
# [entryPoints]
#   [entryPoints.http]
#   address = ":80"
#     [entryPoints.http.redirect]
#       regex = "^http://localhost/(.*)"
#       replacement = "http://mydomain/$1"

# Enable retry sending request if network error
#
# Optional
#
# [retry]

# Number of attempts
#
# Optional
# Default: (number servers in backend) -1
#
# attempts = 3

# Sets the maximum request body to be stored in memory in Mo
#
# Optional
# Default: 2
#
# maxMem = 3
```

## Samples

### HTTP only

```
defaultEntryPoints = ["http"]
[entryPoints]
  [entryPoints.http]
  address = ":80"
```

### HTTP + HTTPS (with SNI)

```
defaultEntryPoints = ["http", "https"]
[entryPoints]
  [entryPoints.http]
  address = ":80"
  [entryPoints.https]
  address = ":443"
    [entryPoints.https.tls]
      [[entryPoints.https.tls.certificates]]
      CertFile = "integration/fixtures/https/snitest.com.cert"
      KeyFile = "integration/fixtures/https/snitest.com.key"
      [[entryPoints.https.tls.certificates]]
      CertFile = "integration/fixtures/https/snitest.org.cert"
      KeyFile = "integration/fixtures/https/snitest.org.key"
```

### HTTP redirect on HTTPS

```
defaultEntryPoints = ["http", "https"]
[entryPoints]
  [entryPoints.http]
  address = ":80"
    [entryPoints.http.redirect]
    entryPoint = "https"
  [entryPoints.https]
  address = ":443"
    [entryPoints.https.tls]
      [[entryPoints.https.tls.certificates]]
      certFile = "tests/traefik.crt"
      keyFile = "tests/traefik.key"
```

### Let's Encrypt support

```
[entryPoints]
  [entryPoints.https]
  address = ":443"
    [entryPoints.https.tls]
      # certs used as default certs
      [[entryPoints.https.tls.certificates]]
      certFile = "tests/traefik.crt"
      keyFile = "tests/traefik.key"
[acme]
email = "test@traefik.io"
storageFile = "acme.json"
onDemand = true
caServer = "http://172.18.0.1:4000/directory"
entryPoint = "https"

[[acme.domains]]
  main = "local1.com"
  sans = ["test1.local1.com", "test2.local1.com"]
[[acme.domains]]
  main = "local2.com"
  sans = ["test1.local2.com", "test2x.local2.com"]
[[acme.domains]]
  main = "local3.com"
[[acme.domains]]
  main = "local4.com"
```

### Override entrypoints in frontends

```
[frontends]
  [frontends.frontend1]
  backend = "backend2"
    [frontends.frontend1.routes.test_1]
    rule = "Host:test.localhost"
  [frontends.frontend2]
  backend = "backend1"
  passHostHeader = true
  entrypoints = ["https"] # overrides defaultEntryPoints
    [frontends.frontend2.routes.test_1]
    rule = "Host:{subdomain:[a-z]+}.localhost"
  [frontends.frontend3]
  entrypoints = ["http", "https"] # overrides defaultEntryPoints
  backend = "backend2"
    rule = "Path:/test"
```
