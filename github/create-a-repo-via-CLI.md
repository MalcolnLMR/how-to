# Ubuntu
## Install dependencies
`sudo apt install git`

## Create local repository

1. `git init` 
2. `touch README.md` - create a readme file
3. `git add README.md`
4. `git commit -m "first commit"`

## Create remote repository

### Using GitHub CLI

1. `sudo apt install gh`
2. `gh auth login` - to login into your account
3. ```bash gh api --method POST \ 
    -H "Accept: application/vnd.github+json" \
    -H "X-GitHub-Api-Version> 2022-11-28" \
    /user/repos \
    -f "name=<REPO>" -f "description=<DESC>"
    ```

