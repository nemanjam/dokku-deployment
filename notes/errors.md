### Errors

```bash
-----> Getting letsencrypt certificate for nextjs-app via HTTP-01
        - Domain 'nextjs-app.dokku.arm1.localhost3002.live'
docker: open /home/ubuntu/traefik-proxy/apps/dokku/dokku-data/home/dokku/nextjs-app/letsencrypt/certs/ac00fb3b1783f8750bfd5ca350e514d4918ca459/docker.env: no such file or directory.

-----> Enabling ACME proxy for nextjs-app...
       ok: run: nginx: (pid 16434) 473s
-----> Getting letsencrypt certificate for nextjs-app via HTTP-01
        - Domain 'nextjs-app.dokku.arm1.localhost3002.live'
docker: open /home/ubuntu/traefik-proxy/apps/dokku/dokku-data/home/dokku/nextjs-app/letsencrypt/certs/ac00fb3b1783f8750bfd5ca350e514d4918ca459/docker.env: no such file or directory.
See 'docker run --help'.
-----> Certificate retrieval failed!
-----> Disabling ACME proxy for nextjs-app...
       ok: run: nginx: (pid 16434) 474s
 !     Failed to setup letsencrypt
 !     Check log output for further information on failure
```

```bash
ubuntu@arm1:~/traefik-proxy$ docker exec -it dokku bash
root@2c3660d832dd:/tmp# cat /home/dokku/nextjs-app/letsencrypt/certs/ac00fb3b1783f8750bfd5ca350e514d4918ca459/docker.env
root@2c3660d832dd:/tmp# ls -la /home/dokku/nextjs-app/letsencrypt/certs/ac00fb3b1783f8750bfd5ca350e514d4918ca459/docker.env
-rwxr-xr-x 1 dokku dokku 0 Feb 25 17:20 /home/dokku/nextjs-app/letsencrypt/certs/ac00fb3b1783f8750bfd5ca350e514d4918ca459/docker.env
root@2c3660d832dd:/tmp#

ubuntu@arm1:~/traefik-proxy$ ls -la /home/ubuntu/traefik-proxy/apps/dokku/dokku-data/home/dokku/nextjs-app/letsencrypt/certs/ac00fb3b1783f8750bfd5ca350e514d4918ca459/docker.env
-rwxr-xr-x 1 200 200 0 Feb 25 17:20 /home/ubuntu/traefik-proxy/apps/dokku/dokku-data/home/dokku/nextjs-app/letsencrypt/certs/ac00fb3b1783f8750bfd5ca350e514d4918ca459/docker.env

sudo vi /home/dokku/nextjs-app/letsencrypt/certs/ac00fb3b1783f8750bfd5ca350e514d4918ca459/docker.env

```

---

