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

1. Create the main repository
2. TODO: Create the submodule repository
3. TODO: Add the submodule repository with `--branch` or `set-branch`
4. TODO: Make a change in the submodule repository and run `git submodule update --remote`
5. TODO: Make a change in the submodule directory and make and push a commit and check it
