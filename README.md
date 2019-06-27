### 1 - Gathering images
in my case I use microscope images

### 2 - Label images 
using `labelImg` https://github.com/tzutalin/labelImg
using `boundingbox` https://github.com/sang1799/spermboundingbox
unzip labelImg\
run cmd and go to labelImg dir
```
conda install pyqt=5 
pyrcc5 -o resources.py resources.qrc
python labelImg.py
```

### 3 - Installing TensorFlow-GPU
```
pip install tensorflow-gpu 
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

### 6 - Download  my Repository 
https://github.com/sang1799/sperm_morphology_classification.git
unzip folder and copy paste in `C:\tensorflow1\models\research\object_detection`
open cmd
```
cd C:\tensorflow1\models\research\object_detection
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

### 10 - Test Trained models
Test trained model to verfity microscope images and movies of sperm
`(tensorflow1) C:\tensorflow1\models\research\object_detection> idle`

 1) images : open the 'Sperm_detection_image.py'
             eidt MODEL_NAME = indicate download folder for your choidce
			 edit IMAGE_NAME = your sperm image in ..\object_detection
			 edit PATH_TO_LABELS = if you use recongnition model, change to Sperm_recongnition_labelmap.pbtxt by other path
			 run
 2) movies : open the 'Sperm_detection_video'
			 eidt MODEL_NAME = indicate download folder for your choidce
			 edit VIDEO_NAME = your sperm VIDEO in ..\object_detection
			 edit PATH_TO_LABELS = if you use recongnition model, change to Sperm_recongnition_labelmap.pbtxt by other path
			 run