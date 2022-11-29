# CMPE258
CMPE258 - Term Project


### Environment Setup

> ⚠️ This step requires GPU. This model was trained on HPC.
> We faced memory issues when trained locally and it takes too long to converge. 
> ** Recommend to do this on HPC. **

1. This setup is done using conda env, you can use pip too.

Incase you are facing an error in setting up and are you on *windows*, make sure to install Visual C++ 2015 build tools first. Else pycocotools will not install. If you are setting up on HPC, skip this step.

```
conda create -n env
conda activate env
pip install git+https://github.com/philferriere/cocoapi.git#subdirectory=PythonAPI
conda install pytorch torchvision cudatoolkit=10.1 -c pytorch
```




1. Clone the yolact repository 
```
git clone https://github.com/dbolya/yolact
```

2. Download the dataset from the [Drive Link](https://drive.google.com/drive/u/0/folders/1o9hCcc947dXmdZxKSu9VoJ5s4mgs_-g4) and place it at the same path as the train.py folder

3. Go to the config file `yolact/data/config.py`
4. Add the **dataset details** in the config under `# ----------------------- DATASETS ----------------------- #` 

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
5. Add the config for the model under `# ----------------------- YOLACT v1.0 CONFIGS ----------------------- #
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

6. 

### Train


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

