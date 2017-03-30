# CB1-mice-Pergola-Reproduce.nf

This repository contains the software, scripts and data to reproduce the results corresponding to the CB1 mice experiment of the Pergola paper.

If you have not install yet [docker](https://www.docker.com/) and [nextflow](https://www.nextflow.io/), follow this [intructions](../README.md)

## Data processing

### Pull docker image
Pull the Docker image use for processing data with Pergola (Pergola and its dependencies installed)

```
docker pull pergola/pergola@sha256:829984c62ac8d8d129240f3db3e5de26be2379c4a6c76d871381101ad122569a
```


### Clone the repository

```
git clone --recursive https://github.com/JoseEspinosa/pergola-paper-reproduce.git
cd pergola-paper-reproduce/cb1_mice
```

### Data

Data is publicly available in [Zenodo](https://zenodo.org/) as a compressed tarball.

Data can be downloaded and uncompressed using the following command:

```
mkdir data
wget -O- https://zenodo.org/record/398779/files/CB1_mice.tar.gz | tar xz -C data
```

### Run nextflow pipeline
Once data is downloaded, it is possible to reproduce all the results using this command:

```
NXF_VER=0.24.1 ./CB1_mice-Pergola-Reproduce.nf --recordings='data/mice_recordings/*.csv' --mappings='data/mappings/b2p.txt' -with-docker
```

## Online visualization

### Downloading shiny-pergola config file

Download the configuration files assigning files to each of the group (wt_food_sc, wt_food_fat, cb1_food_sc, cb1_food_fat)

```
wget -O- https://gist.githubusercontent.com/JoseEspinosa/9e65d54d765d9e5af554d837b3427569/raw/b686558fcd076dcc5fb711553203f5fa5f133bf0/cb1_pergola_conf.txt > exp_info.txt
```

### Downloading and running the shiny-pergola image

Pull the docker image containing the version of shiny-pergola web application used for render the data visualization:

```
docker pull pergola/shiny-pergola@sha256:1c0928aae6950fb25c8521dd71df75a42170ce1c61a72b4dec42d52e7797ec41
```

Go to the folder containing the downloaded data and run the image:

```
docker run --rm -p 80:80 -v "$(pwd)":/pergola_data  pergola/shiny-pergola@sha256:1c0928aae6950fb25c8521dd71df75a42170ce1c61a72b4dec42d52e7797ec41 &
```

**Note**: `"$(pwd)"` can be substitute by your absolute path to the folder where the files were downloaded.
**Note**: Figure has several snapshots if you want to get exactly the exact the same figure just select it by setting on id.txt file. 
For instance if you want to reproduce figure X a you just have to type the following command before running Docker shiny-pergola image.

```
echo "cb1_a" > id.txt
```

Go to your web browser and type in your address bar the ip address returned by the following command e.g. http://192.168.99.100

```
docker-machine ip default
```

Et voila, the running container will load the shiny app in your browser.

TODO: Add image of the browser 


