# MAX78000-Custom-dataset
If you have own dataset you can label by [roboflow](https://app.roboflow.com/), but if you don't have own datasets? find and dowload [universe.roboflow](https://universe.roboflow.com/search?q=object%20detection&t=metadata)

You can labeling images by roboflow and export dataset YOLO v4 PyTorch. 
![](https://github.com/WeerawatW/MAX78000-prepare-dataset/blob/398a3f7c610fcc7d21005ca44188c650777365d9/roboflow.png)


After exporting, place that file in your directory.

![](custom_data_images/1.png)

In this example we use [bottle_dataset.zip](https://github.com/WeerawatW/MAX78000-Custom-dataset/releases/download/Dataset/custom_data.zip)

Create new folder, i'll rename this folder is `custom_data`

![](custom_data_images/2.png)

Open  .zip file from roboflow, move `test` and `train` folder from into `custom_data` . 

<img src ="https://github.com/WeerawatW/MAX78000-Custom-dataset/blob/bc48ff8dee874af55faf5815098fa650d1d3fd03/custom_data_images/3.png" width = '960' height = '480'>

![](images/extrct_file.png)

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



![](custom_data_images/7.png)

train_info.csv(annotations).

![](custom_data_images/11.png)