```bash
# set global email
dokku config:set --global DOKKU_LETSENCRYPT_EMAIL=miroljub.petrovic.acc@gmail.com

# install manually, removed from plugin-list
dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git --committish 0.17.0

# enable, fails unauthorized 403
dokku letsencrypt:enable nextjs-app

ubuntu@arm1:~/dokku-deployment$ dokku letsencrypt:enable nextjs-app
=====> Enabling letsencrypt for nextjs-app
-----> Enabling ACME proxy for nextjs-app...
ok: run: nginx: (pid 9859) 1593s
-----> Getting letsencrypt certificate for nextjs-app... - Domain 'nextjs-app.dokku.arm1.localhost3002.live'
2023/02/28 14:01:17 No key found for account miroljub.petrovic.acc@gmail.com. Generating a P256 key.
2023/02/28 14:01:17 Saved key to /certs/accounts/acme-v02.api.letsencrypt.org/miroljub.petrovic.acc@gmail.com/keys/miroljub.petrovic.acc@gmail.com.key
2023/02/28 14:01:18 [INFO] acme: Registering account for miroljub.petrovic.acc@gmail.com
!!!! HEADS UP !!!!

       Your account credentials have been saved in your Let's Encrypt
       configuration directory at "/certs/accounts".

       You should make a secure backup of this folder now. This
       configuration directory will also contain certificates and
       private keys obtained from Let's Encrypt so making regular
       backups of this folder is ideal.
       2023/02/28 14:01:18 [INFO] [nextjs-app.dokku.arm1.localhost3002.live] acme: Obtaining bundled SAN certificate
       2023/02/28 14:01:18 [INFO] [nextjs-app.dokku.arm1.localhost3002.live] AuthURL: https://acme-v02.api.letsencrypt.org/acme/authz-v3/207093113556
       2023/02/28 14:01:18 [INFO] [nextjs-app.dokku.arm1.localhost3002.live] acme: Could not find solver for: tls-alpn-01
       2023/02/28 14:01:18 [INFO] [nextjs-app.dokku.arm1.localhost3002.live] acme: use http-01 solver
       2023/02/28 14:01:18 [INFO] [nextjs-app.dokku.arm1.localhost3002.live] acme: Trying to solve HTTP-01
       2023/02/28 14:01:25 [INFO] Deactivating auth: https://acme-v02.api.letsencrypt.org/acme/authz-v3/207093113556
       2023/02/28 14:01:25 Could not obtain certificates:
        error: one or more domains had a problem:
       [nextjs-app.dokku.arm1.localhost3002.live] acme: error: 403 :: urn:ietf:params:acme:error:unauthorized :: 141.147.18.176: Invalid response from http://nextjs-app.dokku.arm1.localhost3002.live/.well-known/acme-challenge/81DG6A9ypwUEtOZzuQI1koy9axvSneNFh12MwNs9vl4: 502

-----> Certificate retrieval failed!
-----> Disabling ACME proxy for nextjs-app...
ok: run: nginx: (pid 9859) 1601s
! Failed to setup letsencrypt
! Check log output for further information on failure
ubuntu@arm1:~/dokku-deployment$


ubuntu@arm1:~/dokku-deployment$ dokku nginx:error-logs nextjs-app
2023/02/28 14:01:19 [error] 39220#39220: *3 connect() failed (111: Connection refused) while connecting to upstream, client: 18.191.188.157, server: nextjs-app.dokku.arm1.localhost3002.live, request: "GET /.well-known/acme-challenge/81DG6A9ypwUEtOZzuQI1koy9axvSneNFh12MwNs9vl4 HTTP/1.1", upstream: "http://127.0.0.1:7954/.well-known/acme-challenge/81DG6A9ypwUEtOZzuQI1koy9axvSneNFh12MwNs9vl4", host: "nextjs-app.dokku.arm1.localhost3002.live"
2023/02/28 14:01:19 [error] 39220#39220: *5 connect() failed (111: Connection refused) while connecting to upstream, client: 18.237.0.140, server: nextjs-app.dokku.arm1.localhost3002.live, request: "GET /.well-known/acme-challenge/81DG6A9ypwUEtOZzuQI1koy9axvSneNFh12MwNs9vl4 HTTP/1.1", upstream: "http://127.0.0.1:7954/.well-known/acme-challenge/81DG6A9ypwUEtOZzuQI1koy9axvSneNFh12MwNs9vl4", host: "nextjs-app.dokku.arm1.localhost3002.live"
2023/02/28 14:01:19 [error] 39220#39220: *7 connect() failed (111: Connection refused) while connecting to upstream, client: 23.178.112.107, server: nextjs-app.dokku.arm1.localhost3002.live, request: "GET /.well-known/acme-challenge/81DG6A9ypwUEtOZzuQI1koy9axvSneNFh12MwNs9vl4 HTTP/1.1", upstream: "http://127.0.0.1:7954/.well-known/acme-challenge/81DG6A9ypwUEtOZzuQI1koy9axvSneNFh12MwNs9vl4", host: "nextjs-app.dokku.arm1.localhost3002.live"


ubuntu@arm1:~/dokku-deployment$ dokku nginx:report nextjs-app
=====> nextjs-app nginx information
       Nginx access log format:
       Nginx access log path:         /var/log/nginx/nextjs-app-access.log
       Nginx bind address ipv4:
       Nginx bind address ipv6:       ::
       Nginx client max body size:
       Nginx disable custom config:   false
       Nginx error log path:          /var/log/nginx/nextjs-app-error.log
       Nginx global hsts:             true
       Nginx computed hsts:           true
       Nginx hsts:
       Nginx hsts include subdomains: true
       Nginx hsts max age:            15724800
       Nginx hsts preload:            false
       Nginx computed nginx conf sigil path: nginx.conf.sigil
       Nginx global nginx conf sigil path: nginx.conf.sigil
       Nginx nginx conf sigil path:
       Nginx proxy buffer size:       4096
       Nginx proxy buffering:         on
       Nginx proxy buffers:           8 4096
       Nginx proxy busy buffers size: 8192
       Nginx proxy read timeout:      60s
       Nginx last visited at:         1677593048
       Nginx x forwarded for value:   $remote_addr
       Nginx x forwarded port value:  $server_port
       Nginx x forwarded proto value: $scheme
       Nginx x forwarded ssl:

```
