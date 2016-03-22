
# Launch configuration

Træfɪk can be configured using a TOML file configuration, arguments, or both.
By default, Træfɪk will try to find a `traefik.toml` in the following places:

- `/etc/traefik/`
- `$HOME/.traefik/`
- `.` *the working directory*

You can override this by setting a `configFile` argument:

```bash
$ traefik --configFile=foo/bar/myconfigfile.toml
```

Træfɪk uses the following precedence order. Each item takes precedence over the item below it:

- arguments
- configuration file
- default

It means that arguments overrides configuration file.
Each argument is described in the help section:
```bash
$ traefik --help
traefik is a modern HTTP reverse proxy and load balancer made to deploy microservices with ease.
Complete documentation is available at http://traefik.io

Usage:
  traefik [flags]
  traefik [command]

Available Commands:
  version     Print version

Flags:
      --accessLogsFile string                Access logs file (default "log/access.log")
      --boltdb                               Enable Boltdb backend
      --boltdb.endpoint string               Boltdb server endpoint (default "127.0.0.1:4001")
      --boltdb.filename string               Override default configuration template. For advanced users :)
      --boltdb.prefix string                 Prefix used for KV store (default "/traefik")
      --boltdb.watch                         Watch provider (default true)
  -c, --configFile string                    Configuration file to use (TOML, JSON, YAML, HCL).
      --consul                               Enable Consul backend
      --consul.endpoint string               Comma sepparated Consul server endpoints (default "127.0.0.1:8500")
      --consul.filename string               Override default configuration template. For advanced users :)
      --consul.prefix string                 Prefix used for KV store (default "/traefik")
      --consul.tls                           Enable Consul TLS support
      --consul.tls.ca string                 TLS CA
      --consul.tls.cert string               TLS cert
      --consul.tls.insecureSkipVerify        TLS insecure skip verify
      --consul.tls.key string                TLS key
      --consul.watch                         Watch provider (default true)
      --consulCatalog                        Enable Consul catalog backend
      --consulCatalog.domain string          Default domain used
      --consulCatalog.endpoint string        Consul server endpoint (default "127.0.0.1:8500")
      --defaultEntryPoints value             Entrypoints to be used by frontends that do not specify any entrypoint (default &main.DefaultEntryPoints(nil))
      --docker                               Enable Docker backend
      --docker.domain string                 Default domain used
      --docker.endpoint string               Docker server endpoint. Can be a tcp or a unix socket endpoint (default "unix:///var/run/docker.sock")
      --docker.filename string               Override default configuration template. For advanced users :)
      --docker.tls                           Enable Docker TLS support
      --docker.tls.ca string                 TLS CA
      --docker.tls.cert string               TLS cert
      --docker.tls.insecureSkipVerify        TLS insecure skip verify
      --docker.tls.key string                TLS key
      --docker.watch                         Watch provider (default true)
      --entryPoints value                    Entrypoints definition using format: --entryPoints='Name:http Address::8000 Redirect.EntryPoint:https' --entryPoints='Name:https Address::4442 TLS:tests/traefik.crt,tests/traefik.key'
      --etcd                                 Enable Etcd backend
      --etcd.endpoint string                 Comma sepparated Etcd server endpoints (default "127.0.0.1:4001")
      --etcd.filename string                 Override default configuration template. For advanced users :)
      --etcd.prefix string                   Prefix used for KV store (default "/traefik")
      --etcd.tls                             Enable Etcd TLS support
      --etcd.tls.ca string                   TLS CA
      --etcd.tls.cert string                 TLS cert
      --etcd.tls.insecureSkipVerify          TLS insecure skip verify
      --etcd.tls.key string                  TLS key
      --etcd.watch                           Watch provider (default true)
      --file                                 Enable File backend
      --file.filename string                 Override default configuration template. For advanced users :)
      --file.watch                           Watch provider (default true)
  -g, --graceTimeOut string                  Timeout in seconds. Duration to give active requests a chance to finish during hot-reloads (default "10")
  -l, --logLevel string                      Log level (default "ERROR")
      --marathon                             Enable Marathon backend
      --marathon.domain string               Default domain used
      --marathon.endpoint string             Marathon server endpoint. You can also specify multiple endpoint for Marathon (default "http://127.0.0.1:8080")
      --marathon.filename string             Override default configuration template. For advanced users :)
      --marathon.watch                       Watch provider (default true)
      --maxIdleConnsPerHost int              If non-zero, controls the maximum idle (keep-alive) to keep per-host.  If zero, DefaultMaxIdleConnsPerHost is used
      --providersThrottleDuration duration   Backends throttle duration: minimum duration between 2 events from providers before applying a new configuration. It avoids unnecessary reloads if multiples events are sent in a short amount of time. (default 2s)
      --traefikLogsFile string               Traefik logs file (default "log/traefik.log")
      --web                                  Enable Web backend
      --web.address string                   Web administration port (default ":8080")
      --web.cerFile string                   SSL certificate
      --web.keyFile string                   SSL certificate
      --web.readOnly                         Enable read only API
      --zookeeper                            Enable Zookeeper backend
      --zookeeper.endpoint string            Comma sepparated Zookeeper server endpoints (default "127.0.0.1:2181")
      --zookeeper.filename string            Override default configuration template. For advanced users :)
      --zookeeper.prefix string              Prefix used for KV store (default "/traefik")
      --zookeeper.watch                      Watch provider (default true)

Use "traefik [command] --help" for more information about a command.
```