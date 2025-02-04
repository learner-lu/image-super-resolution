# Image super resolution

## Introduction

this project is a reproduction of the famous paper `Learning a Deep Convolutional Network for Image Super-Resolution` which was publish in 2014 using CNN to do super resolution work

- [paper pdf-download](https://github.com/luzhixing12345/image-super-resolution/releases/download/v0.0.2/Learning.a.Deep.Convolutional.Network.for.Image.Super-Resolution.pdf)

## Result

|low-resolution|high-resolution|
|:--:|:--:|
|<img src="https://raw.githubusercontent.com/learner-lu/picbed/master/QQ%E6%88%AA%E5%9B%BE20220112003016.png">|<img src = "https://raw.githubusercontent.com/learner-lu/picbed/master/2.png">|

## Related work

- [A CSDN blog](https://blog.csdn.net/qianbin3200896/article/details/104181552?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164188419916780264030042%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=164188419916780264030042&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-104181552.pc_search_insert_ulrmf&utm_term=%E8%B6%85%E5%88%86%E8%BE%A8%E7%8E%87%E9%87%8D%E5%BB%BA&spm=1018.2226.3001.4187) that describes details about what is super resolution
- [An excellent github](https://github.com/yjn870/SRCNN-pytorch), this repository was based on it, it offers me great help.
- [great job](https://github.com/xinntao/Real-ESRGAN)!!! strongly recommend

## Requirements

- pytorch

- ```shell
  pip install -r requirements.txt
  ```
  
## Use

### Step1: Prepare the dataset

  download the standard dataset
  The 91-image(train set), Set5(test set) dataset converted to HDF5 can be downloaded from the links below.
  | Dataset | Scale | Type | Link |
  |---------|-------|------|------|
  | 91-image | 2 | Train | [Download](https://github.com/learner-lu/image-super-resolution/releases/download/v0.0.1/91-image_x2.h5) |
  | 91-image | 3 | Train | [Download](https://github.com/learner-lu/image-super-resolution/releases/download/v0.0.1/91-image_x3.h5) |
  | 91-image | 4 | Train | [Download](https://github.com/learner-lu/image-super-resolution/releases/download/v0.0.1/91-image_x4.h5) |
  | Set5 | 2 | Eval | [Download](https://github.com/learner-lu/image-super-resolution/releases/download/v0.0.1/Set5_x2.h5) |
  | Set5 | 3 | Eval | [Download](https://github.com/learner-lu/image-super-resolution/releases/download/v0.0.1/Set5_x3.h5) |
  | Set5 | 4 | Eval | [Download](https://github.com/learner-lu/image-super-resolution/releases/download/v0.0.1/Set5_x4.h5) |

  Download any one of 91-image and Set5 in the same Scale and then **move them under `./datasets` as `./datasets/91-image_x2.h5` and `./datasets/Set5_x2.h5`**

### Step2: Train the model

- easy run

  ```python
  python train.py
  ```

- about arguments
  - `train-file` `eval-file` 2/3/4, different datasets to choose
  - `batch-size`
  - `num-workers`
  - `lr` learning rate
  - `epoch`
  - `f` frequency to test the model
  - `model-dir` where the model was saved

  ```python
  python train.py --train-file 4 --eval-file 4 --batch-size 64 --lr 1e-5 --num-workers 8 --epoch 500 --f 10 
  ```

  All model will be saved under `./model` and the best model is `./model/best.pth`

### Step3: Download the pretrained model(**If you have trained your model, skip this part)

- [trained by x2](https://github.com/learner-lu/image-super-resolution/releases/download/v0.0.1/b.pth)
- [trained by x4](https://github.com/learner-lu/image-super-resolution/releases/download/v0.0.1/best.pth)

**Move the model weights under `./model` as `./model/best.pth` or `./model/b.pth`**

### Step4: Do super resolution

```python
python use.py --weights-file ./model/best.pth --image x/xx/xxx.jpg
```

By default, the weights file is `./model/best.pth`, if you want to use `b.pth` please replace it.
A picture will be created named as `xxx_srcnn.jpg`

## Conclusion

This is just a CNN try for me, to familiar with basic steps of machine learning, almost all code comes from [here](https://github.com/yjn870/SRCNN-pytorch),I just follow his step and reapperance his job. The final result couldn't satisfy me, it actually should be called little super resolution haha. However, there's no doubt that idea of SRCNN was novelty in that period. If you want to improve the neural network, build a deeper model and try Resnet model.

If you are interested in it, you could see this [great job](https://github.com/xinntao/Real-ESRGAN) and i strongly recommend it.

## some errors you may occur

- OSError: Unable to open file (file locking disabled on this file system (use HDF5_USE_FILE_LOCKING
  
  Solution:

  ```shell
  #linux
  nano ~/.bashrc
  ```

  Add `export HDF5_USE_FILE_LOCKING='FALSE'` in a line, use `Ctrl-X` to exit

  ```shell
  source ~/.bashrc
  ```

- OSError: [WinError 1455] 页面文件太小，无法完成操作。 Error loading "C:\ProgramData\Anaconda3\envs\study_and_test\lib\site-packages\torch\lib\caffe2_detectron_ops_gpu.dll" or one of its dependencies.
  Solution:

  ```python
  python train.py --num-workers 0
  ```
