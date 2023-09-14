# Contributing to the hMRI Toolbox

Contributing to the hMRI Toolbox can be done in many ways and at many levels,
from users to experienced developers. If you plan to contribute as a developer,
an inspirational guide on how to contribute to open-source projects can be found [here](https://opensource.guide/how-to-contribute/).

However, contributing involves much more than just coding!
It includes reporting bugs and discussing issues,
supporting other users, contributing documentation and examples,
making suggestions and writing new feature proposals,
reviewing proposals, issues and pull requests.

Contributing code by implementing bug fixes and new features is great,
but contributing any of these other aspects will be hugely appreciated :)!

## Issue Tracking

There are several ways you can contribute issues:

- by opening a new issue for discussion: e.g. if you believe that you have uncovered a bug,
  creating a new issue in the hMRI Toolbox [issue tracker][hmri-issues-page] is the way to report it.
- by helping to document an issue: either by providing supporting details
  (e.g. a test case demonstrating a bug), or providing suggestions on how to address the issue.
- by tidying up issues: link duplicate issues, suggest new issue labels, go through open issues
  and suggest closing old ones, ask clarifying questions on recently opened issues to move the discussion forward.
- by helping to resolve an issue: by opening a [pull request](#developments-and-pull-requests) 
  with the modified code that resolves the issue.

When opening an issue, first check whether an issue on the same or related topic already exists.
If not, go ahead, but make sure you include the following information:

- description of the issue (obviously!): as complete as possible, including error messages,
  code snippets, type of input, screenshot of image output, options...
  whatever may help us to identify the source of the problem!
- version of the hMRI Toolbox:
  if you are using an official release, the release number contained in the version.txt file of your release is all you need.
  If you are working on a fork or clone of the master branch, please also report the commit hash you forked from.
- Matlab version
- SPM version
- operating system

You are encouraged to help other contributors and to solve issues collaboratively.

Responses that provide neither additional context nor supporting detail are not helpful.
In many cases, such responses are simply annoying and unfriendly. Don't :).

Be friendly and supportive!

## Setting up your development branch

Before going into the forking and branching process, you must have the following
elements installed on your system (see also the [Get started](index.md) page):
- Matlab
- SPM12 (version 12.4 or later is recommended)
- Git

The following steps are described using the command line tool for `git`.

You must create a GitHub account or log in to your existing account to proceed.

### Step 1: Fork

You can refer to the GitHub documentation on [how to fork a repository](https://help.github.com/articles/fork-a-repo/).

For the hMRI Toolbox:
- on GitHub, navigate to the [hMRI toolbox]({{config.repo_url}}) repository,
- in the top-right corner of the page, click on the `Fork` button
- your own fork of the repository is now created, somewhere like `https://github.com/<username>/hMRI-toolbox`.

### Step 2: Clone & configure

Now create a local copy of your fork on your computer:

```text
$ git clone https://github.com/<username>/hMRI-toolbox.git
```

Also set the hMRI toolbox public repository as upstream:

```text
$ git remote add upstream https://github.com/hMRI-group/hMRI-toolbox.git
```

We recommend you configure `git` so that it knows who you are, for example:

```text
$ git config user.name "Peter Pepperpot"
$ git config user.email "peter.pepperpot@email.com"
```

### Step 3: Branch

Create a new (development) branch with a name related to the bug fix or feature you plan to implement, for example:

```text
$ git branch myBugFixIssue41
$ git checkout myBugFixIssue41
```

## Developments and Pull Requests

### Step 4: Code and Commit

Now you can start implementing your solution.
Please document each commit clearly, always referring to the specific bug you're trying to resolve,
or the specific feature you're implementing. If your patch fixes an open issue,
you can reference it in your commit.

### Step 5: Rebase

When you're done with the implementation on your development branch, make sure your master branch is up to date,
and if necessary, rebase your development branch to the latest commit from the hMRI toolbox public repository.

```text
$ git fetch upstream
$ git rebase upstream/master
```
This ensures that your development branch has the latest changes from the hMRI toolbox master branch.

### Step 6: Push

Push your development branch to your fork on GitHub.
According to the previous example:

```text
$ git push origin myBugFixIssue41
```

### Step 7: Open a Pull Request
The process for opening and reviewing a Pull Request is similar to that of opening issues.
Like issues, pull requests have their own discussion forum.
The Pull Request will be followed by a review and approval workflow to ensure that the proposed changes meet
the minimal quality and functional guidelines of the hMRI toolbox.

For more information about Pull Requests, visit the GitHub [Pull Request help pages](https://help.github.com/articles/about-pull-requests/).

## Future features

The hMRI-toolbox is an ongoing project and many features are still on our to-do list.
The list below includes features that are either under development, being tested, or not implemented at all.
If you wish to contribute to these or add new ones, before you implement anything please
contact one of the main [hMRI developers](https://github.com/hMRI-group/hMRI-toolbox#developers-of-the-hmri-toolbox)
to check for current status of developments, or open an issue describing your proposal.

- implementation of the classic Driven Equilibrium Single-Pulse
  Observation of T1 (DESPOT1) algorithm, also known as the
  Variable Flip-Angle (VFA) method for T1 mapping [ref].

- map creation for phantom data

- MR g-ratio

- susceptibility mapping

- denoising: spatially adaptive noise removal methods (Tabelow *et al.*, 2016)
  and appropriate handling of the Rician bias problem (Polzehl and Tabelow, 2016;
  Tabelow *et al.*, 2017)


[mailing-list-home]: https://www.jiscmail.ac.uk/cgi-bin/webadmin?A0=HMRI-TOOLBOX
[version-txt]: {{config.repo_url}}/blob/master/version.txt
[changelog-md]: {{config.repo_url}}/blob/master/CHANGELOG.md
[hmri-issues-page]: {{config.repo_url}}/issues