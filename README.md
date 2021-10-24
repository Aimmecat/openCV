# Brief Introduce:

首先将群里的文件夹解压到桌面！

1、打开Pycharm，打开Road_Block_Recognition文件夹，将当前的编译环境改成Python3.x（pytorch），然后在Terminal中运行如下命令：

```
conda update --all
conda install --channel https://conda.anaconda.org/menpo opencv
```

2、安装Opencv，直接在电脑上装即可，参考这篇文章，操作到环境变量配置完毕即可，第三部分部署不需要看

```
https://blog.csdn.net/maizousidemao/article/details/81474834?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163479705916780261970635%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=163479705916780261970635&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~hot_rank-2-81474834.first_rank_v2_pc_rank_v29&utm_term=python+opencv%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F&spm=1018.2226.3001.4187
```

3、Opencv安装完毕之后，打开群里发的文件夹，可以先看一下每个文件夹里都是什么内容，由于训练需要，文件夹中不止有桶锥的图片，还有negative文件夹中不包含待识别物品的图片。

4、treinamento是opencv生成的文件夹，所以每次重新训练的时候，都要把它清掉（虽然我也没试过不删除会有什么影响）。同理，如果在使用cmd命令生成info.txt，negative txt，vector.vec文件时报错的话，最好也删掉重新生成。

5、然后就可以开始生成文件啦！

ps：如果想看每个参数是什么意思，可以参考这篇文章：

```
https://www.cnblogs.com/xixixing/p/12308605.html
```

打开cmd，按照顺序依次输入如下命令：

```
1、cd C:\Users\xxxxx\Desktop\Road_Block_Recognition
(首先进入桌面下的文件夹路径)
2、opencv_annotation -a=info.txt -i=positive -m=500 -r=6
(这个命令运行完之后，正确的情况是会跳出训练的图片的，然后cmd中会有操作的指令，正确框选物品就可以了)
3、opencv_createsamples -info info.txt -vec vector.vec -num 200 -w 24 -h 24

运行第四条指令之前，先在Pycharm中将buildListNegative.py运行一下
4、
opencv_traincascade -data treinamento -vec vector.vec -bg negative.txt -numStages 25 -numPos 40 -numNeg 100 -umStaes 25 -w 24 -h 24 -acceptanceRatioBreakValue 1.0e-5
(运行完之后，就会开始训练了)
```

Ps：关于traincascade的参数的选择(不同的参数应该会影响最终的训练结果)：

> **data** 训练的分类器的存储目录
> **vec** 正样本文件，由open_createsamples.exe生成，正样本文件后缀名为.vec
> **bg** 负样本说明文件，主要包含负样本文件所在的目录及负样本文件名
> **numPos** 每级分类器训练时所用到的正样本数目，应小于vec文件中正样本的数目，具体数目限制条件为：numPos+（numStages- 1）*numPos*（1- minHitRate）<=vec文件中正样本的数目。**根据我的经验，一般为正样本文件的80%**
> **numNeg** 每级分类器训练时所用到的负样本数目，可以大于- bg指定的图片数目。**根据我的经验，一般为numPos的2-3倍**
> **numStages** 训练分类器的级数，强分类器的个数。**根据我的经验，一般为12-20**
> **precalcValBufSize** 缓存大小，用于存储预先计算的特征值，单位MB，**根据自己内存大小分配**
> **precalcIdxBufSize** 缓存大小，用于存储预先计算的特征索引，单位MB，**根据自己内存大小分配**
> **featureType** 训练使用的特征类型，目前支持的特征有Haar，LBP和HOG
> **w** 训练的正样本的宽度
> **h** 训练的正样本的高

6、训练完成之后，点击analiseTreinamento.py，程序第四行：

```
img = cv2.imread("Test/Test-img(8).jpg")
```

可以对img(8)中的数字进行更改，然后运行，即可看到最终的结果。



以下是每个py文件的作用：

analiseTreinamento.py used to test one picture

analiseTreinametoVideo.py used to use your camera to recognize timly

buildListNegative.py used to create negative.txt, run it after you put the negative picture to file

info.txt created by command, it will be touched in the later

renameFiles.py used to rename the img you saved in "negative", "positive" and 'Test'

vectire.vec created by command, it will be touched soon
