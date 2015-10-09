# Script to synchronize two Git repositories 

## Download

```
mkdir <synchronization folder>
curl https://github.com/adidik/git-repos-sync/blob/master/git-repos-sync
chmod +x git-repos-sync
```

## Usage

Just run the script with 3 parameters:
* URL of first repository (left).
* URL of secod repository (right).
* Branch to sync up.

```
./git-repos-sync <URL> <URL> <branch>
```

On first run script will init Git repository in  script's folder and add left and right repositories as remotes. Then first attempt to sync specified branch will be made.

In case if synchronization is not possible because of merge conflict, resolve it manually and just rerun script. It will automatically try to continue syncronization.
 
## How does it work

### Already synced up

Initial:

'''
Left repo:  A-B-C <- branch

Sync repo:  A-B-C <-  branch, left/branch, right/branch

Right repo: A-B-C <-branch
'''

Fetch: 








