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
