'''
$ mkdir docker

$ cd docker/

$ git clone https://github.com/komal-max/Build-and-Push-Your-Own-Docker-Images.git
Cloning into 'Build-and-Push-Your-Own-Docker-Images'...
remote: Enumerating objects: 20, done.
remote: Counting objects: 100% (20/20), done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 20 (delta 4), reused 17 (delta 3), pack-reused 0
Receiving objects: 100% (20/20), 74.58 KiB | 14.92 MiB/s, done.
Resolving deltas: 100% (4/4), done.

$ cd Build-and-Push-Your-Own-Docker-Images/

$ cat Dockerfile 
FROM nginx:latest 
# use tag alpine to have a smaller image size 
# our base image is based on nginx and that is coming from dockerhub

COPY source/html /usr/share/nginx/html 
# Takes two parameters, source - copy what? and destination - copy where? 

# expose port, port 80 is used by default by nginx
#EXPOSE 80, we can comment it out

CMD ["nginx", "-g","daemon off;"]
# this is the same command that nginx image passes in when starts up by default
#we can comment out this line too if we like


$ docker build -t docker-build .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  226.8kB
Step 1/3 : FROM nginx:latest
latest: Pulling from library/nginx
f11c1adaa26e: Pull complete 
c6b156574604: Pull complete 
ea5d7144c337: Pull complete 
1bbcb9df2c93: Pull complete 
537a6cfe3404: Pull complete 
767bff2cc03e: Pull complete 
adc73cb74f25: Pull complete 
Digest: sha256:67682bda769fae1ccf5183192b8daf37b64cae99c6c3302650f6f8bf5f0f95df
Status: Downloaded newer image for nginx:latest
 ---> fffffc90d343
Step 2/3 : COPY source/html /usr/share/nginx/html
 ---> 585b211a1b11
Step 3/3 : CMD ["nginx", "-g","daemon off;"]
 ---> Running in c541e0c4de95
Removing intermediate container c541e0c4de95
 ---> fd7ed64030a3
Successfully built fd7ed64030a3
Successfully tagged docker-build:latest


$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED          SIZE
docker-build                  latest    fd7ed64030a3   16 seconds ago   188MB

$ docker run -d -p 80:80 fd7ed64030a3
6893082751ca4e0b237a31d36a2e2367a2e91221e4312292262641e5392342b4
'''

# Access

'''
http://localhost:80
'''

# Create a docker hub account

'''Login to docker hub and create a new repository'''

'''
$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED         SIZE
docker-build                  latest    fd7ed64030a3   6 minutes ago   188MB


$ docker tag docker-build:latest 782782/docker-images:0.2


$ docker login
Log in with your Docker ID or email address to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com/ to create one.
You can log in with your password or a Personal Access Token (PAT). Using a limited-scope PAT grants better security and is required for organizations using SSO. Learn more at https://docs.docker.com/go/access-tokens/

Username: 782782
Password: 
WARNING! Your password will be stored unencrypted in /home/ubuntu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
'''

# Push the local image to remote(docker hub), here repo created by own 
'''
$ docker push 782782/docker-images:0.2 
The push refers to repository [docker.io/782782/docker-images]
deaa280ccea4: Pushed 
56b6d3be75f9: Mounted from library/nginx 
0c6c257920c8: Mounted from library/nginx 
92d0d4e97019: Mounted from library/nginx 
7190c87a0e8a: Mounted from library/nginx 
933a3ce2c78a: Mounted from library/nginx 
32cfaf91376f: Mounted from library/nginx 
32148f9f6c5a: Mounted from library/nginx 
0.2: digest: sha256:11cdcbfd878978749620accc6827031ec6156787e46c3c8d363f495ef96c0916 size: 1987
'''

