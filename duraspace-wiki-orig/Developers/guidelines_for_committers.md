# Guidelines for Committers

[Committing to the Shared Git Repository](#GuidelinesforCommitters-CommittingtotheSharedGitRepository)

[Use a branch for any work in progress](#GuidelinesforCommitters-Useabranchforanyworkinprogress)

[Always write tests for your code](#GuidelinesforCommitters-Alwayswritetestsforyourcode)

[If you want a code review, request it](#GuidelinesforCommitters-Ifyouwantacodereview,requestit)

[Rebase branches before committing to master or handing off to someone else](#GuidelinesforCommitters-Rebasebranchesbeforecommittingtomasterorhandingofftosomeoneelse)

[Merge your own branches to master](#GuidelinesforCommitters-Mergeyourownbranchestomaster)

[Delete remote branches when they're finished](#GuidelinesforCommitters-Deleteremotebrancheswhenthey'refinished)

[Git commit messages](#GuidelinesforCommitters-Gitcommitmessages)

-   [Sample commit messages](#GuidelinesforCommitters-Samplecommitmessages)

## Committing to the Shared Git Repository

**Note**: If you *are not* a committer on a project, please refer to the [Guidelines for contributors](https://wiki.duraspace.orgdisplay/hydra/Guidelines+for+Contributors) for information on how you can contribute code using pull requests. If you *are* a committer on a project, read these guidelines for contributing directly to the shared Git repository.

The code for each of the shared Hydra projects is collectively maintained by the project's committers. Each project has a technical lead and/or a release manager, but most decisions are made collectively.

1.  Use a branch for any work in progress
2.  Always write tests for your code
3.  If you want a code review, request it
4.  Rebase branches before committing to master or handing off to someone else
5.  Merge your own branches to master
6.  Delete remote branches when they're finished

See [Git Workflow for Hydra Projects](http://hudson.projecthydra.org/job/hydra-head-rails3-plugin/Documentation/file.GIT_WORKFLOW.html) for more information about how to create branches and use git when working on a Hydra project.

Also refer to the General Tips on the [Ways to Contribute Code](https://wiki.duraspace.orgdisplay/hydra/Ways+to+Contribute+Code) page.

### Use a branch for any work in progress

When starting work on a new feature, or merging features from another project, first create a remote git branch named after the most relevant Jira ticket. *Don't have a jira ticket for your work? [Create one](https://wiki.duraspace.org/display/hydra/Developers#Developers-Tickets%3AReportingBugs%26RequestingFeatures).*

### Always write tests for your code

Everyone should always write tests for the code they contribute, but it is especially important for committers to police themselves in this respect. You should be writing RSpec *and* Cucumber tests whenever applicable.

### If you want a code review, request it {#GuidelinesforCommitters-Ifyouwantacodereview,requestit}

Committers (people with push permissions on the git repository) take responsibility for controlling the quality of their own commits, but we all help each other when someone needs a fresh set of eyes. If you want some of your code reviewed, or if you are worried that your changes may affect other people's work, please ask other committers to look at your code & talk through it with you. The best ways to find reviewers:

-   email the hydra-tech list
-   email other committers directly
-   bring it up in a weekly committers call
-   find other committers on the \#blacklight or \#code4lib irc channels and ask them to meet you in the \#projecthydra channel

Once you've found a reviewer, you can transition your Jira ticket into review and assign a reviewer.

### Rebase branches before committing to master or handing off to someone else

Do not rebase on a public branch. Therefore, before rebasing, create a local branch of your public feature branch, rebase and then fast-forward merge with master.

See [Diaspora Git Workflow](https://github.com/diaspora/diaspora/wiki/Git-Workflow) for more info about rebasing.

### Merge your own branches to master

Committers are responsible for merging their own branches into master when their work is finished.
 If you're making major changes, or changes you want to discuss before merging into master, email the [hydra-tech](http://groups.google.com/group/hydra-tech) list and/or put yourself on the agenda for the next [Committers Call](https://wiki.duraspace.orgdisplay/hydra/Notes+from+Meetings+and+Calls).

### Delete remote branches when they're finished {#GuidelinesforCommitters-Deleteremotebrancheswhenthey'refinished}

When you're finished merging a branch into master, delete any associated remote branches you may have created in the process.

Ex. If you want to delete a remote branch called hydra-375

[?](#)

`git push origin :hydra-`{.java .plain}`375`{.java .value}

### Git commit messages

Commit messages should follow the guidelines described in detail at [http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html).

In summary:

-   First line: JIRA issue ID in all caps (if applicable), followed by a brief description (\~ 50 characters)
-   Second line: blank
-   Following lines: more detailed description, line-wrapped at 72 characters. May contain multiple paragraphs, separated by blank lines. Link to the JIRA issue, if applicable.

Use the present tense when writing messages, i.e. "Fix bug, apply patch", not "Fixed bug, applied patch."

#### Sample commit messages

-   linked to a JIRA issue:

        FCREPO-780: NPE thrown on disseminations

        Fix for the following bug: Fedora throws a null pointer exception if
        you call a disseminator that fronts a web service whose response does
        not contain a "Content-type" header.

        https://jira.duraspace.org/browse/FCREPO-780

-   Minor Commits accumulating against a single ticket/story:

        FCREPO-780: Fixed a typo in model spec.

        https://jira.duraspace.org/browse/FCREPO-780

        FCREPO-780: Adjusted padding for inputs

        https://jira.duraspace.org/browse/FCREPO-780

    etc. etc.


