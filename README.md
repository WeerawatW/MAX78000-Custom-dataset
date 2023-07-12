# MAX78000-Custom-dataset
If you have own dataset you can label by [roboflow](https://app.roboflow.com/), but if you don't have own datasets? find and dowload [universe.roboflow](https://universe.roboflow.com/search?q=object%20detection&t=metadata)

You can labeling images by roboflow and export dataset YOLO v4 PyTorch. 
![](https://github.com/WeerawatW/MAX78000-prepare-dataset/blob/398a3f7c610fcc7d21005ca44188c650777365d9/roboflow.png)

### Note: any--> ***custom_data*** <-- word you can change that if you want. 

After exporting, place that file in your directory.

![](custom_data_images/1.png)

In this example we use [bottle_dataset.zip](https://github.com/WeerawatW/MAX78000-Custom-dataset/releases/download/Dataset/custom_data.zip)

Create new folder, i'll rename this folder is `custom_data`

![](custom_data_images/2.png)

Open  .zip file from roboflow, move `test` and `train` folder from into `custom_data` . 

<img src ="https://github.com/WeerawatW/MAX78000-Custom-dataset/blob/bc48ff8dee874af55faf5815098fa650d1d3fd03/custom_data_images/3.png" width = '960' height = '480'>

![](images/extrct_file.png)


than delete these file `test` and `train`, rename `test_rename` folder -> `test` and `train_rename` folder -> `train` folder.

![](custom_data_images/13.png)

Inside  `test` folder.

![](custom_data_images/4.png)

In `_class.txt` .

![](custom_data_images/8.png)

In `_annotation.txt` .

![](custom_data_images/9.png)

### A step) You can use [custom_dataset_convert_format.py](https://github.com/WeerawatW/MAX78000-Custom-dataset/blob/bc48ff8dee874af55faf5815098fa650d1d3fd03/file_for_github/your_directory/custom_dataset_convert_format.py)

 This script use for 1)convert .txt  to .csv format(shiping label) 2)rename images files. 

 
Note: Change ***mode*** to `test`and change ***path*** to extracted directory, `custom_dataset_convert_format.py` file can convert limit only 2 object in 1 image, if you want to add object more than 2 you can modify `custom_dataset_convert_format.py` code.

![](custom_data_images/5.png)

Then , Run `custom_dataset_convert_format.py` .

![](custom_data_images/6.png)

After run `custom_dataset_convert_format.py` that program generate `test_rename` folder and `test_label/test_info.csv` file.

Result

![](custom_data_images/7.png)

test_rename (images).

![](custom_data_images/12.png)

train_info.csv(annotations).

![](custom_data_images/11.png)


than delete these file `test` and `train`.
![](custom_data_images/13.png)

rename `test_rename` folder -> `test` and `train_rename` folder -> `train` folder.

![](custom_data_images/14.png)

Create `processed` folder

![](custom_data_images/16.png)

Move `train_info.csv` and `test_info.csv` to `processed` folder .

![](custom_data_images/15.png)

### Congratulation!! , now you have  `processed`, `test`, `train` folder.

## 1) ai8x-training
Process datasets 
We use [datasets.zip](https://github.com/WeerawatW/MAX78000-Custom-dataset/releases/download/Dataset/custom_data.zip) you can download it, or you have your own dataset **move** `processed`, `test`, `train` folder into `your data` folder, in this exmaple `your data` is `custom_data` folder.

After the download is complete or you have  `processed`, `test`, `train` move it to `custom_data` folder, I organize the directory according to the following structure:
>
```
ai8x-training
 └─ data
   └─ custom_data
             ├─ processed
             │    ├─ test_info.csv
             │    ├─ train_info.csv
             │
             ├─ test
             └─ train
```
Note: if you did't `data` folder becuse you did't train ai on ai8x-training before,  you must create `data` folder first.

Place `custom_data.py` in dataset folder.
```
ai8x-training
  └─ dataset
        └─ custom_data.py.py  
```

You can download [custom_data.py](file_for_github/ai8x-training/datasets/custom_data.py) and change **custom_data** word if you want.

<img src = 'custom_data_images/18.png' height = 360 width = 800>

<img src = 'custom_data_images/19.png' height = 400 width = 800>

<img src = 'custom_data_images/20.png' height = 400 width = 800>

Note: you should canfig output match number any class you have ,for example you have (1,2,3,4,5) 5 class you should config output (1,2,3,4,5) 5 class.

The fields have the following meanings:
*  `name`: The dataset name, the name passed in when training the neural network
*  `input`: The size of the neural network input, indicates: the width of the input picture is 74, the height is 74, and 3 represents the three channels of RGB(3, 74, 74)
*  `output`: The neural network outputs classification results , in this project we need to identify 6 classes fingers,and there is 6 output class here.
*  `loader`: A function that loads a dataset and needs to return a tuple. Each dataset has to be implemented and magical. Returns the size of the dataset. Returns a training sample in the format of a tuple. For neural networks with images as input, a three-dimensional array representing the input images. For the object detection task, tuples, composed of all callout boxes, each labeled box is, where coordinates are normalized. 
* `collate_fn`: Since each image may have a different number of objects, we need a collate function
        (to be passed to the DataLoader).
        This describes how to combine these tensors of different sizes. We use lists.
        :param batch: an iterable of N sets from __getitem__()
        :return: a tensor of images, lists of varying-size tensors of bounding boxes and labels

You can download [train_custom_data.sh](file_for_github/ai8x-training/train_custom_data.sh) and change dataset too.

![](custom_data_images/21.png)

Place `train_custom_data.sh` in ai8x-training:

```
ai8x-training
  └─ train_custom_data.sh
```

In `train_custom_data.sh`:

```
#!/bin/sh
python train.py --deterministic --print-freq 200  --epochs 100 --optimizer Adam --lr 0.001 --wd 0 --model ai85tinierssd --use-bias --momentum 0.9 --weight-decay 5e-4 --dataset custom_data --device MAX78000 --obj-detection --obj-detection-params parameters/obj_detection_params_svhn.yaml --batch-size 16 --qat-policy policies/qat_policy_svhn.yaml --validation-split 0 "$@"
```


The meaning of each parameter is as follows:
| Parameters | value | describe parameter |
| ------------ |----| ------------------- |
| deterministic |none value| Set the random number seed to produce repeatable training results |
| print-freq | 200 | In each epech, how many training samples are printed once |
| pr-curves |none value| Display the precision-recall curves Display the precision-recall curve |
| epochs | 100 | the number of training times |
| optimizer | Adam | optimizer |
| lr | 0.001 | learning rate learning rate |
| wd | 0 | weight decay |
| model | ai85tinierssd | model selection, the model definition is in the models folder |
| use-bias | none value | use bias |
| momentum | 0.9 | Momentum, a parameter of the Adam optimizer |
| weight-decay | 5e-4 | Weight decay to prevent overfitting |
| dataset | custom_data | dataset name, previously defined in the dataset loading file |
| device | MAX78000 | MCU chip model |
| obj-detection | none value | object detection |
| obj-detection-params | none value | parameters/obj_detection_params_svhn.yaml target recognition training parameters |
| batch-size | 16 | The number of samples passed into the neural network each time |
| qat-policy policies/qat_policy_svhn.yaml| none value| policy for quantization parameters |
| validation-split | 0 | Portion of training dataset to set aside for validation We have an independent validation set, no need to divide from the training set, here is set to 0 |

Open terminal and type this command:
```
$cd ai8x-training
$source venv/bin/activate
$./train_custom_data.sh
```
When you run `train_custom_data.sh`:

<img src = 'custom_data_images/22.png' height = 500 width = 800>

<img src = 'custom_data_images/23.png' height = 300 width = 800>

Wait until your model train success.

<img src = 'custom_data_images/24.png' height = 300 width = 800>

Than go to `lastest_log_dir` folder.

![](custom_data_images/25.png)

The naxt step in this will be used `qat_best.pth.tar` but before use we must rename `qat_best.pth.tar` to `ai85-custom_data-qat8.pth.tar`.

![](custom_data_images/26.png)

## 2) ai8x-synthesis

Place `ai85-custom_data-qat8.pth.tar` in directory according to the following structure.
```
ai8x-synthesis
   └─ trained
        └─ ai85-custom_data-qat8.pth.tar
```
<img src = 'custom_data_images/27.png' height = 600 width = 800>

You can download [quantize_custom_data.sh](https://github.com/WeerawatW/MAX78000-Custom-dataset/blob/d077d73f8e5738c78f55f6f1827b7da1c31208ae/file_for_github/ai8x-synthesis/quantize_custom_data.sh) and place `quantize_custom_data.sh`in directory according to the following structure.

```
ai8x-synthesis
   └─ quantize_custom_data.sh
```

In `quantize_custom_data.sh`:

```
#!/bin/sh
python quantize.py trained/ai85-custom_data-qat8.pth.tar trained/ai85-custom_data-qat8-q.pth.tar --device MAX78000 -v "$@"
```

Note:  `ai85-custom_data-qat8.pth.tar` use in `quantization` step that use for convert to  `ai85-custom_data-qat8-q.pth.tar` .

Dowload [sample_custom_data.npy](file_for_github/ai8x-synthesis/tests/sample_custom_data.npy) and place `sample_custom_data.npy` in directory according to the following structure.

```
ai8x-synthesis
   └─ tests
       └─ sample_custom_data.npy
```
![](custom_data_images/30.png)

Note:  **sample_custom_data.npy** is a fake data(random numpy array) use to test your model. 

Download [custom_data.yaml](https://github.com/WeerawatW/MAX78000-Custom-dataset/blob/d077d73f8e5738c78f55f6f1827b7da1c31208ae/file_for_github/ai8x-synthesis/networks/custom_data.yaml)

Place `custom_data.yaml` in directory according to the following structure.

```
ai8x-synthesis
   └─ network
       └─ custom_data.yaml
```
<img src = 'custom_data_images/32.png' height = 600 width = 800>

My hightlight is dataset name you can change it if you dataset are different name.

<img src = 'custom_data_images/33.png' height = 600 width = 800>

I configuration `layer 16 to layer 19 output_processors 0xffffffff00000000` that depend on `finger_number.py`  output.
| number of classes | output_processors | how many bit of output_processors|
| -------- | ---------------- | -------------------------------- |
| 1 or 2 | 0xff00000000000000 |              8 bit               |
| 3 or 4 | 0xffff000000000000 |             16 bit               |
| 5 or 6 | 0xffffff0000000000 |             24 bit               |
| 7 or 8 | 0xffffffff00000000 |             32 bit               |
| 9 or 10 | 0xfffffffffff00000 |            44 bit               |

Final step!! download [gen-custom_data.sh](https://github.com/WeerawatW/MAX78000-Custom-dataset/blob/d077d73f8e5738c78f55f6f1827b7da1c31208ae/file_for_github/ai8x-synthesis/gen-custom_data.sh)

```
 #!/bin/sh
DEVICE="MAX78000"
TARGET="sdk/Examples/$DEVICE/CNN"
COMMON_ARGS="--device $DEVICE --timer 0 --display-checkpoint --verbose"

python ai8xize.py --test-dir sdk/Examples/MAX78000/CNN --prefix custom_data --checkpoint-file trained/ai85-custom_data-qat8-q.pth.tar --config-file networks/custom_data.yaml --device MAX78000 --compact-data --mexpress --timer 0 --display-checkpoint --verbose --overlap-data --mlator --new-kernel-loader --overwrite --no-unload
```

The meaning of each parameter is as follows:
| parameters | discript parameter |
| ----------- | ----------------- |
| test-dir sdk/Examples/MAX78000/CNN | save directory |
| prefix custom_data| saved folder name |
| checkpoint-file trained/ai85-custom_data-qat8-q.pth.tar | quantization format file |
| networks/custom_data.yaml | network configuration file |
| device MAX78000 | the chip model of the microcontroller |
| compact-data | Use memcpy() to load input data to save code space (also enabled by default) |
| mexpress | use express kernel loading (also enabled by default) |
| timer 0 | use timer to time the inference (default: off, supply timer number) Whether to use timer to record the running time of CNN |
| display-checkpoint | show parsed checkpoint data |
| verbose | verbose output |
| overlap-data | allow output to override input |
| mlator | Use hardware to swap output bytes |
| new-kernel-loader | (also enabled by default) |
| overwrite | overwrite target (if present) |
| no-unload | disable cnn_unload() function |

```
ai8x-synthesis
   └─ gen-custom_data.sh
```

Type the following command:
```
$deactivate
$cd 
$cd ai8x-synthesis
$source venv/bin/activate
$./quantize_custom_data.sh
$./gen-custom_data.sh
```
When you run `quantize_custom_data.sh`:

<img src = 'custom_data_images/28.png' height = 600 width = 800>

From `ai85-custom_data-qat8.pth.tar` convert to  `ai85-custom_data-qat8-q.pth.tar`.

![](custom_data_images/29.png)

When you run `gen-custom_data.sh`:

<img src = 'custom_data_images/35.png' height = 600 width = 800>

After the run `gen-custom_data.sh` is completed, you will get the generated file in the directory `sdk/Examples/MAX78000/CNN/custom_data`

![](custom_data_images/36.png)
