# pyACTRECOG
    Version: 1.0.1    
    Author : Md. Nazmuddoha Ansary
                  
![](/info/src_img/python.ico?raw=true )
![](/info/src_img/tensorflow.ico?raw=true)
![](/info/src_img/col.ico?raw=true)

# Version and Requirements
* tensorflow==1.15.0
* numpy==1.16.4        
* Python == 3.6.8
> Create a Virtualenv and *pip3 install -r requirements.txt*

# DataSet 

Dataset is taken from [Hand Action Detection from Depth Sequences](https://web.bii.a-star.edu.sg/archive/machine_learning/Projects/behaviorAnalysis/handAction_ECHT/index.htm)
> Members:Chi Xu,Lakshmi Narasimhan Govindarajan,Li Cheng 

* Get a general idea about the [dataset info](/info/data.md) 
* The **Train1** and **Train2** are combined in one single folder named **Train**
* A random user is chosen for **Eval** Data Within **Train** data
* For **Test** the original source are kept

> Folder tree for the **src_path** for **main.py** in implementation case is as follows:


            ├── Eval
            │   └── LN
            ├── Test
            │   ├── alvin
            │   ├── etienne
            │   └── xiaowei
            └── Train
                ├── aiyang
                ├── ashwin
                ├── biru
                ├── chi
                ├── chris
                ├── gulin
                ├── justin
                ├── lakshmi
                ├── malay
                ├── marc
                ├── michael
                ├── xuchi
                ├── yizhou
                └── yongzhong

            
# Preprocessing

* run **main.py**

            usage: main.py [-h] src_path dest_path

            Hand Action Recognition Preprocessing

            positional arguments:
            src_path    Source Data Folder
            dest_path   Destination Data Folder

            optional arguments:
            -h, --help  show this help message and exit


* The complete preprocessing may take huge time and also cause to crash the system due to high memory useage. A way around is built for **Ubuntu** users is to run **sudo ./clear_mem.sh** in parallel with **main.py**

* After execution, the provided **dest_path** should have a **DataSet** folder with the following folder tree:


            ├── mode:Eval_numOfSeqences:3456_minSeqLen:6.json
            ├── mode:Test_numOfSeqences:21785_minSeqLen:6.json
            ├── mode:Train_numOfSeqences:49920_minSeqLen:6.json
            └── TFRECORD
                ├── Eval
                │   ├── Eval_0.tfrecord
                │   ├── Eval_1.tfrecord
                │   ├── Eval_2.tfrecord
                │   ├── Eval_3.tfrecord
                │   ├── Eval_4.tfrecord
                │   ├── Eval_5.tfrecord
                │   └── Eval_6.tfrecord
                └── Train
                    ├── Train_0.tfrecord
                    ├── Train_10.tfrecord
                    ├── Train_11.tfrecord
                    ├── Train_12.tfrecord
                    ├── Train_13.tfrecord
                    ├── Train_14.tfrecord
                    ├── Train_15.tfrecord
                    ├── Train_16.tfrecord
                    ├── Train_17.tfrecord
                    ├── Train_18.tfrecord
                    ├── Train_19.tfrecord
                    ├── Train_1.tfrecord
                    ├── Train_20.tfrecord
                    ├── Train_21.tfrecord
                    .....................
                    .....................
                    .....................
                    ├── Train_95.tfrecord
                    ├── Train_96.tfrecord
                    ├── Train_97.tfrecord
                    └── Train_9.tfrecord

* 3 directories, 108 files



**ENVIRONMENT**  

    OS          : Ubuntu 18.04.3 LTS (64-bit) Bionic Beaver        
    Memory      : 7.7 GiB  
    Processor   : Intel® Core™ i5-8250U CPU @ 1.60GHz × 8    
    Graphics    : Intel® UHD Graphics 620 (Kabylake GT2)  
    Gnome       : 3.28.2  


# TPU(Tensor Processing Unit)
![](/info/src_img/tpu.ico?raw=true)*TPU’s have been recently added to the Google Colab portfolio making it even more attractive for quick-and-dirty machine learning projects when your own local processing units are just not fast enough. While the **Tesla K80** available in Google Colab delivers respectable **1.87 TFlops** and has **12GB RAM**, the **TPUv2** available from within Google Colab comes with a whopping **180 TFlops**, give or take. It also comes with **64 GB** High Bandwidth Memory **(HBM)**.*
[Visit This For More Info](https://medium.com/@jannik.zuern/using-a-tpu-in-google-colab-54257328d7da)  

#  GCS (Google Cloud Storage)	
![](/info/src_img/bucket.ico?raw=true) Training with tfrecord is not implemented for local implementation in colab.	
For using colab, a **bucket** must be created in **GCS** and connected for:
* tfrecords
* checkpoints (custom training Loop)
# MODELS
## CONVNET3D:
The model is based on the paper [Learning Spatiotemporal Features with 3D Convolutional Networks](https://ieeexplore.ieee.org/document/7410867) 

* **Differences from Original Paper Version**:
1. ***BatchNormalization*** is used after pooling
2. ***Dropouts*** are not used
3. No ***zero padding*** at last conv groups


The adapted model structre is as follow:

![](/info/convNet3D.png?raw=true)

## LRCN:
The model is based on the paper [Long-Term Recurrent Convolutional Networks for Visual Recognition and Description](https://ieeexplore.ieee.org/document/7558228) 

* **Differences from Original Paper Version**:
A Custom **ConvBlock** like structre is used. Also please note that the **BatchNorm** layers are **not time distributed**.

The adapted model structre is as follow:

![](/info/LRCN.png?raw=true)

# TESTING:
* run **score.py**

            usage: score.py [-h] model_name model_path json_path

            Hand Action Recognition Testing

            positional arguments:
            model_name  name of the model. Available:ConvNet3D,LRCN
            model_path  Path for model weights
            json_path   Path for Test Sequence json

            optional arguments:
            -h, --help  show this help message and exit

# SCRIPTS:
* **debug.py**:Local debug script

            usage: debug.py [-h] tfrecord_dir exec_flag

            Hand Action Recognition: debug script

            positional arguments:
            tfrecord_dir  /path/to/tfrecord/
            exec_flag     Check Data or Train a Model. Available flags:
                            CHECK,ConvNet3D,LRCN,DUMMY

            optional arguments:
            -h, --help    show this help message and exit

* **videogen.py**: Video generation script

            usage: videogen.py [-h] json_path max_len

            Hand Action Recognition: Video Labeled Image Generation

            positional arguments:
            json_path   Path for video_sec_<model>.json
            max_len     max number of images used to create one video file

            optional arguments:
            -h, --help  show this help message and exit



# References:
* *Xu, C., Govindarajan, L.N. and Cheng, L., 2017. Hand action detection from ego-centric depth sequences with error-correcting Hough transform. Pattern Recognition, 72, pp.494-503.*
* *Donahue, J., Anne Hendricks, L., Guadarrama, S., Rohrbach, M., Venugopalan, S., Saenko, K. and Darrell, T., 2015. Long-term recurrent convolutional networks for visual recognition and description. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 2625-2634).*
* *Tran, D., Bourdev, L., Fergus, R., Torresani, L. and Paluri, M., 2015. Learning spatiotemporal features with 3d convolutional networks. In Proceedings of the IEEE international conference on computer vision (pp. 4489-4497).*
