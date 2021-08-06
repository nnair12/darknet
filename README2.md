# Yolo v4, v3 and v2 for Windows and Linux

## (neural networks for object detection)

About Darknet framework: http://pjreddie.com/darknet/

AlexeyAB's implementation: https://github.com/AlexeyAB/darknet

Manual: https://github.com/AlexeyAB/darknet/wiki

## Introduction


## Steps

1. First, download a pretrained weights file. I used [darknet53.conv.74](https://pjreddie.com/media/files/darknet53.conv.74). Due to the size, this file is not included in the repository
2. I then created the config file yolov3-kitti.cfg. It can be located in the `cfg/` directory.
3. I created file `obj.names` in the directory `data/`, with objects names - each in new line
  ```ini
  vehicle
  person
  cyclist
  ```
4. I then created file `obj.data` in the directory `data/`, containing (where **classes = number of objects**):
  ```ini
  classes = 2
  train  = data/train.txt
  valid  = data/test.txt
  names = data/obj.names
  backup = backup/
  ```
5. Put image-files (.jpg) of your objects in the directory `data/obj/`. 000001.png is present in the directory as a sample.
6. For each image in the directory, you need to create a text file (with the same name) that describes each object in that image. Give the label (as a number) followed by the coordinates for the box. 000001.txt is present in the directory as a sample.
  ```ini
  0 0.4948309178743961 0.46086666666666665 0.02442834138486315 0.08759999999999998
  0 0.3266666666666667 0.51288 0.0291304347826087 0.0575466666666667
  2 0.5497504025764895 0.47717333333333334 0.009967793880837355 0.07994666666666672
  ```
7. I created the files train.txt and test.txt in the `data/` directory. Each file contains the pngs to be included in the training and validation data, respectively.
8. Since I trained using a GPU VM on Paperspace, I needed to modify some fields in the Makefile. I set `GPU=1` in the first few lines of Makefile.
9. Now, I can start training. I use the following command: `./darknet detector map data/obj.data cfg/yolov3-kitti.cfg file darknet53.conv.74 -dont_show -ext_output < data/train.txt > data/result.txt`
10. The output gets saved in the newly created backup folder. The results for every 10000 iterations, as well as the best results (according to IOU) and most recent results are kept in the folder. I stopped training when I noticed decreasing performance every iteration. The resulting file was called yolov3-kitt_best.weights, which is too large to be included in the repository.

## Citation

```
@misc{bochkovskiy2020yolov4,
      title={YOLOv4: Optimal Speed and Accuracy of Object Detection}, 
      author={Alexey Bochkovskiy and Chien-Yao Wang and Hong-Yuan Mark Liao},
      year={2020},
      eprint={2004.10934},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
```

```
@InProceedings{Wang_2021_CVPR,
    author    = {Wang, Chien-Yao and Bochkovskiy, Alexey and Liao, Hong-Yuan Mark},
    title     = {{Scaled-YOLOv4}: Scaling Cross Stage Partial Network},
    booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    month     = {June},
    year      = {2021},
    pages     = {13029-13038}
}
```
