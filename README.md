### 1 - Gathering images
in my case I use microscope images

### 2 - Label images 
using `labelImg` https://github.com/tzutalin/labelImg
or using `boundingbox` https://github.com/sang1799/spermboundingbox
unzip labelImg\
run cmd and go to labelImg dir
```
conda install pyqt=5 
pyrcc5 -o resources.py resources.qrc
python labelImg.py

  ```
### 3 - Installing TensorFlow-GPU

```pip install tensorflow-gpu 
pip install --upgrade tensorflow-gpu
```

### 4 - Creat virtual environment 

```
conda create -n tensorflow1 
activate tensorflow1 
pip install --ignore-installed --upgrade tensorflow-gpu

```
  other necessary packages
```
(tensorflow1) C:\> conda install -c anaconda protobuf 
(tensorflow1) C:\> pip install pillow 
(tensorflow1) C:\> pip install lxml 
(tensorflow1) C:\> pip install Cython 
(tensorflow1) C:\> pip install jupyter 
(tensorflow1) C:\> pip install matplotlib 
(tensorflow1) C:\> pip install pandas 
(tensorflow1) C:\> pip install opencv-python 
```
### 5 - Download the full TensorFlow object detection repository
https://github.com/tensorflow/models.git


### 6 - Download  `faster_rcnn_inception_v2_coco`
https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md

### 7 - Download  my Repository 
https://github.com/sang1799/sperm_morphology_classification.git
unzip folder and copy paste in `C:\tensorflow1\models\research\object_detection`
open cmd
```
cd C:\tensorflow1\models\research\object_detection
mkdir images
mkdir inference_graph
mkdir training
```

### 8 - Configure environment variable

Configure PYTHONPATH environment variable

PYTHONPATH variable must be created that points to the directories
\models \
\models\research \
\models\research\slim  
##### NOTE : every time you run your project must add this lines
```
set PYTHONPATH=C:\tensorflow1\models;C:\tensorflow1\models\research;C:\tensorflow1\models\research\slim
echo %PYTHONPATH%
set PATH=%PATH%;PYTHONPATH
echo %PATH%
```
### 9 - Compile Protobufs
Protobuf (Protocol Buffers) libraries must be compiled , it used by TensorFlow to configure model and training parameters
Open Anaconda Prompt and go to `C:\tensorflow1\models\research`
```
protoc --python_out=. .\object_detection\protos\anchor_generator.proto .\object_detection\protos\argmax_matcher.proto .\object_detection\protos\bipartite_matcher.proto .\object_detection\protos\box_coder.proto .\object_detection\protos\box_predictor.proto .\object_detection\protos\eval.proto .\object_detection\protos\faster_rcnn.proto .\object_detection\protos\faster_rcnn_box_coder.proto .\object_detection\protos\grid_anchor_generator.proto .\object_detection\protos\hyperparams.proto .\object_detection\protos\image_resizer.proto .\object_detection\protos\input_reader.proto .\object_detection\protos\losses.proto .\object_detection\protos\matcher.proto .\object_detection\protos\mean_stddev_box_coder.proto .\object_detection\protos\model.proto .\object_detection\protos\optimizer.proto .\object_detection\protos\pipeline.proto .\object_detection\protos\post_processing.proto .\object_detection\protos\preprocessor.proto .\object_detection\protos\region_similarity_calculator.proto .\object_detection\protos\square_box_coder.proto .\object_detection\protos\ssd.proto .\object_detection\protos\ssd_anchor_generator.proto .\object_detection\protos\string_int_label_map.proto .\object_detection\protos\train.proto .\object_detection\protos\keypoint_box_coder.proto .\object_detection\protos\multiscale_anchor_generator.proto .\object_detection\protos\graph_rewriter.proto
```
```
(tensorflow1) C:\tensorflow1\models\research> python setup.py build
(tensorflow1) C:\tensorflow1\models\research> python setup.py install
```
### 10 - Test TensorFlow setup
Test TensorFlow setup to verify it works
`(tensorflow1) C:\tensorflow1\models\research\object_detection> jupyter notebook object_detection_tutorial.ipynb`

### 11 - Generate Training Data
TFRecords is an input data to the TensorFlow training model
creat `.csv` files from `.xml` files 
```
cd C:\tensorflow1\models\research\object_detection
python xml_to_csv.py
```
This creates a `train_labels.csv` and `test_labels.csv` file in the `\object_detection\images` folder.
```
python generate_tfrecord.py --csv_input=images\train_labels.csv --image_dir=images\train --output_path=train.record
python generate_tfrecord.py --csv_input=images\test_labels.csv --image_dir=images\test --output_path=test.record
```
### 12 - Edit `generate_tfrecord.py`
edit edit `generate_tfrecord.py` and put your classes names

### 13 - Create a label map and edit the training configuration file.
go to `\data `
copy `pet_label_map.pbtxt` to `\training` dir and rename it to  `labelmap.pbtxt`

edit it  to your class `sperm_classification` 

### 14 - Configure object detection tranning pipeline
`cd C:\tensorflow1\models\research\object_detection\samples\configs`
copy `faster_rcnn_inception_v2_pets.config`
past it in  `\training` dir and edit it 


#### a - 
 In the `model` section change `num_classes` to number of different classes

#### b - 
 fine_tune_checkpoint : `C:/tensorflow1/models/research/object_detection/faster_rcnn_inception_v2_coco_2018_01_28/model.ckpt`

#### c - 
In the `train_input_reader` section change `input_path` and `label_map_path` as : <br/>
Input_path : `C:/tensorflow1/models/research/object_detection/train.record` <br/>
Label_map_path: `C:/tensorflow1/models/research/object_detection/training/labelmap.pbtxt`

#### d - 
In the `eval_config` section change `num_examples` as : <br/>
Num_examples = number of  files in   `\images\test` directory.

#### e -
In the `eval_input_reader` section change `input_path` and `label_map_path` as :<br/>
Input_path : `C:/tensorflow1/models/research/object_detection/test.record` <br/>
Label_map_path: `C:/tensorflow1/models/research/object_detection/training/labelmap.pbtxt`

### 18 - Run the Training
```
python train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/faster_rcnn_inception_v2_pets.config
```

### 19 - Tensorboard 
in cmd type `(tensorflow1) C:\tensorflow1\models\research\object_detection>tensorboard --logdir=training`


### 20 - Export Inference Graph
 training is complete ,the last step is to generate the frozen inference graph (.pb file)
change “XXXX” in “model.ckpt-XXXX” should be replaced with the highest-numbered .ckpt file in the training folder:

```
python export_inference_graph.py --input_type image_tensor --pipeline_config_path training/faster_rcnn_inception_v2_pets.config --trained_checkpoint_prefix training/model.ckpt-XXXX --output_directory inference_graph
```
