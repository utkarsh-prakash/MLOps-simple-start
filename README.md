Create env
```bash
conda create -n mlops_simple_app python=3.7 -y
```
Activate env
```bash
conda activate mlops_simple_app
```
Create a requirement file.

Install the requirements
```bash
pip install -r requirements.txt
```

Download the data from
https://drive.google.com/drive/folders/18zqQiCJVgF7uzXgfbIJ-04zgz1ItNfF5?usp=sharing

git init

dvc init

dvc add data_given/winequality.csv

