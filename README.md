# sshtunnel
Docker SSH Tunnel

The below example is the same as SSH'ing to `host.example.com` on port `22`, then forwarding port `2525` on the local machine to `localhost (which is host.example.com)` port `25` using the identity file `keyfile`. The `ports` setting is only required if you want to use the tunnel on the host. It is not required for other containers to use the tunnel.

```
version: '3'

services:

  sshtunnel:
    image: nowsci/sshtunnel
    container_name: sshtunnel
    ports:
      - "2525:2525"
    volumes:
      - ./sshtunnel/data/:/data/:ro
    environment:
      - TUNNEL_HOST=host.example.com
      - TUNNEL_PORT=22
      - REMOTE_HOST=localhost
      - LOCAL_PORT=2525
      - REMOTE_PORT=25
      - KEY=/data/keyfile
      - VERBOSE=true
      - USER=root
    restart: always
```

Or for multiple ports:

```
version: '3'

services:

  sshtunnel:
    image: nowsci/sshtunnel
    container_name: sshtunnel
    ports:
      - "2525:2525"
    volumes:
      - ./sshtunnel/data/:/data/:ro
    environment:
      - TUNNEL_HOST=host.example.com
      - TUNNEL_PORT=22
      - REMOTE_HOST=localhost,localhost
      - LOCAL_PORT=2525,2526
      - REMOTE_PORT=25,26
      - KEY=/data/keyfile
      - VERBOSE=true
      - USER=root
    restart: always
```

To use the tunnel in reverse mode, allowing a port on a remote server to redirect to a port on a local container, use the following. The below example is the same as SSH'ing to `host.example.com` on port `22`, then forwarding port `8080` on the remote machine to the container `nginx (which is on the local machine)` with port `80` using the identity file `keyfile`.

```
version: '3'

services:

  sshtunnel:
    image: nowsci/sshtunnel
    container_name: sshtunnel
    volumes:
      - ./sshtunnel/data/:/data/:ro
    environment:
      - REMOTE=true
      - TUNNEL_HOST=host.example.com
      - TUNNEL_PORT=22
      - CONTAINER_HOST=nginx
      - CONTAINER_PORT=80
      - REMOTE_PORT=8080
      - KEY=/data/keyfile
      - USER=root
      # Optional
      # - LISTEN_HOST=0.0.0.0
      - VERBOSE=true
    restart: always
```

Or for multiple ports:

```
version: '3'

services:

  sshtunnel:
    image: nowsci/sshtunnel
    container_name: sshtunnel
    volumes:
      - ./sshtunnel/data/:/data/:ro
    environment:
      - REMOTE=true
      - TUNNEL_HOST=host.example.com
      - TUNNEL_PORT=22
      - CONTAINER_HOST=nginx,nginx
      - CONTAINER_PORT=80,443
      - REMOTE_PORT=8080,4043
      - KEY=/data/keyfile
      - USER=root
      # Optional
      # - LISTEN_HOST=0.0.0.0
      - VERBOSE=true
    restart: always
```
Thank you to [Fmstrat/sshtunnel](https://github.com/Fmstrat/sshtunnel) for the original concept and code.
