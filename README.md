# playground-proxy-protocol

Experimentations with PROXY protocol.

https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt

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

netcat of haproxy writing a version 2 of the PROXY protocol header:
```
netcat_1        | 00000000  0d 0a 0d 0a 00 0d 0a 51  55 49 54 0a 21 11 00 0c  |.......QUIT.!...|
netcat_1        | 00000010  ac 13 00 01 ac 13 00 03  a6 52 00 50 47 45 54 20  |.........R.PGET |
netcat_1        | 00000020  2f 20 48 54 54 50 2f 31  2e 31 0d 0a 48 6f 73 74  |/ HTTP/1.1..Host|
netcat_1        | 00000030  3a 20 6c 6f 63 61 6c 68  6f 73 74 3a 38 30 38 30  |: localhost:8080|
netcat_1        | 00000040  0d 0a 55 73 65 72 2d 41  67 65 6e 74 3a 20 63 75  |..User-Agent: cu|
netcat_1        | 00000050  72 6c 2f 37 2e 36 38 2e  30 0d 0a 41 63 63 65 70  |rl/7.68.0..Accep|
netcat_1        | 00000060  74 3a 20 2a 2f 2a 0d 0a  0d 0a                    |t: */*....|
netcat_1        | 0000006a
```

This one starts with the constant 12 bytes block: `\x0D \x0A \x0D \x0A \x00 \x0D \x0A \x51 \x55 \x49 \x54 \x0A` of the protocol.

## nginx writing PROXY protocol

- Only supports writing version 1: https://trac.nginx.org/nginx/ticket/1639

