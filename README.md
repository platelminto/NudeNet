# NudeNet: Neural Nets for Nudity Classification, Detection and selective censoring

[![DOI](https://zenodo.org/badge/173154449.svg)](https://zenodo.org/badge/latestdoi/173154449) ![Upload Python package](https://github.com/notAI-tech/NudeNet/actions/workflows/python-publish.yml/badge.svg)


## Fork differences:

- Only the default classifier is available.
- The classifier no longer throws the `Initializer block1_conv1_bn/keras_learning_phase:0 appears in graph inputs 
  and will not be treated as constant value/weight. etc.` warning.
- It only works on images.
- The classifier is included in the project itself.
- Only the v2 model is available (the original repo's default). So `v2` from original is `main` here.

Uncensored version of the following image can be found at https://i.imgur.com/rga6845.jpg (NSFW)

![](https://i.imgur.com/0KPJbl9.jpg)

**Classifier classes:**
|class name   |  Description    |
|--------|:--------------:
|safe | Image is not sexually explicit     |
|unsafe | Image is sexually explicit|


# As self-hostable API service
```bash
# Classifier
docker run -it -p8080:8080 notaitech/nudenet:classifier

# See fastDeploy-file_client.py for running predictions via fastDeploy's REST endpoints 
wget https://raw.githubusercontent.com/notAI-tech/fastDeploy/master/cli/fastDeploy-file_client.py
# Single input
python fastDeploy-file_client.py --file PATH_TO_YOUR_IMAGE

# Client side batching
python fastDeploy-file_client.py --dir PATH_TO_FOLDER --ext jpg
```

**Note: golang example https://github.com/notAI-tech/NudeNet/issues/63#issuecomment-729555360**, thanks to [Preetham Kamidi](https://github.com/preetham)


# As Python module
**Installation**:
```bash
pip install -U git+https://github.com/platelminto/NudeNet
```

**Classifier Usage**:
```python
# Import module
from nudenet import NudeClassifier

# initialize classifier (downloads the checkpoint file automatically the first time)
classifier = NudeClassifier()

# Classify single image
classifier.classify('path_to_image_1')
# Returns {'path_to_image_1': {'safe': PROBABILITY, 'unsafe': PROBABILITY}}
# Classify multiple images (batch prediction)
# batch_size is optional; defaults to 4
classifier.classify(['path_to_image_1', 'path_to_image_2'], batch_size=BATCH_SIZE)
# Returns {'path_to_image_1': {'safe': PROBABILITY, 'unsafe': PROBABILITY},
#          'path_to_image_2': {'safe': PROBABILITY, 'unsafe': PROBABILITY}}
```

# Notes:
- The current version of NudeDetector is trained on 160,000 entirely auto-labelled (using classification heat maps and 
  various other hybrid techniques) images. 
- The entire data for the classifier is available at https://archive.org/details/NudeNet_classifier_dataset_v1
- A part of the auto-labelled data (Images are from the classifier dataset above) used to train the base Detector is available at https://github.com/notAI-tech/NudeNet/releases/download/v0/DETECTOR_AUTO_GENERATED_DATA.zip
