#### 基于python，tensorflow框架，以yolov3模型训练自己的安全帽数据集并测试 

点击下载[keras—yolov3源代码](https://github.com/qqwweee/keras-yolo3) 
点击下载[yolov3.weights权重文件](https://pjreddie.com/media/files/yolov3.weights)

>环境配置：   
python 3.6.5            
tensorflow1.15.0   
tensorflow-gpu1.4.0     
cuda8.0     
cudnn6.0.21

##### 1.数据集制作
制作voc_2007数据集,首先爬取大量佩戴安全帽和未佩戴图片，对图片数据进行清理，批量重命名修改文件类型，存放到VOC2007\JPEGImages文件夹下，使用labelImg软件对图片进行标注，正标签为hat，负标签为person，标注好的xml文件存放到VOC2007\Annotations文件夹中。  

##### 2.生成索引
运行convert_to_txt.py生成索引文件存放在VOC2007\ImageSets\Main中。  
修改voc_annotation.py文件，将classes的内容改成自己要训练的对象    
![](https://github.com/aoxianglee/img-storage/blob/master/voc_annotation.PNG)     
运行代码后，生成三个文件"2007_test.txt", “2007_train.txt”, “2007_val.txt”，去掉前缀"2007_"，得到三个训练索引文件 

##### 3.修改配置文件    
 修改配置文件model_data 文件夹中的coco_classes.txt和voc_classes.txt，将里面的对象改成自己要训练的对象。     
 ![](https://github.com/aoxianglee/img-storage/blob/master/xiugaipeizhiwenjian.png)     
 
 ##### 4.训练模型
 将model_data文件夹中的yolo.h5复制，改名为yolo_weights.h5，将其作为预训练权重。 
 >注：对于预训练权重，其实是迁移学习的方法，有些博文其实让修改yolo3.cfg文件，然后重新生成权重文件，但是发现这样的做法最后训练时会出现val_loss:nan的情况，所以建议直接用原来官方的权重文件yolo.h5。还有一些博文修改了原来train.py的训练代码，不预加载训练权重，重头开始训练，这种适合样本数量大的数据集，否则训练结果会不稳定。   
 
 ##### 5.执行训练   
 将model_data文件夹中yolo_.h5复制，重命名为yolo_weights.h5作为预训练模型。 修改epochs，batch_size等参数，运行train.py文件。
 >注:```import os    #os.environ['CUDA_VISIBLE_DEVICES'] = '-1' #``` 这一行注释掉就是使用gpu，不注释就是使用cpu    
 
 ##### 6.模型检测   
 * 单个图片/视频：命令行运行yolo_video.py   
 * 批量图片 修改yolo.py 将vid = cv2.VideoCapture(0) 注释掉，运行 yolo_video.py  
 * 摄像头检测 修改yolo.py 将vid = cv2.VideoCapture(video_path)注释掉，运行yolo_video.py
 ***
##### 检测效果  
 ![](https://github.com/aoxianglee/img-storage/blob/master/result_test_000136.jpg)  
 ![](https://github.com/aoxianglee/img-storage/blob/master/result_test_000149.jpg)  
 ![](https://github.com/aoxianglee/img-storage/blob/master/result_test_000233.jpg) 
 
 
 


