# Custom YOLOv3 pytorch for PPE detection.

## Check this youtube link for video sample result [Youtube link](https://youtu.be/Grr8teUPwnk)

# Training the custom PPE dataset on YOLOv3
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1LbKkQf4hbIuiUHunLlvY-cc0d_sNcAgS)
![image](https://github.com/millermuttu/TSAI-EVA5/blob/master/week13/YOLOv3_custom/output/Atest_Moment.jpg)

## Steps

We have added a small dataset for PPE detection dataset in the folder called customdataset.
Download the full dataset from [Google drive](https://drive.google.com/drive/folders/1BuZBogl7zHGSwOduQatf740bhxty5Sb2?usp=sharing)
This downloadable dataset will have 3000+ images and labels labeled using annotation tool given in the repo

Full credit goes to [this](https://github.com/ultralytics/yolov3), and if you are looking for much more detailed explainiation and features, please refer to the original [source](https://github.com/ultralytics/yolov3). 

You'll need to download the weights from the original source. 
1. Create a folder called weights in the root (YoloV3) folder
2. Download from: https://drive.google.com/open?id=1LezFG5g3BCW6iYaV89B2i64cqEUZD7e0
3. Place 'yolov3-spp-ultralytics.pt' file in the weights folder:
  * to save time, move the file from the above link to your GDrive
  * then drag and drop from your GDrive opened in Colab to weights folder
4. run this command
`python train.py --data data/smalcoco/smalcoco.data --batch 10 --cache --epochs 25 --nosave`

For custom dataset:
1. use the annotation tool given in the repo, for more details read [here](https://github.com/millermuttu/YOLOv3_custom/tree/main/annotation_tool)
2. Follow the installation steps as mentioned in the repo. 
3. For the assignment, download 500 images of your unique object. 
4. Annotate the images using the Annotation tool. 
```
data
  --customdata
    --images/
      --img001.jpg
      --img002.jpg
      --...
    --labels/
      --img001.txt
      --img002.txt
      --...
    custom.data #data file
    custom.names #your class names
    custom.txt #list of name of the images you want your network to be trained on. Currently we are using same file for test/train
```
5. As you can see above you need to create **custom.data** file. For 4 class example, your file will look like this:
```
  classes=4
  train=data/customdata/train.txt
  test=data/customdata/test.txt 
  names=data/customdata/custom.names
```
6. As you it a poor idea to keep test and train data same, but the point of this repo is to get you up and running with YoloV3 asap. You'll probably do a mistake in writing to custom.txt file. This is how our file looks like (please note the .s and /s):
```
./data/customdata/images/img001.jpg
./data/customdata/images/img002.jpg
./data/customdata/images/img003.jpg
...
```
7. You need to add custom.names file as you can see above. For our example, we downloaded images of Walle. Our custom.names file look like this:
```
walle
```
8. Walle above will have a class index of 0. 
9. For COCO's 80 classes, VOLOv3's output vector has 255 dimensions ( (4+1+80)*3). Now we have 1 class, so we would need to change it's architecture.
10. Copy the contents of 'yolov3-spp.cfg' file to a new file called 'yolov3-custom.cfg' file in the data/cfg folder. 
11. Search for 'filters=255' (you should get entries entries). Change 255 to 27 = (4+1+4)*3
12. Search for 'classes=80' and change all three entries to 'classes=1'
13. Since you are lazy (probably), you'll be working with very few samples. In such a case it is a good idea to change:
  * burn_in to 100
  * max_batches to 5000
  * steps to 4000,4500
14. Don't forget to perform the weight file steps mentioned in the sectio above. 
15. Run this command `python train.py --data data/customdata/custom.data --batch 10 --cache --cfg cfg/yolov3-custom.cfg --epochs 3 --nosave`


**Results**
After training for 30 Epochs, results look awesome!

![image](https://github.com/millermuttu/TSAI-EVA5/blob/master/week13/YOLOv3_custom/output/Atest_Moment2.jpg)
