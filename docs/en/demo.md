# Demo

We provide some task-specific demo scripts to test a single image.

#### Inpainting

You can use the following commands to test a pair of images for inpainting.

```shell
python demo/inpainting_demo.py \
    ${CONFIG_FILE} \
    ${CHECKPOINT_FILE} \
    ${MASKED_IMAGE_FILE} \
    ${MASK_FILE} \
    ${SAVE_FILE} \
    [--imshow] \
    [--device ${GPU_ID}]
```

If `--imshow` is specified, the demo will also show image with opencv. Examples:

```shell
python demo/inpainting_demo.py \
    configs/global_local/gl_8xb12_celeba-256x256.py \
    https://download.openmmlab.com/mmediting/inpainting/global_local/gl_256x256_8x12_celeba_20200619-5af0493f.pth \
    tests/data/inpainting/celeba_test.png \
    tests/data/inpainting/bbox_mask.png \
    tests/data/inpainting/inpainting_celeba.png
```

The predicted inpainting result will be save in `tests/data/inpainting/inpainting_celeba.png`.

#### Matting

You can use the following commands to test a pair of images and trimap.

```shell
python demo/matting_demo.py \
    ${CONFIG_FILE} \
    ${CHECKPOINT_FILE} \
    ${IMAGE_FILE} \
    ${TRIMAP_FILE} \
    ${SAVE_FILE} \
    [--imshow] \
    [--device ${GPU_ID}]
```

If `--imshow` is specified, the demo will also show image with opencv. Examples:

```shell
python demo/matting_demo.py \
    configs/dim/dim_stage3-v16-pln_1000k-1xb1_comp1k.py \
    https://download.openmmlab.com/mmediting/mattors/dim/dim_stage3_v16_pln_1x1_1000k_comp1k_SAD-50.6_20200609_111851-647f24b6.pth \
    tests/data/matting_dataset/merged/GT05.jpg \
    tests/data/matting_dataset/trimap/GT05.png \
    tests/data/matting_dataset/pred/GT05.png
```

The predicted alpha matte will be save in `tests/data/matting_dataset/pred/GT05.png`.

#### Restoration (Image)

You can use the following commands to test an image for restoration.

```shell
python demo/restoration_demo.py \
    ${CONFIG_FILE} \
    ${CHECKPOINT_FILE} \
    ${IMAGE_FILE} \
    ${SAVE_FILE} \
    [--imshow] \
    [--device ${GPU_ID}] \
    [--ref-path ${REF_PATH}]
```

If `--imshow` is specified, the demo will also show image with opencv. Examples:

```shell
python demo/restoration_demo.py \
    configs/esrgan/esrgan_x4c64b23g32_400k-1xb16_div2k.py \
    https://download.openmmlab.com/mmediting/restorers/esrgan/esrgan_x4c64b23g32_1x16_400k_div2k_20200508-f8ccaf3b.pth \
    tests/data/image/lq/baboon_x4.png \
    demo/demo_out_baboon.png
```

You can test Ref-SR by providing `--ref-path`. Examples:

```shell
python demo/restoration_demo.py \
    configs/ttsr/ttsr-gan_x4c64b16_500k-1xb9_CUFED.py \
    https://download.openmmlab.com/mmediting/restorers/ttsr/ttsr-gan_x4_c64b16_g1_500k_CUFED_20210626-2ab28ca0.pth \
    tests/data/frames/sequence/gt/sequence_1/00000000.png \
    demo/demo_out.png \
    --ref-path tests/data/frames/sequence/gt/sequence_1/00000001.png
```

#### Restoration (Face Image)

You can use the following commands to test an face image for restoration.

```shell
python demo/restoration_face_demo.py \
    ${CONFIG_FILE} \
    ${CHECKPOINT_FILE} \
    ${IMAGE_FILE} \
    ${SAVE_FILE} \
    [--upscale-factor] \
    [--face-size] \
    [--imshow] \
    [--device ${GPU_ID}]
```

If `--imshow` is specified, the demo will also show image with opencv. Examples:

