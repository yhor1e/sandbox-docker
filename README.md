# sandbox-docker

## Docker for beginners

ref: https://github.com/docker/labs/blob/master/beginner/readme.md

- [x] 1.0 Running your first container
  - `-it`
    - `-i, --interactive                    Keep STDIN open even if not attached`
    - `-t, --tty                            Allocate a pseudo-TTY`
- [x] 2.0 Webapps with Docker
    - `docker ps`
    - `docker ps -a` 
    - `-d`
      - `-d, --detach                         Run container in background and print container ID`  
    - `$ docker run --name static-site -e AUTHOR="Your Name" -d -P dockersamples/static-site` 
    - `-P`
      - **`-P, --publish-all                    Publish all exposed ports to random ports`**
    - `docker run --name static-site-2 -e AUTHOR="Your Name" -d -p 8888:80 dockersamples/static-site`
    - `-p`
      - `-p, --publish list                   Publish a container's port(s) to the host`  
    - Specific version
      - `docker pull ubuntu:12.04`
    - > Base images are images that have no parent images, usually images with an OS like ubuntu, alpine or debian.
      - Official images
      - User images 
    - > Child images are images that build on base images and add additional functionality. 
      - Official images
      - User images 
    - `docker build -t <YOUR_USERNAME>/myfirstapp .`
      - `-t, --tag list                Name and optionally a tag in the 'name:tag' format`
    - `docker run -p 8888:5000 --name myfirstapp YOUR_USERNAME/myfirstapp` 
- [ ] 3.0 Deploying an app to a Swarm 
