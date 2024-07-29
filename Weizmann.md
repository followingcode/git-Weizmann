# 数据集解释

## 0. 概览

总共9个不同的人，每个人执行10个不同的动作

lena是执行动作的人的名字，有些动作他执行了两次，所以我命名时第二个动作就以lena2命名了。lena2_6没有，就说明6号标签里lena执行了一次动作。



```python
那train文件夹里面排列顺序就是
daria_0到daria_9，
denis_0到denis_9，
eli_0到eli_9，
ido_0到ido_9，
ira_0到ira_9，
lena_0到lena_9，
lena2_5，lena2_7，lena2_8
lyova_0到lyova_9

而test文件夹里面排列顺序为
moshe_0到moshe_9
shahar_0到shahar_9
```



## 1. 动作ID

总共10种动作类型，表示每个人执行10个不同的动作

```python
bend.zip
jack.zip
jump.zip
pjump.zip
run.zip
side.zip
skip.zip
walk.zip
wave1.zip
wave2.zip
```

翻译为中文

```python
bend翻译为弯腰

jumping jack翻译为开合跳

jump翻译为跳

jump in place翻译为原地跳跃

run翻译为跑

gallop sideways翻译为横向侧跑

skip翻译为单脚跳跃

walk翻译为走

one-hand wave翻译为单手挥舞

two-hands wave翻译为双手挥舞

```



## 2. 人物ID

总共9个不同的人

```python
daria
denis
eli
ido
ira
lena
lyova
moshe
shahar
```



```python
bend.zip
jack.zip
jump.zip
pjump.zip
```



## 3. 特殊情况

```
run.zip
```



```python
daria_run.avi
denis_run.avi
eli_run.avi
ido_run.avi
ira_run.avi
lena_run1.avi
lena_run2.avi
lyova_run.avi
moshe_run.avi
shahar_run.avi
```

发现lena_run1.avi和lena_run2.avi是两个

所以是10个



## 4. 继续

```
side.zip
```

## 5. 特殊情况

```python
skip.zip
```



```python
daria_skip.avi
denis_skip.avi
eli_skip.avi
ido_skip.avi
ira_skip.avi
lena_skip1.avi
lena_skip2.avi
lyova_skip.avi
moshe_skip.avi
shahar_skip.avi
```

发现lena_skip1.avi和lena_skip2.avi是两个

所以是10个

## 6. 特殊情况

```
walk.zip
```



```python
daria_walk.avi
denis_walk.avi
eli_walk.avi
ido_walk.avi
ira_walk.avi
lena_walk1.avi
lena_walk2.avi
lyova_walk.avi
moshe_walk.avi
shahar_walk.avi
```

发现lena_walk1.avi和lena_walk2.avi是两个

所以是10个

## 7. 继续

```python
wave1.zip
wave2.zip
```



## 8. 编号

```python
分成训练集和测试集，目录如下，最好让视频名称按照 ‘视频名_类别.mp4’这样的方式（主要是让视频名称里面含有类别的字段、或者类别的序号，后续好处理）

我的视频名称是这样的，daria_0.avi，我改了原始的视频名称

类别标签按照下面的方式定义，类别序号从0开始，且必须是连续的，要不然后面训练时会报错。

{'bend': '1', 'jack': '2', 'jump': '3', 'pjump': '4','run':'5','side':'6','skip':'7','walk':'8','wave1':'9','wave2':'0'}
```



```python
'wave2':'0'
'bend': '1'
'jack': '2'
'jump': '3'
'pjump':'4'
'run':'5'
'side':'6'
'skip':'7'
'walk':'8'
'wave1':'9'
```

发现类别是从0开始的

**也就是说你的类别label必须从0开始**



## 9. 注意

**类别序号从0开始，且必须是连续的**

**类别序号从0开始，且必须是连续的**

**类别序号从0开始，且必须是连续的**



## 10. 训练集和测试集划分方式

可以思考一下是怎么划分的，盲猜是按人物ID来划分的，比如总共9个不同的人

其实人物也是可以编号的，不止动作可以编号，人物依然可以编号

这里是按人物ID来进行划分训练集和测试集



```python
训练集是
daria
denis
eli
ido
ira
lena
lyova

测试集是
moshe
shahar
```



发现训练集是7个人，测试集是2个人，也就是说根据人物ID来进行划分的

这样的话有点类似于模仿NTU RGB+D数据集根据人物ID来划分训练集和测试集。



## 11. 生成ann_file文件命名格式

```python
import os
from mmcv import load, dump
def traintest(dirpath, pklname, newpklname):
    os.chdir(dirpath)
    train = load('/root/pyskl/until/train.json') 
    test = load('/root/pyskl/until/test.json')
    annotations = load(pklname)
    split = dict()
    split['xsub_train'] = [x['vid_name'] for x in train] #指定训练集里样本名字
    split['xsub_val'] = [x['vid_name'] for x in test] #指定测试集里样本名字
    dump(dict(split=split, annotations=annotations), newpklname)  #最后用于训练的pkl文件名
 
    
if __name__ == '__main__':
 
    dirpath = '/root/pyskl/tools/data/Weizmann'
    pklname = '/root/pyskl/until/train.pkl'
    newpklname = '/root/pyskl/until/Weizmann_xsub_litehrnet.pkl'
    traintest(dirpath, pklname, newpklname)
```



这样的话就可以生成训练需要的骨架数据了

