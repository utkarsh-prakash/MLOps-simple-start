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

.github>workflows>ci-cd.yaml
Using this template we can automate ci-cd pipeline on github
- On every push we can plan to run tests and syntax check.
- For this github will create an environment and install everything to execute the tests
- We can also add the commands to deploy the changes on Heroku, for this we will have to add heroku app name and api on github secrets.

Heroku Deployment
- With app.py and Procfile we are ready for deployment on heroku.
- Create a app and select github connect.
- On connect, opt for automatic deployements and tick "wait for ci checks"
- As github workflows is set, the heroku app will get updated on every push to github repo.

<b> main_mlflow Branch </b>
```bash
git checkout -b main_mlflow
```
shifting to another branch to implement mlflow for model registry/model tracking
- Create an artifacts folder
- We need to run a local server to register a model, without this server running model will be logged locally but we can't mark it as production/ staged model.
mlflow server command
```bash
mlflow server --backend-store-uri sqlite:///mlflow.db --default-artifact-root ./artifacts --host 127.0.0.1 -p 1234
```

Mlflow + Model deployment
- Train and evaluate is part of dvc.yaml and will run through dvc repro
- Only difference is that now every running version will be logged by mlflow
- Using log_production_model.py we can get the run with lowest mae
- We have to just take this model and dump it in prediction_service
- As log_production_model.py is part of dvc.yaml pipeline, dvc repro will automatically store the model in prediction service with best mae after each run.

Update the github workflow to deploy the model on push on main_mlflow branch...and we are good to go :)