This page describes conventions and best practices applicable to the Hydra Git repository.

[Two Ways to Contribute Code: Submitting Pull Requests & Committing Directly](#WaystoContributeCode-TwoWaystoContributeCode:SubmittingPullRequests&CommittingDirectly)

[General Tips for all Contributors](#WaystoContributeCode-GeneralTipsforallContributors)

-   [Reference Material](#WaystoContributeCode-ReferenceMaterial)
-   [Files & Directories to Watch out for in Commits](#WaystoContributeCode-Files&DirectoriestoWatchoutforinCommits)

## Two Ways to Contribute Code: Submitting Pull Requests & Committing Directly {#WaystoContributeCode-TwoWaystoContributeCode:SubmittingPullRequests&CommittingDirectly}

Depending on whether you are a *contributor* or a *committer*, you will submit your code by a different process, so let's define these terms.

*contributor*: Someone who contributes code to a project but does not have commit permissions on the shared repository. Contributors submit their code by means of pull requests.

*committer*: Someone who has commit (ie. push) permissions on the shared repository. Committers apply their changes directly to the shared repository.

[Guidelines for Contributors](/display/hydra/Guidelines+for+Contributors)\
 [Guidelines for Committers](/display/hydra/Guidelines+for+Committers)

## General Tips for all Contributors {#WaystoContributeCode-GeneralTipsforallContributors}

### Reference Material {#WaystoContributeCode-ReferenceMaterial}

Excellent guidance is available in the Fedora Commons repository development area of this wiki: in particular, a page listing [general Git guidelines and best practices](/display/FCREPO/Git+Guidelines+and+Best+Practices), a [quick start guide](/display/FCREPO/Git+Quick+Start+Guide) and a [list of further useful resources](/display/FCREPO/Git+Resources).

Please read the page on [Recommended Code Practices](/display/hydra/Recommended+Code+Practices).

### Files & Directories to Watch out for in Commits {#WaystoContributeCode-Files&DirectoriestoWatchoutforinCommits}

You may have local customisations to your config/fedora.yml, config/database.yml, config/solr.yml which shouldn't be part of any pull requests. It's best to exclude these from commits (or simply stash your changes before committing, such that they are restorable after commit, leaving you with a clean tree). Also, the 'jetty' directory will sometimes incur changes while jetty is running, so it's best not to commit anything in this directory unless you have actually changed something substantive.
