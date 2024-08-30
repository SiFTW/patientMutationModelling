
![From mutations to patients](screenshots/banner.jpg)

# Patient-specific modelling using mutational profiles

[![GitHub release (latest by date including pre-releases)](https://img.shields.io/github/v/release/navendu-pottekkat/awesome-readme?include_prereleases)](https://img.shields.io/github/v/release/navendu-pottekkat/awesome-readme?include_prereleases)
[![GitHub last commit](https://img.shields.io/github/last-commit/navendu-pottekkat/awesome-readme)](https://img.shields.io/github/last-commit/navendu-pottekkat/awesome-readme)
[![License](https://img.shields.io/badge/license-CC_BY_NC_SA-blue?link=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-nc-sa%2F4.0%2F)](https://img.shields.io/badge/license-CC_BY_NC_SA-blue?link=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-nc-sa%2F4.0%2F)

The goal of this repository is to provide the quickest way to generate patient-specific modelling preditions as published in [Norris <i>et al.</i> 2024](https://mitchell.science/publication/patient-specific-computational-models-predict-prognosis-in-b-cell-lymphoma-by-quantifying-pro-proliferative-and-anti-apoptotic-signatures-from-genetic-sequencing-data/).

This should let you load in new datasets and ask new questions either in B-cell lymphoma or new diseases.
### Installation

This guide is only tested on Ubuntu but likely works on any unix-like operating system. 

1. **Get an OncoKB API access token.** 

    You will need to request an OncoKB API key in order to filter mutations by whether or not they are "likely oncogenic". You can do [that here](https://www.oncokb.org/api-access). Once you have the API token make a note of it because you will need it later.


> [!Note] 
> 
> Run steps 2-5 on the computational server you want to run the model from. Ideally a computer with lots of cores and RAM. The remaining steps can be run from any computer with network access to the web-interface (Jupyter Lab) that will be launched when we run the appropriate Docker image. 


2. **Create the right folders on your server**.

    Make a directory to work in, and a directory for model outputs to be saved in.
   ```sh
   mkdir patientModel
   cd patientModel
   mkdir outputs
   ```

3. **Clone this repo into your new folders.**
   ```sh
   git clone https://github.com/SiFTW/patientMutationModelling.git
   ```

4. Pull down the docker image you need, which has the model code included, and all Julia packages fixed at the version used in the publication.
   ```sh
   sudo docker pull siftw/norrisetal
   ```

5. Run the Docker image, mounting the code you've just downloaded.

   ```sh 
    sudo docker run --rm --group-add users -p 10005:8888 -e JUPYTER_ENABLE_LAB=yes -e JULIA_NUM_THREADS=64 -e NB_UID=1000 -e NB_USER=richard -e JUPYTER_TOKEN=letmein -v ~/patientModel/outputs:/multiscaleModel/SSDoutputs -v ~/patientModel/patientMutationModelling:/multiscaleModel/patientMutationModelling -e CHOWN_HOME=yes --user root -w /home/richard -e CHOWN_EXTRA_OPTS='-R' -w /multiscaleModel/ --user root -e CHOWN_EXTRA='/multiscaleModel/*,/multiscaleModel/' siftw/norrisetal:latest
    ```

   You may want to update the port above to an available one, and you may wish to uploade ```JULIA_NUM_THREADS``` to reflect the number of cores you wish Julia to be able to use.

6. Now you can switch to any computer/laptop that has access to the server (or continue on the server if you prefer). 

    Open a web browser and enter the address and port of the Jupyter Lab instance that was launched by the running the Docker image.

    ```http://[IP address of server]:10005```

   Enter the jupyter login token you set above ("letmein" by default) into the login screen and you will load into Jupyter Lab.

![Alt text](screenshots/screenshot1.png?raw=true "copy screenshot")
  

# License
[(Back to top)](#table-of-contents)

[Attribution-NonCommercial-ShareAlike 4.0 International](./LICENSE.txt)


