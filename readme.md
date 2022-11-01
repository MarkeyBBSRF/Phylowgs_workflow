## Test docker
Open `Windows Powershell`, the `docker run hello-world` should be successfully executed.

```
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/


$ docker images hello-world
REPOSITORY    TAG       IMAGE ID       SIZE
hello-world   latest    feb5d9fea6a5   13.26kB
```
## Setup docker environtment

Two docker images need to be pulled from the repositiory.
To get the images, run the following command line by line in `Windows Powershell`.

```
docker pull merckey/hotpot:phylowgs

# example output docker pull merckey/hotpot:phylowgs
phylowgs: Pulling from merckey/hotpot
a7344f52cb74: Pull complete
515c9bb51536: Pull complete
e1eabe0537eb: Pull complete
4701f1215c13: Pull complete
39817c3fcdab: Pull complete
8d338aebcac2: Pull complete
eddc1224ced9: Pull complete
e306a0db5890: Pull complete
732291ef67c6: Pull complete
810845d63e68: Pull complete
3dbfba135fa3: Pull complete
434e9ea99c47: Pull complete
6cbfb4cc1f53: Pull complete
Digest: sha256:8cf96105312080418e8de295357703cca9862fe1c5db54017defcb3db8ada3c0
Status: Downloaded newer image for merckey/hotpot:phylowgs
docker.io/merckey/hotpot:phylowgs
```

Similarly,
```
docker pull merckey/hotpot:phylowgs_stats-v2
```

## Prepare Phylowgs Input
The input for phylowgs analysis should has the following structure as shown in the [demo/phylowgs_input/](demo/phylowgs_input/).

For each sample, it should have the following structure
```
TCGA-YB-A89D/
  -- smm_data.txt
  -- cnv_data.txt
```

## Generate Phylowgs tree

```

```
