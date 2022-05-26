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
- [x] 3.0 Deploying an app to a Swarm 
  - `docker stack deploy --compose-file docker-stack.yml vote`
    ```
    $ docker stack deploy --compose-file docker-stack.yml vote
    Creating network vote_frontend
    Creating network vote_backend
    Creating network vote_default
    Creating service vote_db
    Creating service vote_vote
    Creating service vote_result
    Creating service vote_worker
    Creating service vote_visualizer
    Creating service vote_redis
    ```
  - `docker stack services vote`
    ```
    $ docker stack services vote
    ID             NAME              MODE         REPLICAS   IMAGE                                          PORTS
    irmw9hdow0vf   vote_db           replicated   1/1        postgres:9.4                                   
    f2hr9pi35gi2   vote_redis        replicated   1/1        redis:alpine                                   
    kw71apvahf1t   vote_result       replicated   1/1        dockersamples/examplevotingapp_result:before   *:5001->80/tcp
    fqaosyfrf0pp   vote_visualizer   replicated   1/1        dockersamples/visualizer:stable                *:8080->8080/tcp
    1nxj0dl2i3zv   vote_vote         replicated   2/2        dockersamples/examplevotingapp_vote:before     *:5000->80/tcp
    nka2vp9w6b1c   vote_worker       replicated   1/1        dockersamples/examplevotingapp_worker:latest   
    ```

## Networking concepts

ref: [https://github.com/docker/labs/tree/master/networking](https://github.com/docker/labs/tree/master/networking#networking-concepts)

- [x] Networking Basics
  - `docker network`
  - `docker network ls`
  - `docker network inspect`
    - `docker network inspect docker_gwbridge`     
  - `docker info`
- [ ] Bridge networking
  - `docker network ls`    
  -  > In this example the network and the driver have the same name - but they are not the same thing!
  - `docker run -dt ubuntu sleep infinity`
    ```
    $ docker ps
    CONTAINER ID   IMAGE     COMMAND            CREATED              STATUS              PORTS     NAMES
    b1d56dc7c47a   ubuntu    "sleep infinity"   About a minute ago   Up About a minute             festive_tereshkova
    
    $ docker network inspect bridge
    [
        {
            "Name": "bridge",
            "Id": "538496bd572b349a78cb3b80e5d884a908d69c5a63ad42f407d04373c2cc1f88",
            "Created": "2022-05-20T13:07:31.498789Z",
            "Scope": "local",
            "Driver": "bridge",
            "EnableIPv6": false,
            "IPAM": {
                "Driver": "default",
                "Options": null,
                "Config": [
                    {
                        "Subnet": "172.17.0.0/16",
                        "Gateway": "172.17.0.1"
                    }
                ]
            },
            "Internal": false,
            "Attachable": false,
            "Ingress": false,
            "ConfigFrom": {
                "Network": ""
            },
            "ConfigOnly": false,
            "Containers": {
                "b1d56dc7c47a489cf768de2b5c6490e144aa78a0881e0355836f8685f8189cd3": {  // Ubuntu がここにある
                    "Name": "festive_tereshkova",
                    "EndpointID": "dd67687b21073badc3f2213891f427d94bd63aae0e62a22f028f5863f04dd5ad",
                    "MacAddress": "02:42:ac:11:00:02",
                    "IPv4Address": "172.17.0.2/16",
                    "IPv6Address": ""
                }
            },
            "Options": {
                "com.docker.network.bridge.default_bridge": "true",
                "com.docker.network.bridge.enable_icc": "true",
                "com.docker.network.bridge.enable_ip_masquerade": "true",
                "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
                "com.docker.network.bridge.name": "docker0",
                "com.docker.network.driver.mtu": "1500"
            },
            "Labels": {}
        }
    ]
    ```
  - `docker exec -it 6dd93d6cdc80 /bin/bash` 

## 実践 Docker - ソフトウェアエンジニアの「Docker よくわからない」を終わりにする本

ref: https://zenn.dev/suzuki_hoge/books/2022-03-docker-practice-8ae36c33424b59

- > ubuntu:20.04 は当然 Ubuntu ですが、nginx:latest のようなイメージは一度 bash で起動して OS やパッケージマネージャが何かを調べておくのが基本です。
- ２部: Dockerfile の基礎
  - Dockerfile 作成後、`docker image build --tag xxxxx .`  
  - `RUN apt install emacs` でインストールすると対話できないため、`RUN apt install -y emacs` にする。
  - > 普段目にする Dockerfile は RUN apt update && apt install -y vim のように １つの RUN で複数の Linux コマンドを連続して実行 しているものが大半だと思います。これは RUN がコマンドの結果をレイヤーとして確定する という点に注目すると意図が読み取りやすいです。
  - > 公開されている Dockerfile は 4 レイヤーしか作っていません。
- 3部
  - [work](./work) で演習
  - `docker image build --file=./docker/app/Dockerfile --tag docker-practice:app .`
  - > この本ではいきなり Dockerfile を書きましたが、構築手順が定かではない場合はこの方法は効率が悪い です。「PHP のインストールってこれでいいのかな」「msmtp ってどうやってインストールするんだろう」という状態なら、一度ただベースイメージを起動して自分で bash で試行錯誤する とよいでしょう。
  - `--env`
  - > 接続してから /etc/os-release を見ると Alpine Linux であることがわかるのですが、軽量であることを重視した Alpine Linux をベースとしたイメージには curl はおろか bash も入っていない場合が大半です。
  - > また、sudo もないため --user オプションで root として sh を起動します。
    ```
    $ docker container exec  \
    --interactive        \
    --tty                \
    --user root          \
    mail                 \
    sh
    ```
