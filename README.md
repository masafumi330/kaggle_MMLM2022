![スクリーンショット 2022-02-26 23 29 38](https://user-images.githubusercontent.com/51906769/155846831-e8368170-244b-442e-a3aa-ca446a474acf.png)
# [kaggle_MMLM2022](https://www.kaggle.com/c/mens-march-mania-2022/overview)
kaggle "MMLM 2022" competition repo.

## To-Do

- なにしよう？


## Setup

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