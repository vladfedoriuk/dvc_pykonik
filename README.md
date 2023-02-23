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
- Checkout the `data.dvc` file one commit before:
```bash
git checkout HEAD~1 data.dvc
```
- Show data directory - you will notice that the data didn't change:
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
- Checkout the `data.dvc` one commit ahead:
```bash
git checkout <commit hash> data.dvc
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

- We can easily access data stored on some remote repository
```bash
dvc list https://github.com/iterative/dataset-registry get-started
```

- Also, we can import some DVC tracked data from external project
```bash
 dvc import https://github.com/iterative/dataset-registry              get-started/data.xml -o data.xml
```
   - Notice some differences between `data.xml.dvc` and `.dvc` files created so far.
   - This allows us to bring in changes from the data source later using `dvc update`

- Let's track those data:
```bash
git add .gitignore data.xml.dvc
git commit -m "Track external data"
```
- Let's update the data
```bash
dvc update data.xml.dvc
```