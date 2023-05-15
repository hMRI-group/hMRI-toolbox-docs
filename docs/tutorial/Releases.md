###### [Back to hMRI home page](Home)

# Releases and versioning

## Important note

In general, using the latest version of the toolbox is recommended. 
However, for a given study, it is important to process all the data with the same version of the toolbox. 
**Do not update the toolbox version halfway through processing your data!** 

Please **report the release version number** used when describing and publishing your results.

## Releases and master branch

Official releases and tags are listed on the [repository's releases page][releases-page].

The versions, notable changes between them and their backward compatibility are documented in the [CHANGELOG.md][changelog-md] file.

The [master branch][master-branch] of the repository may include recent developments not included in the last official release, but it is not a development branch. The master branch can include additional small bug fixes and minor changes, but no major changes impairing the backward compatibility of the toolbox. Refer to the diff link on the [releases page][releases-page] (`commit(s) to master since this release`) to compare a release to the current state of the master branch.

Developments are carried out on the developer's own repository forked from the master branch and merged to the master branch when appropriate (see [Develop & Contribute](Contribute) for details).

## Versioning

### Version numbers

Version numbers follow broadly the principles of the [semantic versioning system](http://semver.org).
The format of the version number is `major.minor.patch`. For example:

- Bug fix: increment the patch number (e.g. `1.0.1` to `1.0.2`)
- New feature (with backward compatibility ensuring that toolbox behaviour is identical if not making use of the new feature): increment the minor number and reset patch (e.g. `1.2.2` to `1.3.0`)
- Major change: refactoring and important new features that may or may not alter the backward compatibility (refer to the [CHANGELOG.md][changelog-md] file to check for compatibility): increment the major number and reset minor and patch (e.g. `1.2.2` to `2.0.0`).

### Step-by-step release process

*These notes describe how to release a new version. They are meant to clarify the way version numbers change and their relation to the current master branch. The process described is applied in a clean copy of the repository, after all bug fixes and changes have been accepted and merged into the master branch for a new release.*

- *Run all unit tests - under development*
- Update [CHANGELOG.md][changelog-md] to include the new version number, the release date and a summary of the changes + Commit
- Update [version.txt][version-txt] (e.g. from `v0.1.2-dev` to `v0.1.2`) + Commit
- Tag the release and upload it to the public repository
- Update [version.txt][version-txt] again (e.g. from `v0.1.2` to `v0.1.3-dev`) + Commit
- Update [CHANGELOG.md][changelog-md] with new empty entry (e.g. `v0.1.3-dev`) + Commit
- Push all commits to the public repository.

NOTE: *Before* release, the update of the version number may include more than simply removing the `-dev` suffix. For example, if the new release includes a major change, the update could go from `v0.1.2-dev` to `v1.0.0`. *After* release, the update of the version number will always increment the patch number only and append the `-dev` suffix.

### Version tracking

Between releases, the [CHANGELOG.md][changelog-md] file should be kept up-to-date by filling in the new version entry with a *summary* of the changes.
The *detailed* list of changes can be obtained by checking individual commits. 

The [version.txt][version-txt] and [CHANGELOG.md][changelog-md] files are therefore the (humanly readable) landmarks of the current version.

Please report the version number from [version.txt][version-txt] when opening an issue or emailing a question. If you're using the version from the master branch, please also report the latest commit hash known on the public repository.

Note that only the version number contained in [version.txt][version-txt] is logged in the JSON files accompanying the output of the hMRI-toolbox modules. 

[changelog-md]: ../blob/master/CHANGELOG.md
[version-txt]: ../blob/master/version.txt
[master-branch]: ..
[releases-page]: ../releases