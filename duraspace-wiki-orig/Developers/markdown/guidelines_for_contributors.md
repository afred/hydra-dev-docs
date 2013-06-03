## When to submit a Pull Request {#GuidelinesforContributors-WhentosubmitaPullRequest}

If you are not a committer on a project, you can still contribute code by submitting pull requests. When you submit a pull request, one of the project committers can then merge your changes into the project.

If you *are* a committer on the project, you can skip the pull request and instead push a new branch with your changes. For more info on this, please refer to the [Ways to Contribute Code](/display/hydra/Ways+to+Contribute+Code).

## Before Submitting your Pull Request {#GuidelinesforContributors-BeforeSubmittingyourPullRequest}

Before submitting a pull request, send an email to the hydra-tech mailing list describing the changes you're contributing. The committers might also ask you to join the weekly committers call to discuss your contribution(s).

Please follow the recommended practices listed in the [Developers](/display/hydra/Developers) pages and refer to the General Tips on the [Ways to Contribute Code](/display/hydra/Ways+to+Contribute+Code) page.

## Git Technique: Submitting topic/feature branches from your own fork of the Hydrangea tree to the "upstream" shared repository {#GuidelinesforContributors-GitTechnique:Submittingtopic/featurebranchesfromyourownforkoftheHydrangeatreetothe"upstream"sharedrepository}

Most Hydra features should be developed and contributed as topic branches for clarity and ease of integration ﻿`Graeme: Is this right? Do the committers want to encourage this? Should I be more specific about this relating only to non-committers?`. Using branches allows institution-specific changes to be stripped out for the purpose of submitting back to the trunk.

The basic approach is to create a branch as follows:

[?](#)

`git branch myfeature`{.java .plain}

`git checkout myfeature`{.java .plain}

Then use **git rm** to remove any files not relevant to the feature. You can make commits or roll back to a previous state to get rid of any institution-specific changes.

Note that **git rm** only removes the files from the current changeset - your files are still safe in your main (probably 'master') branch. You can run:

[?](#)

`git checkout master`{.java .plain}

to get back to your original tree (with all its institution-specific stuff) at any time.

When you have pared down your new branch to just that which is relevant to the feature in question, check that the feature works and that all Rspec tests pass. Then commit and push to your fork on Github:

[?](#)

`git push origin myfeature`{.java .plain}

After that, you can make a pull request from the Github interface, asking projecthydra/hydrangea to merge in your 'myfeature' branch.

**Tip:** The Github pull request page is an excellent way to itemise the differences between your feature branch and the upstream Hydrangea Git repository. Try clicking the Files Changed tab to get a diff across the whole project of your branch relative to the upstream repository. This can help in weeding out institution-specific changes.
