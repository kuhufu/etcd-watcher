Etcd Watcher [![Build Status](https://travis-ci.org/casbin/etcd-watcher.svg?branch=master)](https://travis-ci.org/casbin/etcd-watcher) [![Coverage Status](https://coveralls.io/repos/github/casbin/etcd-watcher/badge.svg?branch=master)](https://coveralls.io/github/casbin/etcd-watcher?branch=master) [![Godoc](https://godoc.org/github.com/casbin/etcd-watcher?status.svg)](https://godoc.org/github.com/casbin/etcd-watcher)
====

Etcd Watcher is the [Etcd](https://github.com/coreos/etcd) watcher for [Casbin](https://github.com/casbin/casbin). With this library, Casbin can synchronize the policy with the database in multiple enforcer instances.

## Installation

    go get github.com/kuhufu/etcd3-watcher

## Simple Example

```go
package main

import (
    "github.com/casbin/casbin"
    "log"
    etcdwatcher "github.com/kuhufu/etcd3-watcher"
)

func updateCallback(rev string) {
    log.Println("New revision detected:", rev)
}

func main() {
    // Initialize the watcher.
    // Use the endpoint of etcd cluster as parameter.
    w := etcdwatcher.NewWatcher("http://127.0.0.1:2379")
    
    // Initialize the enforcer.
    e := casbin.NewEnforcer("examples/rbac_model.conf", "examples/rbac_policy.csv")
    
    // Set the watcher for the enforcer.
    e.SetWatcher(w)
    
    // By default, the watcher's callback is automatically set to the
    // enforcer's LoadPolicy() in the SetWatcher() call.
    // We can change it by explicitly setting a callback.
    w.SetUpdateCallback(updateCallback)
    
    // Update the policy to test the effect.
    // You should see "[New revision detected: X]" in the log.
    e.SavePolicy()
}
```

## Getting Help

- [Casbin](https://github.com/casbin/casbin)

## License

This project is under Apache 2.0 License. See the [LICENSE](LICENSE) file for the full license text.
