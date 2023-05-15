###### [Back to hMRI home page](Home)

# Develop and contribute

An inspirational guide to contributing to open-source projects can be found [here](https://opensource.guide/how-to-contribute/).

Contributing to the hMRI-toolbox can be done in many ways and at many levels, 
from users to experienced developers.

Contributing involves much more than coding! 
It includes reporting bugs and discussing issues, 
supporting other users, contributing documentation and examples, 
making suggestions and writing new feature proposals, 
reviewing proposals, issues and pull requests...

Contributing code by implementing bug fixes and new features is great, 
but contributing any of these other aspects will be hugely appreciated :)!

## CONTENT

[INTRODUCTION AND GENERAL GUIDELINES](#introduction-and-general-guidelines)     
Guidelines about which means of communication and contribution to use.

[MAILING LIST](#mailing-list)       
Practical information on how to use the mailing list.

[ISSUE TRACKING](#issue-tracking)        
How to create an issue or contribute to discussing an issue.

[SETTING UP YOUR DEVELOPMENT BRANCH](#setting-up-your-development-branch)      
How to fork the repository, branch it to fix a bug or implement a new feature.

[DEVELOMENTS & PULL REQUESTS](#developments-and-pull-requests)      
The process of making changes and how to create a pull request.

[FUTURE FEATURES](#future-features)         
A short list of features currently under development or considered for future development.

## Introduction and general guidelines

Welcome to the hMRI-toolbox!

The hMRI-toolbox was made publicly available in June 2018. 
Until then, its development was carried out by a limited number of developers among whom good communication and good humour prevented any conflict.
We hope that this will continue, and so we expect communication, mutual respect and good humour to be values held by all contributors.

The guidelines provided here are a suggestion of how to proceed, and are expected to evolve as more users and contributors get involved.
Feedback and suggestions on these guidelines are also welcome!

Useful information is already available:                    
- on the [Wiki pages](../wiki)
- in the [mailing list archive](#mailing-list)
- in the [issue tracking system](#issue-tracking-system)

If you cannot find the expected information there, you can either post a new question on the [mailing list](#mailing-list) or open an [issue](#issue-tracking-system).

If you believe you have uncovered a bug, [opening an issue](#issue-tracking-system) is the recommended way to proceed.

If you have a solution to an issue or a new feature enhancing the toolbox, you may want to [fork the repository](#step-2-fork) and later [create a pull request](#pull-requests).

These topics are described in more detail below.

## Mailing list

If you have questions about the hMRI toolbox, 
or want to contribute by answering other users' questions, 
or wish to keep posted about what's going on with the hMRI toolbox, [join the mailing list][mailing-list-home]!

Go to the [HMRI-TOOLBOX mailing list home page][mailing-list-home] to subscribe, unsubscribe, manage your account and browse the archives. 

Once you have subscribed to the mailing list, you can send emails to HMRI-TOOLBOX@JISCMAIL.AC.UK 
with your questions and/or contribute by answering questions and helping out other users.

You can also create a password from the same [home page][mailing-list-home] in order to access and search the mailing list archives. 

## Issue tracking

There are several ways you can contribute issues:

- by opening a new issue for discussion: e.g. if you believe that you have uncovered a bug, 
creating a new issue in the hMRI-toolbox [issue tracker][hmri-issues-page] is the way to report it.
- by helping to document an issue: either by providing supporting details 
(e.g. a test case demonstrating a bug), or providing suggestions on how to address the issue.
- by tidying up issues: link duplicate issues, suggest new issue labels, go through open issues 
and suggest closing old ones, ask clarifying questions on recently opened issues to move the discussion forward.
- by helping to resolve an issue: by opening a [pull request](#pull-requests) with the modified code that resolves the issue.

When opening an issue, first check whether an issue on the same or related topic already exists. 
If not, go ahead! but make sure you include the following information:            

- description of the issue (obviously!): as complete as possible, including error messages, 
code snippets, type of input, screen shot of image output, options... 
whatever may help us to identify the source of the problem!
- version of the hMRI-toolbox:
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
elements installed on your system (see also the [Get started](GetStarted) page):        
- Matlab
- SPM12 (version 12.4 or later is recommended)
- Git

The following steps are described using the command line tool for `git`.

You must create a GitHub account or sign into your existing account to proceed.

### Step 1: Fork

You can refer to the GitHub documentation on [how to fork a repository](https://help.github.com/articles/fork-a-repo/).

For the hMRI-toolbox:      
- on GitHub, navigate to the [hMRI toolbox](..) repository,
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
[version-txt]: ../blob/master/version.txt
[changelog-md]: ../blob/master/CHANGELOG.md
[hmri-issues-page]: ../issues