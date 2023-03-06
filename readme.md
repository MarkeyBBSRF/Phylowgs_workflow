This repository contains the test data and the command line to run the phylowgs workflow. You can download the entire folder to your local computer to test with the demo data.

## Install docker on Windows
Follow the tutorial `https://www.youtube.com/watch?v=F3wEUL2i5KY&ab_channel=softbikas`, starts at 2:55

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
## Setup docker R environtment
Open your `Windows Powershell` and type in the following commands

Pull docker image
```
docker pull merckey/hotpot:rstudio-archR-gsea
```

Start the local R studio server. 

The `-v` parameter in the code below mount your local computer to the R server so that data can be accessed in R. The parameter contain 2 parts with a colon separating each other `local_dir:server_dir`.
   * `local_dir` is your data folder you'd like to access. For example, if you want to access all the files in your dropbox, you can set it as `C:/Dropbox`.
   * `server_dir` is the path accessbile inside R server. If you set it to `/mnt`. Everything in your dropbox can be accessed through `/mnt` in R server.
   
```
docker run --rm -p 8787:8787 --name rstudio_412 -e PASSWORD=123 -v "C:/your_directory_to_access_in_R_server:/mnt" merckey/hotpot:rstudio-archR-gsea
```


## Setup docker phylowgs environtment

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

Test `phylowgs`. You should see the following output

```
docker run -ti merckey/hotpot:phylowgs python /phylowgs/multievolve.py
usage: multievolve.py [-h] [-n NUM_CHAINS]
                      [-r RANDOM_SEEDS [RANDOM_SEEDS ...]]
                      [-I CHAIN_INCLUSION_FACTOR] [-O OUTPUT_DIR] --ssms
                      SSM_FILE --cnvs CNV_FILE
multievolve.py: error: argument --ssms is required
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

The following command will generate 4 individual trees for the sample `TCGA-YB-A89D` using `multievolve.py` function.
[See phylowgs github](https://github.com/morrislab/phylowgs#running-phylowgs-with-multiple-mcmc-chains-recommended)


```
docker run -v C:\Desktop\demo:/demo merckey/hotpot:phylowgs python /phylowgs/multievolve.py -n 4 -I inf --ssms /demo/phylowgs_input/TCGA-YB-A89D/smm_data.txt --cnvs /demo/phylowgs_input/TCGA-YB-A89D/cnv_data.txt -B 1000 -s 2500 -i 5000 -O /demo/results/TCGA-YB-A89D/

```

Explanation:

  1. `docker run -v demo:/demo` merckey/hotpot:phylowgs will map your local folder `C:\Desktop\demo` to the docker container and so that everythin in the `demo` folder can be accessed within docker under a different path `/demo`. If `demo` folder is under a different path, replace `C:\Desktop\demo` to the correct path on your computer`.
  2. The rest of the command `python /phylowgs/multievolve.py -n 4 -I inf --ssms /demo/phylowgs_input/TCGA-YB-A89D/smm_data.txt --cnvs /demo/phylowgs_input/TCGA-YB-A89D/cnv_data.txt -B 1000 -s 2500 -i 5000 -O /demo/results/TCGA-YB-A89D/` will run the actually phylowgs tree pipeline and generate the output under the `/demo/results` in the concainer folder. You can access this folder locally in `demo/results`.


The detailed explanation for this function:

```
    usage: multievolve.py [-h] [-n NUM_CHAINS]
                          [-r RANDOM_SEEDS [RANDOM_SEEDS ...]]
                          [-I CHAIN_INCLUSION_FACTOR] [-O OUTPUT_DIR] --ssms
                          SSM_FILE --cnvs CNV_FILE

    optional arguments:
      -h, --help            show this help message and exit
      -n NUM_CHAINS, --num-chains NUM_CHAINS
                            Number of chains to run concurrently (default: 4)
      -r RANDOM_SEEDS [RANDOM_SEEDS ...], --random-seeds RANDOM_SEEDS [RANDOM_SEEDS ...]
                            Space-separated random seeds with which to initialize
                            each chain. Specify one for each chain. (default:
                            None)
      -I CHAIN_INCLUSION_FACTOR, --chain-inclusion-factor CHAIN_INCLUSION_FACTOR
                            Factor for determining which chains will be included
                            in the output "merged" folder. Default is 1.5, meaning
                            that the sum of the likelihoods of the trees found in
                            each chain must be greater than 1.5x the maximum of
                            that value across chains. Setting this value = inf
                            includes all chains and setting it = 1 will include
                            only the best chain. (default: 1.5)
      -O OUTPUT_DIR, --output-dir OUTPUT_DIR
                            Directory where results from each chain will be saved.
                            We will create it if it does not exist. (default:
                            chains)
      --ssms SSM_FILE       File listing SSMs (simple somatic mutations, i.e.,
                            single nucleotide variants. For proper format, see
                            README.md. (default: None)
      --cnvs CNV_FILE       File listing CNVs (copy number variations). For proper
                            format, see README.md. (default: None)
```
## Summarize tree

```
mkdir summarize
python /phylowgs/tree_reader_multi_latest.py \
-t results/trees.zip -s TCGA-02-2470/ssm_data.txt -O summarize/TCGA-02-2470  -k 10

```
