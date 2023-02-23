### Add ssh key for Git

```bash
# prefix, see alias
docker exec -it dokku bash dokku

# from local dev
cat ~/.ssh/id_rsa.pub | ssh root@dokku.arm1.localhost3002.live docker exec -it dokku bash dokku ssh-keys:add admin

# on server
cat ~/id_rsa.pub | docker exec -it dokku bash dokku ssh-keys:add admin

```