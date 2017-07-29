### Do you want to request a feature or report a bug?

I want to report a bug (or a lack of documentation)

### What did you do?

I'm trying to make websocket handshake working with traefik and docker

### What did you expect to see?

Websocket handshake succeeded => 101 Switching Protocols

### What did you see instead?

Websocket hanshake failed => 403 Forbidden

### Output of traefik version: (What version of Traefik are you using?)

1.3.2

### What is your environment & configuration (arguments, toml, provider, platform, ...)?

Here is my traefik.toml conf:
```toml
defaultEntryPoints = ["http", "https", "ws", "wss"]

[web]
address = ":8080"

[entryPoints]

[entryPoints.http]
 address = ":80"
 
[entryPoints.http.redirect]
entryPoint = "https"

[entryPoints.https]
 address = ":443"
[entryPoints.https.tls]
      [[entryPoints.https.tls.certificates]]
      CertFile = "/ssl/server.crt"
      KeyFile = "/ssl/server.key"

[docker]
endpoint = "unix:///var/run/docker.sock"
domain = "example.com"
watch = true
exposedbydefault = false
```
And docker side, here is my service container conf:
```
labels:

      - "traefik.network=app-network"
      - "traefik.protocol=http"
      - "traefik.frontend.entryPoints=https"
      - "traefik.port=9000"
      - "traefik.backend=app-main"
      - "traefik.frontend.rule=Host:app-main.docker.localhost"
      - "traefik.frontend.rule=PathPrefix:/,/logo,/avatars,/internal/users/findByEmail,/internal/users,/wsApp" #(I think when / is specified the other paths are useless?)
If applicable, please paste the log output in debug mode (--debug switch)

time="2017-07-06T15:33:12Z" level=debug msg="Writing outgoing Websocket request to target connection: &{Method:GET URL:ws:/wsApp/129/mcmpg5fu/websocket Proto:HTTP/1.1 ProtoMajor:1 ProtoMinor:1 Header:map[Sec-Websocket-Extensions:[permessage-deflate; client_max_window_bits] Sec-Websocket-Version:[13] Sec-Websocket-Key:[ZsG8z67B9AGPSQrzpnD9uw==] Connection:[Upgrade] Accept-Language:[fr-FR,fr;q=0.8,en-US;q=0.6,en;q=0.4] Upgrade:[websocket] Cookie:[SESSION=a8849fb7-84fe-40a1-a124-7f064cd93422] Cache-Control:[no-cache] Pragma:[no-cache] User-Agent:[Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36] Accept-Encoding:[gzip, deflate, br] Origin:[https://192.168.99.100]] Body:{} GetBody:<nil> ContentLength:0 TransferEncoding:[] Close:false Host:192.168.99.100 Form:map[] PostForm:map[] MultipartForm:<nil> Trailer:map[] RemoteAddr:192.168.99.1:60832 RequestURI:/wsApp/129/mcmpg5fu/websocket TLS:0xc4208d49a0 Cancel:<nil> Response:<nil> ctx:0xc4206c16c0}"
time="2017-07-06T15:33:12Z" level=debug msg="Switching protocol failed"
```

I read no special configuration is required to make websocket working, so I couldn' find any doc on the subject but I can't deal with it...
