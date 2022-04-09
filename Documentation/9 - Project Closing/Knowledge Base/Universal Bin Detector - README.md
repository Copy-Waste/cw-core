## Installation
clone the yolov5 repository into the bin detector directory.

```bash
    git clone https://github.com/proboticsinc/prairie-robotics-yolov5-fork.git
```
After installing yolo, install the other dependencies using:

```bash
    pip install -r requirements.txt
```
## Preparing Data / Annotations
The original dataset was created using the binary classification data and False Positives collected from Streamsight streetview images, annotated using bounding boxes and contained the following classes: 'BLUE_BIN_CLOSE', 'BLACK_BIN_CLOSE', 'BLUE_BIN_FAR', 'BLACK_BIN_FAR','PORTA_POTTY', 'BUILDING', 'VEHICLE_BLUE', 'BLUE_BARREL_FAR', 'OMITTED']

The current dataset is based on a subset of the afformentioned classes. BLUE_BIN_CLOSE and BLUE_BIN_FAR were combined into a single classe, BLUE_BIN. The same was done for BLACK_BIN. The PORTA_POTTY class remained unchanged and continues to be included in the dataset, the remaining classes were excluded. Additional images of bins have been periodically added to the dataset over time.

### Converting from SuperAnnotate to Yolo Format
Images annotated with SuperAnnotate need to be converted from SuperAnnotate's proprietary format or Coco format to Yolo Format (Darknet). 

Use the `file sa_to_txt.py` to convert your annotation file from SuperAnnotate to Darknet format. 

### Organizing your data

Step 1: Use `DataSplitter.py` to separate the fused images and raw images into separate folders. The fused images are included in the SuperAnnotate download, however are not needed for Yolo training.

Step 2: Use the `script sa_to_txt.py` to convert from SuperAnnotate format to the Darknet text format. The script `removeClasses.py` can be used to strip unwanted classes out of an existing dataset and produce a dataset in Darknet format. Notice that Darknet format uses integer values to represent classes. The names of the classes are stored separately.

Step 3: Use the autosplit function in `Yolo/utils/datasets.py` or `createvalidation.py` to generate the required `test/validation/train annotation files`. 

Step 4: Organize your dataset folder into the preferred YOLO format. The dataset directory should have the following structure:
```Dataset Folder:
    |
    ├── Images Folder
    |   ├── image.png
    |   └── ...
    |
    ├── Labels
    |   ├── image_name.txt
    |   └── ...
    |
    ├── test.txt
    ├── val.txt
    └── train.txt
```
## Training a New Model
    
### Yaml File
Inside the Yolo project folder `Yolov5/data/` create a .yaml file with a descriptive name. You can look at the example files in the project, there are a few things to customize. Set the path value to your dataset root directory. Next set the filenames for the test, validation and training text files that were created using the autosplit function. The Yolov5 script will look in the root of your dataset folder, so you only need to set the corresponding filename, not the complete path. 

Next, set "nc" which is number of classes, to however many classes are present in your dataset. Finally set the "names" dictionary to contain the string values that represent the object classes.

### Train.py
Run the commands as specified by Ultralytics, replacing filenames where needed:

```bash
$ python train.py --data dataset.yaml --cfg yolov5s.yaml --weights '' --batch-size 64
```

## Evaluating Model Performance

### Extract frames from a video clip
Run the following command with the flags:
```
--weights custom model
--source video file
--name output directory
```

```
detect_with_logic.py --weights best.pt --source video.mov --name output
```

### Statistics
Run the file `service_event_level_statistics.py`

The script will plot inference levels against bin detections to show how many service events are generated with each confidence level. 