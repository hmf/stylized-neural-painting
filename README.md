# Stylized Neural Painting 

[![Open in RunwayML Badge](https://open-app.runwayml.com/gh-badge.svg)](https://open-app.runwayml.com/?model=akhaliq/stylized-neural-painting)

[Preprint](<https://arxiv.org/abs/2011.08114>) | [Project Page](<https://jiupinjia.github.io/neuralpainter/>)  | [Colab Runtime 1](<https://colab.research.google.com/drive/1XwZ4VI12CX2v9561-WD5EJwoSTJPFBbr?usp=sharing/>)  | [Colab Runtime 2](<https://colab.research.google.com/drive/1ch_41GtcQNQT1NLOA21vQJ_rQOjjv9D8?usp=sharing/>)  | [Demo and Docker image on Replicate](https://replicate.ai/jiupinjia/stylized-neural-painting-oil)

## Official PyTorch implementation of the preprint paper "Stylized Neural Painting", accepted to CVPR 2021.

We propose an image-to-painting translation method that generates vivid and realistic painting artworks with controllable styles. Different from previous image-to-image translation methods that formulate the translation as pixel-wise prediction, we deal with such an artistic creation process in a vectorized environment and produce a sequence of physically meaningful stroke parameters that can be further used for rendering. Since a typical vector render is not differentiable, we design a novel neural renderer which imitates the behavior of the vector renderer and then frame the stroke prediction as a parameter searching process that maximizes the similarity between the input and the rendering output. Experiments show that the paintings generated by our method have a high degree of fidelity in both global appearance and local textures. Our method can be also jointly optimized with neural style transfer that further transfers visual style from other images.

![](./gallery/gif_teaser_1.gif)



In this repository, we implement the complete training/inference pipeline of our paper based on Pytorch and provide several demos that can be used for reproducing the results reported in our paper. With the code, you can also try on your own data by following the instructions below.

The implementation of the sinkhorn loss in our code is partially adapted from the project [SinkhornAutoDiff](https://github.com/gpeyre/SinkhornAutoDiff).



## License
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">  Stylized Neural Painting</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="http://www-personal.umich.edu/~zzhengxi/">Zhengxia Zou</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.




## One-min video result

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/oerb-nwrXhk/0.jpg)](https://www.youtube.com/watch?v=oerb-nwrXhk)



## **Updates on CPU mode (Nov 29, 2020)

PyTorch-CPU mode is now supported! You can try out on your local machine without any GPU cards.




## **Updates on lightweight renderers (Nov 26, 2020)

We have provided some lightweight renderers where users now can easily generate high resolution paintings with much more stroke details. With the lightweight renders, the rendering speed also improves a lot (x3 faster). This update also solves the out-of-memory problem when running our demo on a GPU card with limited memory (e.g. 4GB). 

Please check out the following for more details.



## Requirements

See [Requirements.txt](Requirements.txt).




## Setup

1. Clone this repo:

```bash
git clone https://github.com/jiupinjia/stylized-neural-painting.git 
cd stylized-neural-painting
```

2. Download one of the pretrained neural renderers from Google Drive (1. [oil-paint brush](https://drive.google.com/file/d/1sqWhgBKqaBJggl2A8sD1bLSq2_B1ScMG/view?usp=sharing), 2. [watercolor ink](https://drive.google.com/file/d/19Yrj15v9kHvWzkK9o_GSZtvQaJPmcRYQ/view?usp=sharing), 3. [marker pen](https://drive.google.com/file/d/1XsjncjlSdQh2dbZ3X1qf1M8pDc8GLbNy/view?usp=sharing), 4. [color tapes](https://drive.google.com/file/d/162ykmRX8TBGVRnJIof8NeqN7cuwwuzIF/view?usp=sharing)), and unzip them to the repo directory.

```bash
unzip checkpoints_G_oilpaintbrush.zip
unzip checkpoints_G_rectangle.zip
unzip checkpoints_G_markerpen.zip
unzip checkpoints_G_watercolor.zip
```


3. We have also provided some lightweight renderers where users can generate high-resolution paintings on their local machine  with limited GPU memory.  Please feel free to download and unzip them to your repo directory. (1. [oil-paint brush (lightweight)](https://drive.google.com/file/d/1kcXsx2nDF3b3ryYOwm3BjmfwET9lfFht/view?usp=sharing), 2. [watercolor ink (lightweight)](https://drive.google.com/file/d/1FoclmDOL6d1UT12-aCDwYMcXQKSK6IWA/view?usp=sharing), 3. [marker pen (lightweight)](https://drive.google.com/file/d/1pP99btR2XV3GtDHFXd8klpdQRSc0prLx/view?usp=sharing), 4. [color tapes (lightweight)](https://drive.google.com/file/d/1aHyc9ukObmCeaecs8o-a6p-SCjeKlvVZ/view?usp=sharing)).

```bash
unzip checkpoints_G_oilpaintbrush_light.zip
unzip checkpoints_G_rectangle_light.zip
unzip checkpoints_G_markerpen_light.zip
unzip checkpoints_G_watercolor_light.zip
```



## To produce our results

#### Photo to oil painting

![](./gallery/apple_oilpaintbrush.jpg)

- Progressive rendering

```bash
python demo_prog.py --img_path ./test_images/apple.jpg --canvas_color 'white' --max_m_strokes 500 --max_divide 5 --renderer oilpaintbrush --renderer_checkpoint_dir checkpoints_G_oilpaintbrush --net_G zou-fusion-net
```

- Progressive rendering with lightweight renderer (with lower GPU memory consumption and faster speed)

```bash
python demo_prog.py --img_path ./test_images/apple.jpg --canvas_color 'white' --max_m_strokes 500 --max_divide 5 --renderer oilpaintbrush --renderer_checkpoint_dir checkpoints_G_oilpaintbrush_light --net_G zou-fusion-net-light
```

- Rendering directly from mxm image grids

```bash
python demo.py --img_path ./test_images/apple.jpg --canvas_color 'white' --max_m_strokes 500 --m_grid 5 --renderer oilpaintbrush --renderer_checkpoint_dir checkpoints_G_oilpaintbrush --net_G zou-fusion-net
```


#### Photo to marker-pen painting

![](./gallery/diamond_markerpen.jpg)

- Progressive rendering

```bash
python demo_prog.py --img_path ./test_images/diamond.jpg --canvas_color 'black' --max_m_strokes 500 --max_divide 5 --renderer markerpen --renderer_checkpoint_dir checkpoints_G_markerpen --net_G zou-fusion-net
```

- Progressive rendering with lightweight renderer (with lower GPU memory consumption and faster speed)

```bash
python demo_prog.py --img_path ./test_images/diamond.jpg --canvas_color 'black' --max_m_strokes 500 --max_divide 5 --renderer markerpen --renderer_checkpoint_dir checkpoints_G_markerpen_light --net_G zou-fusion-net-light
```

- Rendering directly from mxm image grids

```bash
python demo.py --img_path ./test_images/diamond.jpg --canvas_color 'black' --max_m_strokes 500 --m_grid 5 --renderer markerpen --renderer_checkpoint_dir checkpoints_G_markerpen --net_G zou-fusion-net
```


#### Style transfer

![](./gallery/nst.jpg)

- First, you need to generate painting and save stroke parameters to output dir

```bash
python demo.py --img_path ./test_images/sunflowers.jpg --canvas_color 'white' --max_m_strokes 500 --m_grid 5 --renderer oilpaintbrush --renderer_checkpoint_dir checkpoints_G_oilpaintbrush --net_G zou-fusion-net --output_dir ./output
```

- Then, choose a style image and run style transfer on the generated stroke parameters

```bash
python demo_nst.py --renderer oilpaintbrush --vector_file ./output/sunflowers_strokes.npz --style_img_path ./style_images/fire.jpg --content_img_path ./test_images/sunflowers.jpg --canvas_color 'white' --net_G zou-fusion-net --renderer_checkpoint_dir checkpoints_G_oilpaintbrush --transfer_mode 1
```

You may also specify the --transfer_mode (0: transfer color only, 1: transfer both color and texture)

Also, please note that in the current version, the style transfer are not supported by the progressive rendering mode. We will be working on this feature in the near future. 


#### Generate 8-bit graphic artworks

![](./gallery/8bitart.jpg)

```bash
python demo_8bitart.py --img_path ./test_images/monalisa.jpg --canvas_color 'black' --max_m_strokes 300 --max_divide 4
```



## Running through SSH

If you would like to run remotely through ssh and do not have something like X-display installed, you will need --disable_preview to turn off cv2.imshow on the run.

```bash
python demo_prog.py --disable_preview
```



## Google Colab

Here we also provide a minimal working example of the inference runtime of our method. Check out the following runtimes and see your result on Colab.

[Colab Runtime 1](https://colab.research.google.com/drive/1XwZ4VI12CX2v9561-WD5EJwoSTJPFBbr?usp=sharing/) : Image to painting translation (progressive rendering)

[Colab Runtime 2](https://colab.research.google.com/drive/1ch_41GtcQNQT1NLOA21vQJ_rQOjjv9D8?usp=sharing/) : Image to painting translation with image style transfer 



## To retrain your neural renderer

You can also choose a brush type and train the stroke renderer from scratch. The only thing to do is to run the following common. During the training, the ground truth strokes are generated on-the-fly, so you don't need to download any external dataset. 

```bash
python train_imitator.py --renderer oilpaintbrush --net_G zou-fusion-net --checkpoint_dir ./checkpoints_G --vis_dir val_out --max_num_epochs 400 --lr 2e-4 --batch_size 64
```



## Citation

If you use our code for your research, please cite the following paper:

``````
@inproceedings{zou2020stylized,
    title={Stylized Neural Painting},
      author={Zhengxia Zou and Tianyang Shi and Shuang Qiu and Yi Yuan and Zhenwei Shi},
      year={2020},
      eprint={2011.08114},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
``````

# Local Changes


user@node:~/VSCodeProjects$ cd stylized-neural-painting/
user@node:~/VSCodeProjects/stylized-neural-painting$ git config user.name "Hugo Ferreira"
user@node:~/VSCodeProjects/stylized-neural-painting$ git config user.email user@inescporto.pt


[GDrive 3](https://github.com/glotlabs/gdrive)

1. https://stackoverflow.com/questions/25010369/wget-curl-large-file-from-google-drive/63781195#63781195
  1. https://github.com/wkentaro/gdown


https://drive.google.com/file/d/1sqWhgBKqaBJggl2A8sD1bLSq2_B1ScMG/view?usp=sharing
wget https://drive.google.com/file/d/1sqWhgBKqaBJggl2A8sD1bLSq2_B1ScMG/checkpoints_G_oilpaintbrush.zip

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1sqWhgBKqaBJggl2A8sD1bLSq2_B1ScMG' -O checkpoints_G_oilpaintbrush.zip

[gdown](https://github.com/wkentaro/gdown) already in the `Requirements.txt` file
open a bash session in the container
get link
gdown 1sqWhgBKqaBJggl2A8sD1bLSq2_B1ScMG -O checkpoints_G_oilpaintbrush.zip


Make sure [gdown](https://github.com/wkentaro/gdown) is installed. This can be installed via `pip`. It is already installed via the `Requirements.txt` file, so it is also available in the Dev container. 

Get the Google Drive download links of the pre-trained **neural renderers**:

1. [oil-paint brush](https://drive.google.com/file/d/1sqWhgBKqaBJggl2A8sD1bLSq2_B1ScMG/view?usp=sharing)
1. [watercolor ink](https://drive.google.com/file/d/19Yrj15v9kHvWzkK9o_GSZtvQaJPmcRYQ/view?usp=sharing)
1. [marker pen](https://drive.google.com/file/d/1XsjncjlSdQh2dbZ3X1qf1M8pDc8GLbNy/view?usp=sharing)
1. [color tapes](https://drive.google.com/file/d/162ykmRX8TBGVRnJIof8NeqN7cuwwuzIF/view?usp=sharing)

Extract the IDs from each link above:

1. oil-paint brush: `1sqWhgBKqaBJggl2A8sD1bLSq2_B1ScMG`
1. watercolor ink: `19Yrj15v9kHvWzkK9o_GSZtvQaJPmcRYQ`
1. marker pen: `1XsjncjlSdQh2dbZ3X1qf1M8pDc8GLbNy`
1. color tapes: `162ykmRX8TBGVRnJIof8NeqN7cuwwuzIF`

Download the files with `gdown`:

1. `gdown 1sqWhgBKqaBJggl2A8sD1bLSq2_B1ScMG -O checkpoints_G_oilpaintbrush.zip`
1. `gdown 19Yrj15v9kHvWzkK9o_GSZtvQaJPmcRYQ -O checkpoints_G_rectangle.zip`
1. `gdown 1XsjncjlSdQh2dbZ3X1qf1M8pDc8GLbNy -O checkpoints_G_markerpen.zip`
1. `gdown 162ykmRX8TBGVRnJIof8NeqN7cuwwuzIF -O checkpoints_G_watercolor.zip`

Here is an example:

<!--- cSpell:disable --->
```shell
vscode ➜ /workspaces/stylized-neural-painting (try_out_1) $ gdown 1sqWhgBKqaBJggl2A8sD1bLSq2_B1ScMG -O checkpoints_G_oilpaintbrush.zip
Downloading...
From (uriginal): https://drive.google.com/uc?id=1sqWhgBKqaBJggl2A8sD1bLSq2_B1ScMG
From (redirected): https://drive.google.com/uc?id=1sqWhgBKqaBJggl2A8sD1bLSq2_B1ScMG&confirm=t&uuid=1cc0579a-564c-4e26-9fce-e14b2933c261
To: /workspaces/stylized-neural-painting/checkpoints_G_oilpaintbrush.zip
100%|█████████████████████████████████████████████████████████████████████████████████████████████| 181M/181M [00:06<00:00, 27.3MB/s]
```
<!--- cSpell:enable --->

You can confirm the content of the archives with `zipinfo` Here is an example:

<!--- cSpell:disable --->
```shell
hmf@cc-cese-52:~/VSCodeProjects/stylized-neural-painting$ zipinfo ./checkpoints_G_oilpaintbrush.zip 
Archive:  ./checkpoints_G_oilpaintbrush.zip
Zip file size: 181018148 bytes, number of entries: 2
drwx---     3.1 fat        0 bx stor 20-Nov-16 19:21 checkpoints_G_oilpaintbrush/
-rw-a--     3.1 fat 217149268 bx defN 20-Nov-03 14:19 checkpoints_G_oilpaintbrush/last_ckpt.pt
2 files, 217149268 bytes uncompressed, 181017766 bytes compressed:  16.6%
```
<!--- cSpell:enable --->

Unzip the contents:

1. `unzip checkpoints_G_oilpaintbrush.zip`
1. `unzip checkpoints_G_rectangle.zip`
1. `unzip checkpoints_G_markerpen.zip`
1. `unzip checkpoints_G_watercolor.zip`

Here is an example:

<!--- cSpell:disable --->
```shell
vscode ➜ /workspaces/stylized-neural-painting (try_out_1) $ unzip checkpoints_G_watercolor.zip
Archive:  checkpoints_G_watercolor.zip
   creating: checkpoints_G_rectangle/
  inflating: checkpoints_G_rectangle/last_ckpt.pt  
```
<!--- cSpell:enable --->

Make sure these files are not added to the Git repository by adding these lines to your `.gitignore`:

```
checkpoints_G_oilpaintbrush.zip
checkpoints_G_rectangle.zip
checkpoints_G_markerpen.zip
checkpoints_G_watercolor.zip
checkpoints_G_oilpaintbrush
checkpoints_G_rectangle
checkpoints_G_markerpen
checkpoints_G_watercolor
```

Get the Google Drive download links of the pre-trained **lightweight neural renderers**:

1. [oil-paint brush (lightweight)](https://drive.google.com/file/d/1kcXsx2nDF3b3ryYOwm3BjmfwET9lfFht/view?usp=sharing)
1. [watercolor ink (lightweight)](https://drive.google.com/file/d/1FoclmDOL6d1UT12-aCDwYMcXQKSK6IWA/view?usp=sharing)
1. [marker pen (lightweight)](https://drive.google.com/file/d/1pP99btR2XV3GtDHFXd8klpdQRSc0prLx/view?usp=sharing)
1. [color tapes (lightweight)](https://drive.google.com/file/d/1aHyc9ukObmCeaecs8o-a6p-SCjeKlvVZ/view?usp=sharing)


Extract the IDs from each link above:

1. oil-paint brush (lightweight):`1kcXsx2nDF3b3ryYOwm3BjmfwET9lfFht`
1. watercolor ink (lightweight): `1FoclmDOL6d1UT12-aCDwYMcXQKSK6IWA`
1. marker pen (lightweight):     `1pP99btR2XV3GtDHFXd8klpdQRSc0prLx`
1. color tapes (lightweight):    `1aHyc9ukObmCeaecs8o-a6p-SCjeKlvVZ`

Download the files with `gdown`:

1. `gdown 1kcXsx2nDF3b3ryYOwm3BjmfwET9lfFht -O checkpoints_G_oilpaintbrush_light.zip`
1. `gdown 1FoclmDOL6d1UT12-aCDwYMcXQKSK6IWA -O checkpoints_G_rectangle_light.zip`
1. `gdown 1pP99btR2XV3GtDHFXd8klpdQRSc0prLx -O checkpoints_G_markerpen_light.zip`
1. `gdown 1aHyc9ukObmCeaecs8o-a6p-SCjeKlvVZ -O checkpoints_G_watercolor_light.zip`


Unzip the contents:

1. `unzip checkpoints_G_oilpaintbrush_light.zip`
1. `unzip checkpoints_G_rectangle_light.zip`
1. `unzip checkpoints_G_markerpen_light.zip`
1. `unzip checkpoints_G_watercolor_light.zip`

Make sure these files are not added to the Git repository by adding these lines to your `.gitignore`:

```
```



<!--- cSpell:disable --->
```shell
```
<!--- cSpell:enable --->


----------------

<details close>
<summary>Output</summary>
<br>
</details>

<!--- cSpell:disable -->
<details close>
<summary>Output:</summary>
<br>

```shell
TODO
```

</details>
<!--- cSpell:enable -->



<!--- cSpell:disable --->
```shell
```
<!--- cSpell:enable --->


<!--- cSpell:disable --->
```shell
```
<!--- cSpell:enable --->


<!--- cSpell:disable --->
```shell
```
<!--- cSpell:enable --->


<!--- cSpell:disable -->
<details close>
<summary>Output:</summary>
<br>

```shell
TODO
```

</details>
<!--- cSpell:enable -->

<!--- cSpell:disable -->
<details close>
<summary>Output:</summary>
<br>

```shell
TODO
```

</details>
<!--- cSpell:enable -->

<!--- cSpell:disable --->
```shell
```
<!--- cSpell:enable --->









