## Train

```shell
# train
CUDA_VISIBLE_DEVICES=0 PORT=29500 \
./tools/dist_train.sh configs/rretinanet/rretinanet_obb_r50_fpn_1x_dota_v3.py 1
```


## Test
```shell
CUDA_VISIBLE_DEVICES=0 PORT=29500 \
./tools/dist_test.sh configs/rretinanet/rretinanet_obb_r50_fpn_1x_dota_v3.py \
        work_dirs/rretinanet_obb_r50_fpn_1x_dota_v3/epoch_12.pth 1 --mAP
```


## Inference & Submit


```shell
CUDA_VISIBLE_DEVICES=0 PORT=29500 \
./tools/dist_test.sh configs/rretinanet/rretinanet_obb_r50_fpn_1x_dota_v3.py \
        work_dirs/rretinanet_obb_r50_fpn_1x_dota_v3/epoch_12.pth 1 --format-only\
        --eval-options submission_dir=work_dirs/rretinanet_obb_r50_fpn_1x_dota_v3/Task1_results 
```

## Crop Images

For DOTA dataset, please crop the original images into 1024×1024 patches with an overlap of 200 by run 
```shell
python tools/split/img_split.py --base_json \
       tools/split/split_configs/split_configs/dota1_0/ss_trainval.json

python tools/split/img_split.py --base_json \
       tools/split/split_configs/dota1_0/ss_test.json

```
Please change path in `ss_trainval.json`, `ss_test.json` to your path. (Forked from [BboxToolkit](https://github.com/jbwang1997/BboxToolkit), which is faster then DOTA_Devkit.)