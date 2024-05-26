## Лабораторная работа 8 ##

1. Создаём Dockerfile

`````sh
cat >> Dockerfile <<EOF
FROM ubuntu:18.04

RUN apt update
RUN apt install -yy gcc g++ cmake

COPY . print/
WORKDIR print

RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
RUN cmake --build _build

ENV LOG_PATH /home/logs/log.txt

VOLUME /home/logs

WORKDIR _install/bin

ENTRYPOINT ./demo
EOF
`````

2. Создаём main.yml


`````sh
name: CMake

on:
 push:
  branches: [master]
 pull_request:
  branches: [master]

jobs: 
  build:
    runs-on: ubuntu-latest
  
    steps:
    - uses: actions/checkout@v3
  
    - name: Dockerki
      run: sudo apt install runc containerd docker.io
  
    - name: RunDocke
      run: sudo docker build -t logger .
`````

3. Workflow сработал, теперь проверим на локальном репозитории.





`````sh
sudo docker build -t logger .
sudo docker images
sudo docker inspect logger
`````

`````sh
Sending build context to Docker daemon  1.946MB
Step 1/11 : FROM ubuntu:18.04
 ---> f9a80a55f492
Step 2/11 : RUN apt update
 ---> Using cache
 ---> be9679507b62
Step 3/11 : RUN apt install -yy gcc g++ cmake
 ---> Using cache
 ---> 14da1be89fa2
Step 4/11 : COPY . print/
 ---> 427206142a9d
Step 5/11 : WORKDIR print
 ---> Running in 33796e190199
Removing intermediate container 33796e190199
 ---> 28a54a279e1e
Step 6/11 : RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
 ---> Running in 5f7e0509caaf
-- The C compiler identification is GNU 7.5.0
-- The CXX compiler identification is GNU 7.5.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /print/_build
Removing intermediate container 5f7e0509caaf
 ---> d0f456f4796e
Step 7/11 : RUN cmake --build _build
 ---> Running in 2a62163a9da7
Scanning dependencies of target solver_lib
[ 12%] Building CXX object solver_application/CMakeFiles/solver_lib.dir/__/solver_lib/solver.cpp.o
[ 25%] Linking CXX static library libsolver_lib.a
[ 25%] Built target solver_lib
Scanning dependencies of target formatter_ex_lib
[ 37%] Building CXX object solver_application/CMakeFiles/formatter_ex_lib.dir/__/formatter_ex_lib/formatter_ex.cpp.o
[ 50%] Linking CXX static library libformatter_ex_lib.a
[ 50%] Built target formatter_ex_lib
Scanning dependencies of target formatter_lib
[ 62%] Building CXX object solver_application/CMakeFiles/formatter_lib.dir/__/formatter_lib/formatter.cpp.o
[ 75%] Linking CXX static library libformatter_lib.a
[ 75%] Built target formatter_lib
Scanning dependencies of target solver
[ 87%] Building CXX object solver_application/CMakeFiles/solver.dir/equation.cpp.o
[100%] Linking CXX executable solver
[100%] Built target solver
Removing intermediate container 2a62163a9da7
 ---> f101d5ea936a
Step 8/11 : ENV LOG_PATH /home/logs/log.txt
 ---> Running in 3e069b6b1786
Removing intermediate container 3e069b6b1786
 ---> 4bedaf44d57b
Step 9/11 : VOLUME /home/logs
 ---> Running in 61598f70dbac
Removing intermediate container 61598f70dbac
 ---> 28b4d8eba0c7
Step 10/11 : WORKDIR _install/bin
 ---> Running in a5e384790b63
Removing intermediate container a5e384790b63
 ---> 7aed637e8921
Step 11/11 : ENTRYPOINT ./demo
 ---> Running in d6c8647cfc72
Removing intermediate container d6c8647cfc72
 ---> 5c3f66a5b312
Successfully built 5c3f66a5b312
Successfully tagged logger:latest

sudo docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
logger       latest    5c3f66a5b312   4 seconds ago    335MB

sudo docker inspect logger

[
    {
        "Id": "sha256:5c3f66a5b312d0e3ce48223fee7d8f3d582d80c16a9e8de5d1166cf583ee0a89",
        "RepoTags": [
            "logger:latest"
        ],
        "RepoDigests": [],
        "Parent": "sha256:7aed637e8921c1eeef52849cc17afb572768b8844c4c69bed2aa5c058b18be7b",
        "Comment": "",
        "Created": "2024-05-24T16:59:29.712769177Z",
        "Container": "d6c8647cfc72a4a308eb01bb71acc579df53032c842a9e4d220b825cc46c2e9e",
        "ContainerConfig": {
            "Hostname": "d6c8647cfc72",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "LOG_PATH=/home/logs/log.txt"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "ENTRYPOINT [\"/bin/sh\" \"-c\" \"./demo\"]"
            ],
            "Image": "sha256:7aed637e8921c1eeef52849cc17afb572768b8844c4c69bed2aa5c058b18be7b",
            "Volumes": {
                "/home/logs": {}
            },
            "WorkingDir": "/print/_install/bin",
            "Entrypoint": [
                "/bin/sh",
                "-c",
                "./demo"
            ],
            "OnBuild": null,
            "Labels": {
                "org.opencontainers.image.ref.name": "ubuntu",
                "org.opencontainers.image.version": "18.04"
            }
        },
        "DockerVersion": "20.10.25+dfsg1",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "LOG_PATH=/home/logs/log.txt"
            ],
            "Cmd": null,
            "Image": "sha256:7aed637e8921c1eeef52849cc17afb572768b8844c4c69bed2aa5c058b18be7b",
            "Volumes": {
                "/home/logs": {}
            },
            "WorkingDir": "/print/_install/bin",
            "Entrypoint": [
                "/bin/sh",
                "-c",
                "./demo"
            ],
            "OnBuild": null,
            "Labels": {
                "org.opencontainers.image.ref.name": "ubuntu",
                "org.opencontainers.image.version": "18.04"
            }
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 335299416,
        "VirtualSize": 335299416,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/94f688343ed4a469181d449f02c8a84990d2e850d6f95da16d5b823aaba5b401/diff:/var/lib/docker/overlay2/a661527495fe786234a75e13446cfdc86b2d4150eca2b1e0111532175a039380/diff:/var/lib/docker/overlay2/b445d3b52aa7272bc6dc839fde3f5dd21140d2293f82796abd09213cd0460f4b/diff:/var/lib/docker/overlay2/272afa9de8c2301ee8da9c611baba26ea774471482fc3adc8341396c7d581c31/diff:/var/lib/docker/overlay2/0583f880498f34dd202f483466117f8222ce538420903beee225c1ef8b424fe2/diff:/var/lib/docker/overlay2/a52c505532d9110f1fe3c8925a23dd3bca58ad1060983b9bd53a587353af1086/diff",
                "MergedDir": "/var/lib/docker/overlay2/ae4d9da6c5add6e1cda05b13d7ee016ebd20789a1687420b89a168f7dcb99d3c/merged",
                "UpperDir": "/var/lib/docker/overlay2/ae4d9da6c5add6e1cda05b13d7ee016ebd20789a1687420b89a168f7dcb99d3c/diff",
                "WorkDir": "/var/lib/docker/overlay2/ae4d9da6c5add6e1cda05b13d7ee016ebd20789a1687420b89a168f7dcb99d3c/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:548a79621a426b4eb077c926eabac5a8620c454fb230640253e1b44dc7dd7562",
                "sha256:779155f89a8189397fe30f1523cb430a8584f2a4f157da22a10048c4096b31d1",
                "sha256:b2ea22a64f3bedca0e9472430af5d2afe138d363140fde923c4e7a6b2cd1b9d9",
                "sha256:c90a8f675575f8aea3ac96ccfce8b776c76532a4ba52bda7c60fd19a20993566",
                "sha256:bfea50548890cf262efad7adaa973c9d2d89445bd54acb6cc358934c2ab95a6b",
                "sha256:83c9da85d445559d8ba509d6aabc73a736b6b6f0873c60737e38bd2a0343f3e7",
                "sha256:d94a5077cd333d0b6d53d9627d77cb51e40aa4de8967648646f392d5a603a663"
            ]
        },
        "Metadata": {
            "LastTagTime": "2024-05-24T12:59:29.746594272-04:00"
        }
    }
]

`````

