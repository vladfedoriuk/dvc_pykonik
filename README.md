 DVC repo init:
```bash
dvc init
# The .dvc directory will be created
```

- Add some data to the repo:
```bash
mkdir data
# Add a large file to the data directory
ls -lh data
```

- Add the data to the dvc repo:
```bash
dvc add data
# The data.dvc file will be created
# The data.dvc file contains the hash of the data
# The .dvc/cache is a content-addressable storage for the files
```

- Add the data to the git repo:
```bash
# We need to stage the respective DVC artifacts
git add data.dvc .gitignore
git commit -m "Add data to the repo"
```
- Now you will notice that the `data/` directory stores a link to the actual data.
```bash
ls -i data
```
### Remote Storage

- Add a remote storage:
```bash
dvc remote add -d storage gdrive://<folder-id>
# -d flag sets the default remote storage
# The .dvc/config file will be updated
```
- Commit the changes:
```bash
git add .dvc/config
git commit -m "Add remote storage"
```
- Push the data to the remote storage:
```bash
dvc push
```
- Let's remove the data from the local repo:
```bash
rm -rf data
# Remove the data from the dvc cache
rm -rf .dvc/cache
```
- Pull the data from the remote storage:
```bash
dvc pull
```
- Let's modify the data slightly via `exploration.ipynb`:
```bash
ls -lh data
```
- Add the altered data to the dvc repo:
```bash
dvc add data
```
- Add the new file to the git repo:
```bash
git add data.dvc
git commit -m "Add altered data to the repo"
```
- Push the new file to the remote storage:
```bash
dvc push
```
- History of commits
```bash
git log --oneline
```
- Checkout one commit before:
```bash
git checkout HEAD~1
```
- Show data directory:
```bash
ls -lh data
```
- Do the dvc checkout:
```bash
dvc checkout
```
- Show data directory:
```bash
ls -lh data
```
- Checkout one commit ahead:
```bash
git checkout master
```
- Log the commits
```bash
git log --oneline
```
- Do the dvc checkout:
```bash
dvc checkout
```
- Show data directory:
```bash
ls -lh data
```

### Using DVC as data registry

- List the data files from the remote repository
```bash
dvc list https://github.com/vladfedoriuk/dvc_showcase
```
- [Data access docs](https://dvc.org/doc/start/data-management/data-and-model-access)

- Import the data from the remote repository
```bash
dvc import https://github.com/vladfedoriuk/dvc_showcase  -o data_1/
# To import files, we must be inside the DVC repo
```

- Update the imported files
```bash
dvc update data_1.dvc
# We can always checkout the newest version of the data
```

- [Python API docs](https://dvc.org/doc/api-reference)