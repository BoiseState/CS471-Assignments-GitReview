# Exercise: Git Review

## Step 1
Familiarize yourself with the repository by checking the repository history, file structure, file contents, etc.

## Step 2
Read and understand the contents of the `./chaos_monkey.sh` script which will create an intricate repository structure with different branches, but without creating any merge commits.

Each branch will make changes in a separate file, thus avoiding any merge conflicts during future rebasing/merging operations.

The diagram below illustrates the conceptual structure of the repository and emphasizes the chronological order in which the commits were added to the multiple branches.
```
                                                                                                    (branch6)
                                                                                                        |
             (branch1)                                          (branch4)                  /---------- C18 --------
                 |                                                  |                     /
        /------- C3 ----                     /-------------------- C12 ----------------- C16 -- C17 ---------------
       /                                    /
C0 -- C1 -- C2 --------------------------- C8 -------------- C11 ------------------------------------------- C19 --  //main
             \
              \----- C4 ------ C6 ------------- C9 ------------------- C13
                     |          \                                          \
                 (branch2)       \-- C7 -------------                       \ ---- C15 ----------------------------
                                     |                                              |
                                 (branch3)                                      (branch5)
```

## Step 3
Run the `./chaos_monkey.sh` script.

## Step 4
Run the following command to view the branch and commit history of the repository, where the current checked out branch is `main`:
```bash
$ git log --oneline --decorate --graph --all
```
Compare the output with the diagram from `Step 2`.

To focus only on a subset of branches, run:
```bash
$ git log --oneline --decorate --graph branch1 branch4 branch6
$ git log --oneline --decorate --graph branch2 branch3 branch5
```
Compare the output with the diagram from `Step 2`.

## Step 5
Rebase `branch3` on top of `branch2`.

First, checkout the branch that is going to be rebased.
```bash
$ git checkout branch3
```

Second, "zoom" into the two branches to understand better their relationship and interaction.
```bash
$ git log --oneline --decorate --graph branch3 branch2
```

Third, run the following command to conclude the rebasing.
```bash
$ git rebase branch2
```

Next, analyze the result of the rebasing operation.
```bash
$ git log --oneline --decorate --graph branch3 branch2
$ git log --oneline --decorate --graph --all
```

Finally, the `branch2` can be safely deleted because it is already integrated into `branch3`:
```bash
$ git branch --merged branch3
$ git branch -d branch2
```

## Step 6
Rebase `branch5` on top of `branch3`.

Delete `branch3`.

## Step 7
Rebase `branch6` on top of `branch4`.

Delete `branch4`.

## Step 8
Rebase `branch6` on top of `main`.

At this point, `branch6` is "ahead" of `main` by four commits. To integrate the changes from `branch6` back into `main` use the `merge` command, which will result in a "fast forward" merge since:
* `main` did not diverge from `branch6` and
* `branch6` had its commits "(re)based" on `main` (which was the result of the previous rebasing).
```bash
$ git checkout main
$ git merge branch6
$ git log --oneline --decorate --graph --all
```

Delete `branch6`.

## Step 9
Rebase `branch1` on top of `main`.

Merge into `main` the changes from `branch1`, using the "fast forward" strategy.

Delete `branch1`.

## Step 10
Finally, rebase `branch5` on top of `main`, then ("fast forward") merge the changes from `branch5` back into `main`, and delete `branch5`.

## Step 11
Run the following command:
```bash
$ git log --oneline --decorate --graph --all
```

It is important to highlight that after these rebasing and "fast forward" merging operations, the `main` branch incorporated the changes from all branches while maintaining a linear history that is not cluttered with merge commits, making it easier for future developers to maintain the software contained in the repository.
