### Introduction

> A tunnel similiar to kcptun but primarily used in private networks (LANs).
> Need one server with public ip and one udp port for hole digging. 

For example, you setup a private game server in LAN, and you want to invite your friends to play, you can try this tool.

### QuickStart

name server is used for service discovery, it must be deploy in a public network (ip).

```
Name Server: ./name_server -l ":4000" 
Client Proxy: ./client_proxy -r "NAME_SERVER_IP:4000" -l ":10022" -k 4321
Server Proxy: ./server_proxy -r "NAME_SERVER_IP:4000" -t "GAME_SERVER_IP:22"  -k 4321
```
The above commands will establish port forwarding channel from Client to Server as:

> game client ->  [client proxy]    <--  kcp -->    [server proxy]  -> game server

which tunnels the original connection:

> client -> server

### Install from source

```
$ go install github.com/opendarkeden/tunnel/...
```

#### Usage

```
server_proxy -h
NAME:
   server_proxy - server_proxy (with kcptun)

USAGE:
   server_proxy [global options] command [command options] [arguments...]

VERSION:
   SELFBUILD

COMMANDS:
   help, h  Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --targettcp value, -t value      target server address (default: "127.0.0.1:22")
   --listentcp value, -l value      local listen address (default: ":2022")
   --remoteudp value, -r value      kcp server address (default: "127.0.0.1:4000")
   --key value, -k value            p2p pair key (default: "1234")
   --passwd value                   pre-shared secret between client and server (default: "1234") [$KCPTUN_KEY]
   --crypt value                    aes, aes-128, aes-192, salsa20, blowfish, twofish, cast5, 3des, tea, xtea, xor, sm4, none (default: "none")
   --mode value                     profiles: fast3, fast2, fast, normal, manual (default: "fast")
   --conn value                     set num of UDP connections to server (default: 1)
   --autoexpire value               set auto expiration time(in seconds) for a single UDP connection, 0 to disable (default: 0)
   --mtu value                      set maximum transmission unit for UDP packets (default: 1350)
   --sndwnd value                   set send window size(num of packets) (default: 1024)
   --rcvwnd value                   set receive window size(num of packets) (default: 1024)
   --datashard value, --ds value    set reed-solomon erasure coding - datashard (default: 0)
   --parityshard value, --ps value  set reed-solomon erasure coding - parityshard (default: 0)
   --dscp value                     set DSCP(6bit) (default: 0)
   --nocomp                         disable compression
   --sockbuf value                  per-socket buffer in bytes (default: 4194304)
   --keepalive value                seconds between heartbeats (default: 10)
   --snmplog value                  collect snmp to file, aware of timeformat in golang, like: ./snmp-20060102.log
   --snmpperiod value               snmp collect period, in seconds (default: 60)
   --log value                      specify a log file to output, default goes to stderr
   --quiet                          to suppress the 'stream open/close' messages
   -c value                         config from json file, which will override the command from shell
   --help, -h                       show help
   --version, -v                    print the version
```

### References

1. https://github.com/xtaci/kcptun/ --- A Stable & Secure Tunnel Based On KCP with N:M Multiplexing
2. https://github.com/hikaricai/p2p_tun -- Based on this project

