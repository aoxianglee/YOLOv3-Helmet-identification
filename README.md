# YOLO-安全帽识别
基于python，tensorflow，以yolov3模型训练自己的数据集并测试
keras-yolo3代码： https://github.com/qqwweee/keras-yolo3

环境配置：python 3.6.5   tensorflow1.15.0   tensorflow-gpu1.4.0  cuda8.0   cudnn6.0.21

制作voc_2007数据集
因为需要大量佩戴安全帽图片数据，本项目主要通过爬虫拿到，共有7581张图片，包含9044个佩戴安全帽的bounding box（正类），以及111514个未佩戴安全帽的bounding box(负类)，将爬取后的图片放到VOC2007\JPEGImages文件夹下，使用labelImg软件对图片进行标注，正标签为hat，负标签为person，标注好的xml文件存放到VOC2007\Annotations文件夹中。

生成索引
运行convert_to_txt.py生成索引文件存放在VOC2007\ImageSets\Main
生成训练索引
修改voc_annotation.py文件，将classes的内容改成要训练的对象，运行。生成三个文件"2007_test.txt", “2007_train.txt”, “2007_val.txt”，把前缀"2007_"去掉，得到 “test.txt”, “train.txt”, “val.txt”三个训练索引文件
修改配置文件
修改配置文件model_data 文件夹中的coco_classes.txt和voc_classes.txt，将里面的对象改成自己要训练的对象。
执行训练
将model_data文件夹中yolo_.h5复制，重命名为yolo_weights.h5作为预训练模型。
修改epochs，batch_size等参数，运行train.py文件。

注：import os
#os.environ['CUDA_VISIBLE_DEVICES'] = '-1'  # 这一行注释掉就是使用gpu，不注释就是使用cpu

模型检测
单个图片/视频 运行yolo_video.py
批量图片  修改yolo.py  将vid = cv2.VideoCapture(0) 注释掉，运行yolo_test.py
摄像头检测   修改yolo.py  将vid = cv2.VideoCapture(video_path)注释掉，运行yolo_video.py
