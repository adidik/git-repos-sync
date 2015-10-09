# Script to synchronize two Git repositories 

## Download

```
mkdir <synchronization-folder> && cd <synchronization-folder>
wget https://raw.githubusercontent.com/adidik/git-repos-sync/master/git-repos-sync
chmod +x git-repos-sync
```
or

```
mkdir <synchronization-folder> && cd <synchronization-folder>
curl -O https://raw.githubusercontent.com/adidik/git-repos-sync/master/git-repos-sync
chmod +x git-repos-sync
```

## Usage

Just run the script with 3 parameters:
* URL of first repository (left).
* URL of second repository (right).
* Branch to sync up.

```
./git-repos-sync <URL> <URL> <branch>
```

Both repositories must be accessble on server you execute the script.


On first run script will init Git repository in  script's folder and add left and right repositories as remotes. Then first attempt to sync specified branch will be made.

In case if synchronization is not possible because of merge conflict, resolve it manually and just rerun script. It will automatically try to continue syncronization.

In case if sync repo has uncommitted modifications, script will exit with warning.
 
## How does it work

Script makes next actions to sync up two repos:

1. Fetch specified branch from left repo and right repo.
2. Create sync branch based on right repo branch.
3. Merge left repo branch, if merge conflict, stop for manual resolution.
4. Push sync branch to right repo.
5. Push sync branch to left repo.
6. Remove sync branch.

In case of merge conflict, then:

1. Wait while conflict will be solved manually and commited.
2. Fetch specified branch from left repo and right repo.
3. Merge sync branch with right repo branch, if merge conflict, stop for manual resolution.
4. Merge left repo branch, if merge conflict, stop for manual resolution.
5. Push sync branch to right repo.
6. Push sync branch to left repo.
7. Remove sync branch.


See below different cases.

### Already synced up

Initial:

```
Left repo:  A-B-C <- branch

Sync repo:  A-B-C <-  left/branch, right/branch

Right repo: A-B-C <-branch
```

Fetch: 
```
Left repo:  A-B-C <- branch

Sync repo:  A-B-C <-  sync-branch, left/branch, right/branch

Right repo: A-B-C <-branch
```

Merge: 

```
Left repo:  A-B-C <- branch

Sync repo:  A-B-C <-  sync-branch, left/branch, right/branch

Right repo: A-B-C <-branch
```

Push and remove sync branch:
```
Left repo:  A-B-C <- branch

Sync repo:  A-B-C <-  left/branch, right/branch

Right repo: A-B-C <-branch
```

### Left ahead

Initial:

```
Left repo:  A-B-C-D <- branch

Sync repo:  A-B-C <-  left/branch, right/branch

Right repo: A-B-C <-branch
```

Fetch: 
```
Left repo:  A-B-C-D <- branch

Sync repo:  A-B-C-D <-  left/branch
				^-- sync-branch, right/branch		

Right repo: A-B-C <-branch
```

Merge: 

```
Left repo:  A-B-C-D <- branch

Sync repo:  A-B-C-D <-  sync-branch, left/branch
				^-- right/branch

Right repo: A-B-C <-branch
```

Push and remove sync branch:
```
Left repo:  A-B-C-D <- branch

Sync repo:  A-B-C-D <-  left/branch, right/branch

Right repo: A-B-C-D <-branch
```

### Right ahead

Initial:

```
Left repo:  A-B-C <- branch

Sync repo:  A-B-C <-  left/branch, right/branch

Right repo: A-B-C-D <-branch
```

Fetch: 
```
Left repo:  A-B-C <- branch

Sync repo:  A-B-C-D <-  sync-branch, right/branch
				^-- left/branch		

Right repo: A-B-C-D <-branch
```

Merge: 

```
Left repo:  A-B-C <- branch

Sync repo:  A-B-C-D <- sync-branch, right/branch
				^-- left/branch

Right repo: A-B-C-D <-branch
```

Push and remove sync branch:
```
Left repo:  A-B-C-D <- branch

Sync repo:  A-B-C-D <- left/branch, right/branch

Right repo: A-B-C-D <-branch
```

### Diverged

Initial:

```
Left repo:  A-B-C-D <- branch

Sync repo:  A-B-C <- left/branch, right/branch

Right repo: A-B-C-E <-branch
```

Fetch: 
```
Left repo:  A-B-C-D <- branch

Sync repo:  A-B-C-E <- sync-branch, right/branch
				'-D <- left/branch		

Right repo: A-B-C-E <-branch
```

Merge: 

```
Left repo:  A-B-C-D <- branch

                   ,- right/branch
Sync repo:  A-B-C-E-F <-  sync-branch
				'-D-' <- left/branch	

Right repo: A-B-C-E <-branch
```

Push and remove sync branch:
```
Left repo:  A-B-C-E-F <- branch
				'-D-' 

                   
Sync repo:  A-B-C-E-F <- left/branch, right/branch
				'-D-' 

Right repo: A-B-C-E-F <- branch
				'-D-' 
```

# License

The MIT License (MIT)

Copyright (c) 2015 Aleksey Didik

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

