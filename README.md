# playground-proxy-protocol

Experimentations with PROXY protocol.

## Results

netcat as the upstream of the edge server (nginx writing the PROXY protocol header):
```
netcat_1  | PROXY TCP4 172.19.0.1 172.19.0.3 42272 80
netcat_1  | GET / HTTP/1.1
netcat_1  | Host: localhost:8080
netcat_1  | User-Agent: curl/7.68.0
netcat_1  | Accept: */*
```

netcat as the upstream of the proxy server (nginx reading the PROXY protocol header):
```
netcat_1  | GET / HTTP/1.0
netcat_1  | Host: localhost
netcat_1  | X-Real-IP: 172.19.0.1
netcat_1  | X-Forwarded-For: 172.19.0.1
netcat_1  | Connection: close
netcat_1  | User-Agent: curl/7.68.0
netcat_1  | Accept: */*
```

## nginx writing PROXY protocol

- Only supports writing version 1: https://trac.nginx.org/nginx/ticket/1639

