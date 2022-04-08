# Copy Waste

This directory contains all the code pertaining to the Copy Waste Data Augmentation Pipeline.

## Installation
To install all the necessary packages for usage run the following command in the base directory:
```
pip install -r requirements.txt
```

## Usage
Before running main.py there we need to set up a data directory. To do this:
* Create a new directory named data
* In this data directory create another directory named s3
* Place the directory containing artificial images directly into the data directory
* Place the annotations.json for all the original images in data/s3
* Set up your environment variables 

To run the Copy Waste pipeline enter the following:
```
python main.py
```

### PyTests
In order to run the pytests for copy-waste run the following command from the copy-waste directory:
```
pytest ./tests
```

## Environment Variables
| Variable                   | Description                                                                                                                                               |
|----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| REGINA_IMPORT_BUCKET       | Bucket containing service event images in Regina.                                                                                                         |
| FORTSTJOHN_IMPORT_BUCKET   | Bucket containing service event images in Fort St. John.                                                                                                  |
| AUGMENTED_EXPORT_BUCKET    | The name of the bucket to send augmented images to.                                                                                                       |
| LOCAL                      | Setting this to true will output the augmented dataset to a local directory. Setting this to false wil upload the results to the AUGMENTED_EXPORT_BUCKET. |
| DATA_DIR                   | Parent directory for all data which will be used.                                                                                                         |
| ORIGINAL_IMAGES            | Directory containing the original images.                                                                                                                 |
| ORIGINAL_ANNOTATIONS       | Path for the annotations.json file for the original images.                                                                                               |
| ARTIFICIAL_IMAGES          | Directory containing the artificial images.                                                                                                               |
| ARTIFICIAL_ANNOTATIONS     | Path for the annotations.json file for the artificial images.                                                                                             |
| OUTPUT_DIR                 | The parent directory where the augmented dataset will be placed.                                                                                          |
| AUGMENTED_ANNOTATIONS_PATH | The path for the annotations.json file for the augmented images.                                                                                          |
| AUDITED_ANNOTATIONS        | Path for the audited annotations.json file.                                                                                                               |

## Potential Errors
There are a number of potential errors which can happen when augmenting data. These are handled and error logs can be found in a csv file under each run directory found in OUTPUT_DIR from your .env file. These potential errors include:

| Error                          | Description                                                                                                  |
|--------------------------------|--------------------------------------------------------------------------------------------------------------|
| Error loading image            | This can happen when a file is not in the path provided by the annotations.json file.                        |
| Error loading artificial image | This can happen when a file is not in the path provided by the annotations.json file.                        |
| Error extracting masks         | Error occurs while clipping or extracting polygon masks.                                                     |
| Failed building image          | This can happen because images are of different sizes.                                                       |
| Multiple contours              | Happens when the objects fail the check_for_multiple_contours test, indicative of incorrect transformations. |

## Acknowledgements
This project is inspired by the paper [Simple Copy-Paste is a Strong Data Augmentation Method for Instance Segmentation](https://arxiv.org/abs/2012.07177) and its unofficial implementation found [here](https://github.com/conradry/copy-paste-aug).