# Git submodule track branch

I want to test out having a submodule in a repository and having it be tracking a
branch such that when the submodule is updated, it fetches the latest commits of
that branch as well as when a change is made in the submodule directory, it is
possible to commit and push that change from that directory to the submodule
repository from within the context of the main repository.

Here's what I did and tried.
I am hoping to build a full checklist at the end of this experiment.
No guarantees this actually worked until this text states so, for now I am only
messing around.

- [x] Create the main repository

  https://github.com/TomasHubelbauer/git-submodule-track-branch

- [x] Create the submodule repository

  https://github.com/TomasHubelbauer/git-submodule-track-branch-sub

- [x] Add the submodule repository to the main repository

  ```sh
  git submodule add --branch main https://github.com/TomasHubelbauer/git-submodule-track-branch-sub sub
  cd sub
  git status
  # On branch main
  # Your branch is up to date with 'origin/main'.
  cd ..
  ```

  `git submodule set-branch` can be used instead of `--branch` in a later stage.

  `.` is a special value for `--branch` which should make the submodule track a
  branch by the same name as the branch of the main repository, but I was not
  able to make this work in Git 2.39 on macOS:

  > fatal: 'origin/.' is not a commit and a branch '.' cannot be created from it

  If anything goes wrong in this step, this is how adding the submodule can be
  fully undone to try again with a clean slate:

  ```sh
  git rm -f .gitmodules
  git rm -f sub
  rm -rf .git/modules/sub
  # .git/config
  git config --remove-section submodule.sub
  ```

  `-f` is used to delete the path regardless of whether it has unstaged changes.
  `git config --file .gitmodules  --remove-section submodule.sub` can be used if
  wanting to remove a single of multiple submodules instead of nuking the file.

  In GitHub, the submodule "directory" will still link to the tree of the given
  commit, it will not respect `--branch` and take you to the repository tree at
  that branch. :/

- [ ] Make a change in the submodule repository and verify pulling it in the main repository

  ```sh
  git submodule update --remote
  git status
  # modified:   sub (new commits)
  cd sub
  git status
  # HEAD detached at
  ```

  I was able to pull the changes but it looks like it broke the remote branch
  tracking of the submodule?

- [ ] Make a change in the submodule directory and verify pushing it to the submodule repository

  ```sh
  cs sub
  echo "- Added a change to the submodule directory for push to the repository"
  git add *
  git status
  git commit -m "Made a change from the submodule directory in the main repository" -m "Not in the submodule repository!"
  git push
  git status
  cd ..
  git submodule update --remote
  git status
  ```
