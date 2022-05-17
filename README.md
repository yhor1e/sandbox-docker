# sandbox-docker

## Docker for beginners

ref: https://github.com/docker/labs/blob/master/beginner/readme.md

- [x] 1.0 Running your first container
  - `-it`
    - `-i, --interactive                    Keep STDIN open even if not attached`
    - `-t, --tty                            Allocate a pseudo-TTY`
- [ ] 2.0 Webapps with Docker
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
