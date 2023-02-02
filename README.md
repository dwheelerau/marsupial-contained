# Marsupial-contained: a docker image of marsupial.ai for aussie-centric camera trap data automation   

## Introduction  
This repo creates a docker image that allows users to run [Marsupial.au](https://github.com/Sydney-Informatics-Hub/marsupial) in a system agnostic way. I have also created a `prediction_batch.py` script that batch processes images contained in a target directory. The script also creates a CSV file of output information and automatically generates images with BBOX information for trouble shooting. Some more information about my marsupial.ai fork can be found [here](https://github.com/dwheelerau/marsupial.git).    

[Marsupial.au](https://github.com/Sydney-Informatics-Hub/marsupial) is a camera trap processing pipeline based on [megadetector v5](https://github.com/microsoft/CameraTraps) that was developed by the [Sydney Uni data hub](https://github.com/Sydney-Informatics-Hub). 

## How is Marsupial.ai different from megadetector  
- animal detection as well as classification for common animals found in Australia
- trained on the wild count dataset from NSW Parks
- authors claim that the latest model can reliably detect 72 species

# Creating the image and running the container
To build this file:  

```
sudo docker build -f Dockerfile . -t dwheelerau/marsupial:ubuntu2004
```

The following Mounts your current host directory in the container directory,
at /project, and runs the `prediction_batch.py` script with demo settings.  

 
```
sudo docker run --gpus all -it -v `pwd`:/project dwheelerau/marsupial:ubuntu2004 /bin/bash -c "cd /project && python /build/marsupial/prediction_batch.py -i /build/marsupial/data -m /build/marsupial/weights/marsupial_72s.pt -o processed_images"
```
