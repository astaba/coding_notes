# CODING NOTES

## Merging repositories

Let's say we have to repositories named `backend` and `frontend` with unrelated histories:

1. **Clone the repos**  
To provide for potential glitch, clone both repos where you see fit.  

```sh
git clone --no-local /to_be_merged_repo_path/frontend
git clone --no-local /to_be_merged_repo_path/backend
```

2. **Filter them with `git-filter-repo`**  
Since git source control do not keep track of the name of repo root directory, in case your merging endeavour would like to keep original repos directory structure, this program is required.  
**`git-filter-repo`** is not a git-builtin program. Copy it from **[git-filter-repo Github repo](https://github.com/newren/git-filter-repo)**. Set up its **Python script** with **shbang** for python interpreter (previously installed), executable credential (`chmod +x`), and add it to some `bin/` folder visible from the `$PATH`(`/home` one is recommanded)  

```sh
cd /to_clone_repo_path/frontend/
git filter-repo --to-subdirectory-filter frontend

cd /to_clone_repo_path/backend/
git filter-repo --to-subdirectory-filter backend
```

3. **Create merging repo**  

```sh
cd /to_new_merging_repo_path/
mkdir merging-repo && cd merging-repo
git init
```

4. **Add remotes to fetch data**  

```sh
git remote add frontend-origin  /to_clone_repo_path/frontend/
git remote add backend-origin  /to_clone_repo_path/backend/

git fetch frontend-origin --tags
git fetch backend-origin --tags
```

5. **Finally you merge**  

```sh
git merge --allow-unrelated-histories frontend-origin/main-branch
git merge --allow-unrelated-histories backend-origin/main-branch

git remote remove frontend-origin
git remote remove backend-origin
```