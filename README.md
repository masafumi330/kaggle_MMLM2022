![スクリーンショット 2022-02-26 23 29 38](https://user-images.githubusercontent.com/51906769/155846831-e8368170-244b-442e-a3aa-ca446a474acf.png)
# [kaggle_MMLM2022](https://www.kaggle.com/c/mens-march-mania-2022/overview)
kaggle "MMLM 2022" competition repo.

## To-Do

- なにしよう？


## Setup

### Installation

```console
$ git clone --recursive https://github.com/masafumi330/kaggle_MMLM2022.git
$ sudo apt install cmake build-essential libboost-dev libboost-filesystem-dev
$ cd {your_name}/LightGBM
$ mkdir build && cd build
$ cmake -DUSE_GPU=1 -DOpenCL_LIBRARY=/usr/local/cuda/lib64/libOpenCL.so -DOpenCL_INCLUDE_DIR=/usr/local/cuda/include/ ..
$ make -j7
$ cd ../python-package
$ sudo python setup.py install --precompile --gpu --cuda

```

### Download Data

```bash
pip install --user kaggle
# check if ~/.local/bin exists in $PATH
cd {your_name}/workspace
mkdir data && cd data
kaggle competitions download -c mens-march-mania-2022
unzip mens-march-mania-2022.zip

mkdir -p /workspace/{your_name}/data/ncaa-men-538-team-ratings
cd /workspace/{your_name}/data/ncaa-men-538-team-ratings
touch 538ratingsMen.csv
# write https://www.kaggle.com/raddar/ncaa-men-538-team-ratings?select=538ratingsMen.csv 
# into 538ratingsMen.csv
```

## Submission

| score | issues | who | note |
| :---: | :---: | :---: | :--- |
| 0.54140 | [#10](https://github.com/masafumi330/kaggle_MMLM2022/issues/10) | Mine | not use strategy |
| 0.56574 | [#10](https://github.com/masafumi330/kaggle_MMLM2022/issues/10) | Mine | safe strategy |
| 0.65036 | [#10](https://github.com/masafumi330/kaggle_MMLM2022/issues/10) | Mine | risky strategy |

