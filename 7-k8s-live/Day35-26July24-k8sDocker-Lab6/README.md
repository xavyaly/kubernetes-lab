# For cleaning

'''
$ docker system prune 
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N] y

$ docker volume prune

$ docker network prune
'''

# Remove all the stopped containers

'''
$ docker rm -aq $(docker ps -a)
'''

# Development containers

[Link](https://docs.docker.com/guides/workshop/06_bind_mounts/)

# Bridge network

[Link](https://docs.docker.com/network/drivers/bridge/)

# Storage

[Link](https://docs.docker.com/storage/)

# Multi container apps

[Link](https://docs.docker.com/guides/workshop/07_multi_container/)

# Docker compose

[Link](https://docs.docker.com/guides/workshop/08_using_compose/)

# Multi Stage builds

[Links](https://docs.docker.com/guides/workshop/09_image_best/)

