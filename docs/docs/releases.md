# Releases and Versioning

In general, using the latest version of the toolbox is recommended.
However, for a given study, it is important to process all the data with the same version of the toolbox.

!!! warning
    Do not update the toolbox version halfway through processing your data!

Please **report the release version number** used when describing and publishing your results.

## Releases and Master Branch

Official releases and tags are listed on the [repository's releases page][releases-page].

The versions, notable changes between them and their backward compatibility are documented in
the [CHANGELOG.md][changelog-md] file.

The [master branch][master-branch] of the repository may include recent developments not included in the last official
release, but it is not a development branch. The master branch can include additional small bug fixes and minor changes,
but no major changes impairing the backward compatibility of the toolbox. Refer to the diff link on
the [releases page][releases-page] (commit(s) to master since this release) to compare a release to the current state
of the master branch.

Developments are carried out on the developer's own repository forked from the master branch and merged to the master
branch when appropriate (see [Develop & Contribute](Contribute) for details).

## Versioning

Version numbers follow broadly the principles of the [semantic versioning system](http://semver.org).
The format of the version number is `major.minor.patch`. For example:

- Bug fix: increment the patch number (e.g. `1.0.1` to `1.0.2`)
- New feature (with backward compatibility ensuring that toolbox behaviour is identical if not making use of the new
  feature): increment the minor number and reset patch (e.g. `1.2.2` to `1.3.0`)
- Major change: refactoring and important new features that may or may not alter the backward compatibility (refer to
  the [CHANGELOG.md][changelog-md] file to check for compatibility): increment the major number and reset minor and
  patch (e.g. `1.2.2` to `2.0.0`).


[changelog-md]: {{config.repo_url}}/blob/master/CHANGELOG.md
[version-txt]: {{config.repo_url}}/blob/master/version.txt
[master-branch]: {{config.repo_url}}
[releases-page]: {{config.repo_url}}/releases