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
  git submodule add https://github.com/TomasHubelbauer/git-submodule-track-branch-sub sub --branch .
  ```

  Note that `git submodule set-branch` can be used instead of `--branch` later.
  Also note that `.` is a special name which makes the submodule trach a branch
  by the same name as the branch name of the main repository is.

- [ ] Make a change in the submodule repository and verify pulling it in the main repository

  ```sh
  git submodule update --remote
  ```

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
