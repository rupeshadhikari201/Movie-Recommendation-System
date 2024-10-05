# Removing Large Files from Git Tracking

## Introduction

This guide explains how to remove large files from Git tracking, ensuring that files like `similarity.pkl` and `movie_list.pkl` are not pushed to the repository. This is useful when dealing with files that exceed GitHub's file size limit or files that are generated during runtime and are not necessary for version control.

## Steps to Remove Large Files from Git Tracking

### 1. Add Files to `.gitignore`

To prevent Git from tracking the large files in the future:

* Open (or create) the `.gitignore` file in the root of your repository.
* Add the following lines:
```markdown
similarity.pkl
movie_list.pkl
```
### 2. Remove Files from Git's Tracking History

Even if you've added the files to `.gitignore`, they may still be tracked in previous commits. To stop Git from tracking these files without deleting them locally:

```bash
git rm --cached similarity.pkl movie_list.pkl
```
### Why This Happens

* Tracked Files in History: Once a file has been committed to Git, it stays in the Git history even if it's later deleted or added to `.gitignore`.
* File Too Large for GitHub: GitHub blocks files larger than 100MB from being pushed.

### Solution

Remove the large file from Git's history, then push the changes to the remote repository.

## Steps to Remove Large Files from Git History

### Add files to `.gitignore`

```bash
echo 'similarity.pkl' >> .gitignore
echo 'movie_list.pkl' >> .gitignore
```
### Remove files from Git's tracking

```bash
git rm --cached similarity.pkl
git rm --cached movie_list.pkl
```
### Commit the changes

```bash
git commit -m "Remove large files from tracking"
```
### Remove large files from Git history

Use either `git filter-repo` or `BFG Repo-Cleaner`:

#### Using `BFG Repo-Cleaner`

* Install `BFG Repo-Cleaner`
* Run:
```bash
bfg --delete-files similarity.pkl
bfg --delete-files movie_list.pkl
```
#### Using `Git Filter-Repo`

* Install `Git Filter-Repo`
* Run:
```bash
git filter-repo --path similarity.pkl --invert-paths
git filter-repo --path movie_list.pkl --invert-paths
```
### Push the changes

```bash
git push origin main --force
```
## Important Notes

* Force pushing rewrites Git history. Use caution if others are working on the repository.
* Back up your repository before rewriting Git history.

## Viewing Tracked Files After Commit

### 1. View Tracked Files (Working Directory)

```bash
git ls-files
```
### 2. View All Files in the Last Commit

```bash
git show --name-only
```
### 3. View Files in Any Commit

```bash
git show --name-only <commit_hash>
```
Replace `<commit_hash>` with the specific commit hash.

### 4. Check Status of Tracked Files (After a Commit)

```bash
git status
```
This shows the state of your working directory, including modified or untracked files.