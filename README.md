# README

This repo is used to reproduce the issue xray does not work with squid outbound proxy. I setup the following environment to test. The service list comes from <https://stackoverflow.com/questions/30385254/getting-my-servers-requesting-ip-address-from-an-outgoing-curl-request>

The topology are

```mermaid
flowchart BT
    proxy[proxy on port 1080] --> |icanhazip.com ifconfig.me| xray[xray on port 1081]
    proxy --> |httpbin.org ipecho.net| squid[squid on port 3128]
```

- If the user access `icanhazip.com` or `ifconfig.me`, `proxy` will use `xray`
- If the user access `httpbin.org` or `ipecho.net`, `proxy` will use `xray`

```sh
# Use squid direct
curl --proxy http://localhost:3128 -Lv http://httpbin.org/get

# Use xray direct
curl --proxy http://localhost:1081 -Lv http://httpbin.org/get

# proxy using squid fails
curl --proxy http://localhost:1080 -Lv http://httpbin.org/get

# proxy using xray succeed
curl --proxy http://localhost:1080 -Lv http://ifconfig.me
```
