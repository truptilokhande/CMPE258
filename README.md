# CMPE258

### Idea

Detetction of Cigarette Butts using Semantic Segmentation (Yolact)

Cigarette butts are poisoning shoreline animals. We aim to use ML to identify and detect and locate the cigartte butts.
Out future plan is to build a bot on which this ML algo will run inorder to pick up waste near oceans, gutter, land etc.
As a first step, we aim to build the ML aolution to detect and idenitfy cigarette butts. 

### Dataset 

We have used the [Cigarette Butts Dataset](https://www.immersivelimit.com/datasets/cigarette-butts) and augmented to improve the model.
1. Additional images from the internet of cigarette butts
2. Took a cigartte butt and took photos of it 


### Annote dataset

1. Clone the COCO Annotator froom [github](https://github.com/jsbroks/coco-annotator)
2. Annote and images and export the COCO annotation file
3. Add these annotations to the existing annotation file in `cig_butts/train` folder

![image](https://user-images.githubusercontent.com/98665151/204422071-aa14ac54-a2b0-43ff-bf02-bf83c347fcb6.png)


### Environment and Config  

> ⚠️ This step requires GPU. This model was trained on HPC.
> We faced memory issues when trained locally and it takes too long to converge. 
** Highly recommend to train the model on HPC. **

1. This setup is done using conda env, you can use pip too.

Incase you are facing an error in setting up and are you on *windows*, make sure to install Visual C++ 2015 build tools first. Else pycocotools will not install. If you are setting up on HPC, skip this step.

```
conda create -n env
conda activate env
pip install git+https://github.com/philferriere/cocoapi.git#subdirectory=PythonAPI
conda install pytorch torchvision cudatoolkit=10.1 -c pytorch
```


2. Clone the yolact repository 
```
git clone https://github.com/dbolya/yolact
```

3. Download the dataset from the [Drive Link](https://drive.google.com/drive/u/0/folders/1o9hCcc947dXmdZxKSu9VoJ5s4mgs_-g4) and place it at the same path as the train.py folder


4. Go to the config file `yolact/data/config.py`
5. Add the **dataset details** in the config under `# ----------------------- DATASETS ----------------------- #` 

```
cig_butts_dataset = dataset_base.copy({
  'name': 'Cig Butts',
  'train_info': 'cig_butts/train/coco_annotations.json',
  'train_images': 'cig_butts/train/images/',
  'valid_info': 'cig_butts/val/coco_annotations.json',
  'valid_images': 'cig_butts/val/images/',
  'class_names': ('cig_butt'),
  'label_map': { 1:  1 }
})

```
6. This step configures the model, add the below script under `# ----------------------- YOLACT v1.0 CONFIGS ----------------------- #
`

```
yolact_resnet50_cig_butts_config = yolact_resnet50_config.copy({
    'name': 'yolact_plus_resnet50_cig_butts',
    'dataset': cig_butts_dataset,
    'num_classes': len(cig_butts_dataset.class_names) + 1,

    # Image Size
    'max_size': 512,
})

```

### Training

This model was trained this on top of a pretrained Resnet model that was trained on COCO dataset.

#### Steps 

1. Download the pretrained weights - here we are using [Resnet50](https://drive.google.com/file/d/1Jy3yCdbatgXa5YYIdTCRrSV0S9V5g1rn/view?usp=sharing) and store in in the `weights/` directory. If you are training the model for the first time, you will need to create this weights directory. 

2. Train the model 
```
python ./train.py --config=yolact_resnet50_cig_butts_config
```

3. You can interrupt the model training once you are satisfies with the mAP score. (Press Ctrl+C to interrupt model). After training you can find the model at `weights/`. It will follow the naming convention that you configured in Step 6 `yolact_resnet50_cig_butts_x_x_interrupt.pth`




### Validation 

> ⚠️ This step requires GPU too, this can be done on HPC/ Colab.

Link to [Colab evalutaion notebook](https://colab.research.google.com/drive/1kq2hs-tSiPx0x6MoPA0X5xLYip2UOUH2?usp=sharing)

#### Steps 

1. If using Google Colab set runtime to GPU. (Go to Menu bar : Runtime > Change Runtime)
![image](https://user-images.githubusercontent.com/98665151/204405998-35aee7b3-11f1-47a2-b880-ef8207a5fe7a.png)

2. Set the image/ video path
```
video="PXL_20221128_181428548.mp4"
```
3. Run the eval script for the image. The `video` parameter takes both input and output in the format of `input_video_path:output_video_path`

For example 

```
python eval.py --trained_model='yolact_resnet50_cig_butts_1706_105817_interrupt.pth' --score_threshold=0.3 --top_k=5 --video=input/$video:output/PXL_20221128_181428548_segmented.mp4
```

where `PXL_20221128_181428548_segmented.mp4` is the video with sematic segmentation of cigarette butts.

The same syntax can be done for image too. Replace --video with `--image=inputpath:output_path`




### Results 

Butts scattered on ground and pine shavings along with other trash


![ezgif-4-03762b4cf6](https://user-images.githubusercontent.com/98665151/204409375-66342d65-9992-4b29-982c-09ffa5c6ee07.gif)

