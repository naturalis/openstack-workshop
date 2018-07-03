# Install R

This will show the steps of installing R in Ubuntu 16.04
For R we need a extral package repository. We first need to add a key 
to thrust the repo then add the repo. 
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo add-apt-repository 'deb [arch=amd64,i386] https://cran.rstudio.com/bin/linux/ubuntu xenial/'
sudo apt-get update
sudo apt-get install r-base
```

To start R run
```
R
```

