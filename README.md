# Marsupial-contained
A docker image of marsupial.ai for aussie-centric camera trap data automation.

## Introduction  
This repo creates a docker image that allows users to run [Marsupial.au](https://github.com/Sydney-Informatics-Hub/marsupial) in a system agnostic way. I have also created a `prediction_batch.py` script that batch processes images contained in a target directory. The script also creates a CSV file of output information and automatically generates images with BBOX information for trouble shooting. Some more information about my marsupial.ai fork can be found [here](https://github.com/dwheelerau/marsupial.git).    

[Marsupial.au](https://github.com/Sydney-Informatics-Hub/marsupial) is a camera trap processing pipeline based on [megadetector v5](https://github.com/microsoft/CameraTraps) that was developed by the [Sydney Uni data hub](https://github.com/Sydney-Informatics-Hub). 

The current model descriptions from the authors are shown below (these can be specified by `-m /build/marsupial/weights/MODEL_FILENAME.pt`. The `marsupial_72s.pt` is the default model.  

marsupial_detector: This model is a pure animal detector, and simply finds any animal in an image.
marsupial_16s: This model can identify 16 species of animal, with extremely high precision and recall.
marsupial_33s: This model can identify 33 species, with very high precision and recall.
marsupial_41s: This model can identify 41 species, and is a great balance between pure performance and detailed predictions.
marsupial_72s: This model can identify 72 species, and is our most detailed predictor while still delivering stare of the art performance.

If you use this script you should cite the marsupial.au paper as well as Megadetector!The citation for megadetector itself is:  

ToDo: find masupial.ai citation  

```
@article{beery2019efficient,
  title={Efficient Pipeline for Camera Trap Image Review},
  author={Beery, Sara and Morris, Dan and Yang, Siyu},
  journal={arXiv preprint arXiv:1907.06772},
  year={2019}
}

```

## How is Marsupial.ai different from megadetector  
- animal detection as well as classification for common animals found in Australia
- trained on the wild count dataset from NSW Parks
- authors claim that the latest model can reliably detect 72 species

# Quickstart
Pull the container from the docker hub by search for `dwheelerau/marsupial`.  

The following command will create a container and run the `prediction_batch.py` script, generating a `prediction.csv` file of results (bbox information, detection class, detection probabilities), and copies of the images with bboxes added (these are saved in the directory specified by `-o`). The `-i` directory is required (ie the target directory with images you want to process). The other options can be left as defaults are provided.
```
# -o output directory for bbox images
sudo docker run --gpus all -it -v `pwd`:/project dwheelerau/marsupial:ubuntu2004 /bin/bash -c "cd /project && python /build/marsupial/prediction_batch.py -i /build/marsupial/data -m /build/marsupial/weights/marsupial_72s.pt -o processed_images"
```

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
