# CMPE258
CMPE258 - Term Project


### Setup 



### Train


### Validation 

1. Set the image/ video path
```
video="PXL_20221128_181428548.mp4"
```
2. Run the eval script for the image. The `video` parameter takes both input and output in the format of `input video path: output video path`

For example 

```
python eval.py --trained_model='yolact_resnet50_cig_butts_1706_105817_interrupt.pth' --score_threshold=0.3 --top_k=5 --video=input/$video:output/PXL_20221128_181428548_segmented.mp4
```

where `PXL_20221128_181428548_segmented.mp4` is the video with sematic segmentation of cigarette butts.

The same syntax can be done for image too. Replace --video with `--image=inputpath:output_path`

⚠️ This step requires GPU too, this can be done on HPC/ Colab.

Link to [Colab evalutaion notebook] (https://colab.research.google.com/drive/1kq2hs-tSiPx0x6MoPA0X5xLYip2UOUH2?usp=sharing)

### Results 
