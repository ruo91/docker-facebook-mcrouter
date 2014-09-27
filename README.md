# Facebook - MCRouter
See [Link] for more information.

### Run container
```
root@ruo91:~# docker run -d --name="mcrouter" -h "mcrouter" ruo91/facebook-mcrouter:1.x
```
or

### Build MCRouter
```
root@ruo91:~# git clone https://github.com/ruo91/docker-facebook-mcrouter /opt/docker-facebook-mcrouter
root@ruo91:~# docker build --rm -t mcrouter /opt/docker-facebook-mcrouter
```

### Run container
```
root@ruo91:~# docker run -d --name="mcrouter" -h "mcrouter" mcrouter
```

### Login SSH
Passwrods: mcrouter
```
root@ruo91:~# ssh `docker inspect -f '{{ .NetworkSettings.IPAddress }}' mcrouter`
```

### Run Memcached
```
root@mcrouter:~# memcached -m 64 -p 11211 -u memcache -l 127.0.0.1 &
root@mcrouter:~# memcached -m 64 -p 11212 -u memcache -l 127.0.0.1 &
```

### MCRouter
- Start
```
root@mcrouter:~# mcrouter \
--config-str='
{
     "pools":{
         "A":{
             "servers":[
                 "127.0.0.1:11211",
                 "127.0.0.1:11212"
             ]
         }
     },
     "route":"PoolRoute|A"
}' \
-p 5000
```
```
I0917 14:45:18.952839   264 main.cpp:528] mcrouter --config-str=
{
    "pools":{
        "A":{
            "servers":[
                "127.0.0.1:11211",
                "127.0.0.1:11212"
            ]
        }
    },
    "route":"PoolRoute|A"
} -p 5000
I0917 14:45:18.953008   264 main.cpp:625] mcrouter 1.0 startup (264)
I0917 14:45:18.964162   264 proxy.cpp:956] reconfigured 1 proxies with 2 clients and 1 pools (5d05d51d667fcfb3d02d403b09669eca)
I0917 14:45:18.964607   264 server.cpp:141] Spawning AsyncMcServer
```

- Test
```
root@mcrouter:~# echo -ne "get key\r\n" | nc 0 5000
```
![MCRouter][mcrouter]

Thanks :)
[Link]: https://github.com/facebook/mcrouter/wiki
[mcrouter]: http://cdn.yongbok.net/ruo91/img/facebook/mcrouter/facebook_mcrouter.png
