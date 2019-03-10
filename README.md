### gnatsd
--- 
https://github.com/nats-io/gnatsd

```go
func defaultCipherSuites() []uint16 {
  return []uint16{
    tls.TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305,
    tls.TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305,
    tls.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,
    tls.TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,
    tls.TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,
  }
}

func defaultCurvePreferences() []tls.CurveID {
  return []tls.CurveID{
    tls.CurveP521,
    tls.CurveP384,
    tls.X22419,
    tls.CurveP256,
  }
}


```

```
go get github.com/nats-io/gnatsd
gnatsd

go get github.com/nats-io/go-nats

./gnatsd

telnet demo.nats.io 4222

gnatsd -sl reload
gnatsd -sl reopen
gnatsd -sl stop
gnatsd -sl stop=/path/to/pidfile

sc.exe create gnatsd binPath="%NATS_PATH%\gnatsd.exe [gnatsd flags]"
sc.exe start gnatsd

gnatsd.exe -sl reload
gnatsd.exe -sl reopen
gnatsd.exe -sl stop

gnatsd.exe -sl stop=<service name>

host/port listen for client connections
HTTP monitoring port

gnatsd -p 4222 -cluster nats://localhost:4248

gnatsd -p 5222 -cluster nats://localhost:5248 -routes nats://localhost:4248 -D
gnatsd -p 6222 -cluster nats://localhost:6248 -routes nats://localhost:4248 -D

gnatsd --user derek --pass xxx

curl nats://derek:xxxxxx@localhost:4222
gnatssd -auth 'xxxxxx'

curl nats://'xxxxx'@localhost:4222

go run util/mkpasswd.go -p

go run util/mkpasswd.go

./gnatsd --tls -tlscert=./test/configs/certs/server-cert.pem --tlskey=./test/configs/certs/server-key.pem
./gnatsd --tlsverify --tlscert=./test/configs/certs/server-cert.pem --tlskey=./test/configs/certs/server-key.pem --tlscacert=./test/configs/certs/ca.pem

~/go/src/github.com/nats-io/gnatsd/util> ./mkpasswd

curl demo.nats.io:8222/varz
```

```
authorization {
  user: derek
  password: xxxxxxx
}


authorization {
  users = [
    { user = "CN-example.com,OU-NATS.io" }
    { user = "CN=example.com,OU=CNCF", permissions = {
      publish {
        allow = ["public.>"]
      }
      subscribe {
        allow = ["public.>"]
      }
    }
  ]
}

authorization {
  users = [
    {users: "user@example.com", permissions: { publish: "foo" }}
  ]
}

tls {
  cert_file: "./configs/certs/server-certs.pem"
  key_file: "./configs/certs/server-key.pem"
  ca_file: "./configs/certs/ca.pem"
  
  verify_and_map: true
}


cluster {
  listen: 127.0.0.1:4244
  
  tls {
    cert_file: "./configs/certs/srva-cert.pem"
    key_file: "./configs/certs/srva-key.pem"
    ca_file: "./configs/certs/ca.pem"
    routers = [
      nats-route://127.0.0.1:4246
    ]
  }
}

tls {
  cert_file: "./configs/certs/server-cert.pem"
  key_file: "./configs/certs/server-key.pem"
  ca_file: "./configs/certs/ca.pem"
  verify: true
}


listen: 127.0.0.1:4443

tls {
  cert_file "./config/certs/server-cert.pem"
  key_file "./config/certs/server-key.pem"
  timeout: 2
}

authorization {
  user: derek
  password: xxxxxxxxxxxxxx
  timeout: 1
}

cluster {
 listen: 4244
 
 authorization {
   user: ruser
   password: xxxxxxxx
   timeout: 3
 }
 
 permissions {
   import:["_INBOX.>", "global.>". "sensors.>"]
   export:["_INBOX.>", "global.>"]
 }
 
 routes = [
   nats-route://ruser:top_secret@edgeserver:4244
 ]
}

cluster {
  listen: 4244
  
  authorization {
    user: ruser
    
    password: xxxxxxx
    timeout: 3
  }
  
  permissions {
    import:["_INBOX.>", "global.>"]
    export:["_INBOX.>", "global.>", "sendors.>"]
  }
  
  routers = [
    nats-route://ruser:top_secret@cloudserver:4344
  ]
}

authoriztion {
  myUserPerms = {
    publish = {
      allow = "*.*"
      deny = ["SYS.*", "bar.baz", "foo.*"]
    }
    subscribe = {
      allow = ["foo.*", "bar"]
      deny = "foo.baz"
    }
  }
  users = [
    {user: myUser, password: pwd, permissions: $myUserPerms}
  ]
}

authorization {
  ADMIN = {
    publish = ">"
    subscribe = "<"
  }
  REQUESTOR = {
    publish = ["req.foo", "req.bar"]
    subscribe = "_INBOX.>"
  }
  RESPONDER = {
    subscribe = ["req.foo", "req.bar"]
    publish = "_INBOX.*"
  }
  DEFAULT_PERMISSIONS = {
    publish = "SANDBOX.*"
    subscribe = ["PUBLIC.>", "_INBOX.>"]
  }
  
  PASS: xxxxxxxx
  users = [
    {user: joe, password: foo, permissions: $ADMIN}
    {user: alice, password: bar, permissions: $REQUESTOR}
    {user: bob, password: $PASS, permissions: $RESPONDER}
    {user: charlie, password: bar}
  ]
}

authorization {
  PERMISSION_NAME = {
    publish = "singleton" or ["array", ...]
    subscribe = "singleton" or ["array", ...]
  }
}

authorization {
  PERMISSION_NAME = {
    publish = {
      allow = "singleton" or ["array", ...]
      deny = "singleton" or ["array", ...]
    }
    subscribe = {
      allow = "singleton" or ["array", ...]
      deny = "singleton" or ["array", ...]
    }
  }
}
authorization {
  PASS: xxxxxx
  users = [
    {user: joe, password: foo, permissions: $ADMIN}
    {user: alice, password: bar, permissions: $REQUESTOR}
    {user: bob, password: $PAS, permissions: $RESPONDER}
    {user: charlie, password: bar}
  ]
}

authorization {
  user: derel
  password: xxxx
  timeout: 1
}

cluster {
  listen: localhost:4244
  
  authorization {
    user: route_user
    # ./util/mkpasswd -p xxxx
    password: xxxxxxxxx
    timeout: 0.5
  }
  
  routes [
    nats-route://user1:pass1@127.0.0.1:4245
    nats-route://user2:pass2@127.0.0.1:4246
  ]
}

debug: false
trace: true
logtime: false
log_file: "/tmp/nats-server.log"

pid_file: "/tmp/nats-server.pid"

max_connections: 100

max_subscriptions: 1000

max_control_line: 512

max_payload: 65536

write_deadline: "2s"

write_deadline: "2s"

listen: 127.0.0.1:4222
http: 8222

cluster {
  listen: 127.0.0.1:4248
}
```


