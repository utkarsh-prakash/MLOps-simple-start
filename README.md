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

pytest
```bash
pytest -v
```
- This command will run each file and each function under tests folder.
- Convension : file name should start with test_ and function name should start with test_
- Test will pass if the fuction assert True.
- Also
```bash
with pytest.raises(NotInRange):
```
If the codelines under this with statement raises NotInRange exception, testcase will pass, If it raises any other error or even if it asserts True, testcase will fail.

tox<br>
tox can be used to set end to end environment and run our codes on the environment. For running tox, we will have to create tox.ini file.
```bash
[tox]
envlist = py37
```
- We set up a python 3.7 environment using this command
- for testing multiple environments : envlist = py27, py36
```bash
skipsdist = True
```
- This is set to true if we have not converted our folder to a package or have NOT created setup.py file<br>
setup.py
- By creating this file we can convert the folders with __init__.py as a package.
- In the setup.py file the folder name should be given under "name". The folder should have __init__.py
- Now we can install this folder as a package to any environment using the below given command.
```bash
pip install -e .
```
- If we want to make own package with tar and wheel
```bash
python setup.py sdist bdist_wheel
```
- If we have the setup file we can run without skipsdist
```bash
[testenv]
deps = -rrequirements.txt
commands = 
    pytest -v
```
- -rrequirements.txt will recursively read the required packages.
- For running a tox file.
```bash
tox
```
- There will be one time tox environment setup
- In case there is update in requirement file and tox environment setup need update, we can run rebuilding
```bash
tox -r
```  
- We can also add some flake8 commands in tox test environment to ensure that there is no syntax error or unused variable names.