```shell
python demo/restoration_face_demo.py \
    configs/glean/glean_in128out1024_300k-4xb2_ffhq-celeba-hq.py \
    https://download.openmmlab.com/mmediting/restorers/glean/glean_in128out1024_4x2_300k_ffhq_celebahq_20210812-acbcb04f.pth \
    tests/data/image/face/000001.png \
    tests/data/image/face/pred.png \
    --upscale-factor 4
```

#### Restoration (Video)

You can use the following commands to test a video for restoration.

```shell
python demo/restoration_video_demo.py \
    ${CONFIG_FILE} \
    ${CHECKPOINT_FILE} \
    ${INPUT_DIR} \
    ${OUTPUT_DIR} \
    [--window-size=${WINDOW_SIZE}] \
    [--device ${GPU_ID}]
```

It supports both the sliding-window framework and the recurrent framework. Examples:

EDVR:

```shell
python demo/restoration_video_demo.py \
    configs/edvr/edvrm_wotsa_reds_600k-8xb8.py \
    https://download.openmmlab.com/mmediting/restorers/edvr/edvrm_wotsa_x4_8x4_600k_reds_20200522-0570e567.pth \
    data/Vid4/BIx4/calendar/ \
    demo/output \
    --window-size=5
```

BasicVSR:

```shell
python demo/restoration_video_demo.py \
    configs/basicvsr/basicvsr_2xb4_reds4.py \
    https://download.openmmlab.com/mmediting/restorers/basicvsr/basicvsr_reds4_20120409-0e599677.pth \
    data/Vid4/BIx4/calendar/ \
    demo/output
```

The restored video will be save in ` demo/output/`.

#### video frame interpolation

You can use the following commands to test a video for frame interpolation.

```shell
python demo/video_interpolation_demo.py \
    ${CONFIG_FILE} \
    ${CHECKPOINT_FILE} \
    ${INPUT_DIR} \
    ${OUTPUT_DIR} \
    [--fps-multiplier ${FPS_MULTIPLIER}] \
    [--fps ${FPS}]
```

`${INPUT_DIR}` / `${OUTPUT_DIR}` can be a path of video file or the folder of a sequence of ordered images.
If `${OUTPUT_DIR}` is a path of video file, its frame rate can be determined by the frame rate of input video and `fps_multiplier`, or be determined by `fps` directly (the former has higher priority). Examples:

The frame rate of output video is determined by the frame rate of input video and `fps_multiplier`：

```shell
python demo/video_interpolation_demo.py \
    configs/cain/cain_g1b32_1xb5_vimeo90k-triplet.py \
    https://download.openmmlab.com/mmediting/video_interpolators/cain/cain_b5_320k_vimeo-triple_20220117-647f3de2.pth \
    tests/data/frames/test_inference.mp4 \
    tests/data/frames/test_inference_vfi_out.mp4 \
    --fps-multiplier 2.0
```

The frame rate of output video is determined by `fps`:

```shell
python demo/video_interpolation_demo.py \
    configs/cain/cain_g1b32_1xb5_vimeo90k-triplet.py \
    https://download.openmmlab.com/mmediting/video_interpolators/cain/cain_b5_320k_vimeo-triple_20220117-647f3de2.pth \
    tests/data/frames/test_inference.mp4 \
    tests/data/frames/test_inference_vfi_out.mp4 \
    --fps 60.0
```

#### Generation

```shell
python demo/generation_demo.py \
    ${CONFIG_FILE} \
    ${CHECKPOINT_FILE} \
    ${IMAGE_FILE} \
    ${SAVE_FILE} \
    [--unpaired-path ${UNPAIRED_IMAGE_FILE}] \
    [--imshow] \
    [--device ${GPU_ID}]
```

If `--unpaired-path` is specified (used for CycleGAN), the model will perform unpaired image-to-image translation. If `--imshow` is specified, the demo will also show image with opencv. Examples:

Paired:

```shell
python demo/generation_demo.py \
    configs/example_config.py \
    work_dirs/example_exp/example_model_20200202.pth \
    demo/demo.jpg \
    demo/demo_out.jpg
```

Unpaired (also show image with opencv):

```shell
python demo/generation_demo.py 、
    configs/example_config.py \
    work_dirs/example_exp/example_model_20200202.pth \
    demo/demo.jpg \
    demo/demo_out.jpg \
    --unpaired-path demo/demo_unpaired.jpg \
    --imshow
```