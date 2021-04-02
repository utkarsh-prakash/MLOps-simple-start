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

Add remote storage for all the tracked data.
```bash
dvc remote add -d storage gdrive://<DRIVE ID>
git add .dvc/config && git commit -m "Configure remote storage"
```
- Here the drive ID is last part of the google drive link.

Pushing the data to remote storage
```bash
dvc push
```
- This step will ask you to authenticate yourself by clicking on the link which will appear in the terminal.
- Once you allow dvc to read and write on gdrive it'll give an access token which you'll paste in the terminal.
- Now the copy of your data will be pushed to the gdrive
- Above step will create a gdrive credential file
- Find this credentials in the given path -
```bash
.dvc >> temp >> gdrive-user-credentials.json
```
- Now to add the secrets in your github repo -
    - Go to settings
    - secrets
    - Click on add secrets
    - Give name of secretes
    - Paste the json file content from ```gdrive-user-credentials.json ```

To retrieve data from remote in future.
```bash
dvc pull
```

To list all the files being tracked by dvc
```bash
dvc list --dvc-only -R .dvc
```

DVC is tracking all the parameters and metrics which we have declared in dvc.yaml
```bash
dvc params show
dvc metrics show
```
These commands will show which params and metrics are being tracked and what are the values.
```bash
dvc params diff
dvc metrics diff
```
When we update the params and rerun with dvc repro, we will get new metrics. Above mentioned commands will give us the old and new values of these parameters and Metrics.
- After a run, dvc.lock is modified and thus we need to dvc push to update the remote storage.
