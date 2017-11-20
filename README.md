# proxytunnel-docker

A small but useful docker image to proxytunnel ougoing connections e.g. to a foreign SMTP server.

How does it work: It uses the HTTP Connect Method in the proxy server which forwards traffic to a target server
unchanged. This is the same mechanism used if you HTTPS over a proxy server. To learn more read:

[HTTP tunnel](https://en.wikipedia.org/wiki/HTTP_tunnel)
[How to use the git protocol through a http connect proxy](https://www.emilsit.net/blog/archives/how-to-use-the-git-protocol-through-a-http-connect-proxy/)

Small example to talk to an external SMTP server in a cooperated environment over a proxy server 
(HTTP CONNECT method must be working on the proxy server)

```bash
git clone https://github.com/OliPelz/proxytunnel-docker.git
docker build . -t OliPelz/proxytunnel-docker
``` 

As an example:
To connect to your favourit outside SMTP server ```smtp.yourmailhost.com``` on smtp port ```587``` using a localhost port ```2222``` 
and using your cooperates HTTP firewall ```cooperate.proxyserver``` at port ```3128```

```bash
docker run -e PROXY_SERVER=cooperate.proxyserver \
  -e PROXY_PORT=3128 \
  -e TARGET_SERVER=smtp.yourmailhost.com \
  -e TARGET_PORT=587 \
  -e LOCAL_PORT=2222 \
  -p 555:2222
  olipelz/proxytunnel-docker
```

Now test your setup using your favorite cli mail sending tool or simply telnet

leave the docker container open, in a second window type
```bash
telnet localhost 555
```
if you are proxypassing a SMTP server, this should give you something like the following as accessible SMTP connection to return

```bash
Trying ::1...
Connected to localhost.
Escape character is '^]'.
220 smtp.yourmailhost.com ESMTP ready
```


Another tip If you are bypassing SMTP
To test SMTP connections I usually use [Swaks](http://www.jetmore.org/john/code/swaks/)

here is an example I would use

```bash
swaks -tls --to xxx@xxx.com --from xxx@yourmaihost.com --auth-user xxx@yourmailhost.com  --server localhost
```

