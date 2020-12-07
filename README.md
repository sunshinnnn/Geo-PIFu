# Geo-PIFu: Geometry and Pixel Aligned Implicit Functions for Single-view Human Reconstruction

This repository is the official PyTorch implementation of [Geo-PIFu: Geometry and Pixel Aligned Implicit Functions for Single-view Human Reconstruction](https://papers.nips.cc/paper/2020/file/690f44c8c2b7ded579d01abe8fdb6110-Paper.pdf), NeurIPS, 2020.
<p align="center">
<img src="https://github.com/simpleig/Geo-PIFu/blob/master/assests/pipeline.png" width="750">
</p>

If you find this code useful, please consider citing
```
@inproceedings{he2020geopifu,
  title     = {Geo-PIFu: Geometry and Pixel Aligned Implicit Functions for Single-view Human Reconstruction},
  author    = {Tong He and John Collomosse and Hailin Jin and Stefano Soatto},
  booktitle = {Conference on Neural Information Processing Systems (NeurIPS)},
  year      = {2020}
}
```

Development log
- Environment configuration
  - [x] For `Geo-PIFu` and `PIFu` training, test and evaluation
  - [ ] For `DeepHuman` data rendering
- Data preparation
  - [x] DeepHuman and LSUN datasets setup
  - [ ] Clothed human mesh rendering
- Training
  - [ ] Train `PIFu` on the DeepHuman training dataset
  - [ ] Train `Geo-PIFu` on the DeepHuman training dataset
- Test
  - [ ] Test `PIFu` on the DeepHuman test dataset
  - [ ] Test `Geo-PIFu` on the DeepHuman test dataset
- Evaulation
  - [ ] Compute 4 metrics: CD, PSD, Normal Cosine, Normal L2

## Requirements

We provide a conda `yaml` environment file.

	conda env create -f geopifu_requirements.yaml
	conda activate geopifu

## Dataset

Download the `DeepHuman` mesh dataset from [here](https://github.com/ZhengZerong/DeepHuman/tree/master/THUmanDataset) and move it into your preferred data folder. The downloaded data should be a zip file about 30.5 G.

	mv DeepHumanDataset.zip data/DeepHumanDataset/

Download the `LSUN` background image dataset and unzip the `zip` files.

	cd data/LSUN
	python3 Geo-PIFu/LSUN/download.py
	unzip "*.zip"

Parse the `lmdb` files into jpeg images. You might need to `pip install lmdb`.

	python Geo-PIFu/LSUN/data.py export ./*_val_lmdb --out_dir ./val_jpg --imageType jpg
	python Geo-PIFu/LSUN/data.py export ./*_train_lmdb --out_dir ./train_jpg --imageType jpg --limit 30000

## Human Mesh Rendering

You can specify `splitNum` and `splitIdx` (e.g. {0, ..., `splitNum`-1}) in order to split the full data and launch multiple rendering scripts in parallel. For example, with 30 splits the whole rendering process will take about 37 hrs and generate 1.4T rendered data. The rendering script generates various items besides the color human images: `config`, `rgbImage`, `normalRGB`, `maskImage`, `meshVoxels`, `maskFromVoxels`, `meshSem`, `skeleton3D`, `skeleton2D`, `smplSem`, `smplIUV`, `smplSemVoxels`. Not all the items are needed/used in this project. But they could be very useful for other relevant tasks. Please read `render_mesh.py` for detailed explanation of each item and modify the rendering script accordingly to fit your need.

	python render_mesh.py --meshDirSearch ${PREFERRED_DATA_FOLDER}/data --bgDirSearch ${PREFERRED_DATA_FOLDER}/data --saveDir ${PREFERRED_DATA_FOLDER}/data/humanRender --resolutionScale 4 --splitNum 30 --splitIdx 0
	python render_mesh.py --meshDirSearch ${PREFERRED_DATA_FOLDER}/data --bgDirSearch ${PREFERRED_DATA_FOLDER}/data --saveDir ${PREFERRED_DATA_FOLDER}/data/humanRender --resolutionScale 4 --splitNum 30 --splitIdx 1
	...

## Acknowledgements

This repository is built on: [DeepHuman](https://github.com/ZhengZerong/DeepHuman) and [PIFu](https://github.com/shunsukesaito/PIFu). Thank the authors for sharing their code!