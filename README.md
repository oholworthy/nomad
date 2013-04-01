# nomad

A Clojure library designed to allow Clojure configuration to travel between hosts.

You can use Nomad to define and access host-specific configuration,
which can be saved and tracked through your usual version control
system. For example, when you're developing a web application, you may
want the web port to be different between development and production
instances, or you may want to send out e-mails to clients (or not!)
depending on the host that the application is running on.

While this does sound an easy thing to do, I have found myself coding
this in many different projects, so it was time to turn it into a
separate dependency!

## Usage

### Set-up

Add the ``nomad`` dependency to your ```project.clj```

```clojure
[jarohen/nomad "0.1.0"]
```

Nomad expects your configuration to be stored in an [1](EDN) file
called ``nomad-config.edn`` in the root of your classpath. Nomad does
expect a particular structure for your configuration, however it will
load any data structure in the file.

To load the data structure in the file, use the ```get-config``` function:

nomad-config.edn:

```clojure
{:my-key "my-value"}
```

your-ns.clj:

```clojure
(require '[nomad :refer [get-config]])

(get-config)
;; -> {:my-key "my-value"}
```

### Differentiating between hosts

To differentiate between different hosts, put the configuration for
each host under a ```:hosts``` key, then under a string key for the given
hostname, as follows:

```clojure
{:hosts {"my-laptop" {:key1 "dev-value"}
         "my-web-server" {:key1 "prod-value"}}}
```

To access the configuration for the current host, call
```get-host-config```:

```clojure
(require '[nomad :refer [get-host-config]])

(:key1 (get-host-config))
;; On "my-laptop", will return "dev-value"
;; On "my-web-server", will return "prod-value"
```


## License

Copyright © 2013 James Henderson

Distributed under the Eclipse Public License, the same as Clojure.