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

```bash
git init
```

```bash
dvc init
```

```bash
dvc add data_given/winequality.csv
```

```bash
git add .
```

```bash
git commit -m "first commit"
```

```bash
git remote add origin git@github.com:utkarsh-prakash/MLOps-simple-start.git
```

Renaming local branch as main
```bash
git branch -M main
```

```bash
git push origin main
```
- dvc.yaml file tells dvc the pipeline it has to follow.
- once we run the pipeline, dvc will run the "cmd" if there is a change in dependency "deps" and will store the output in "outs"
- When we run the dvc stage, it creeates dvc.lock which tracks all the dependencies. A stage will only run if its dependency changes, if there is no change, dvc will skip the stage.
```bash
dvc repro
```