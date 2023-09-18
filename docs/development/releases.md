# Releases and Versioning

These notes describe how to release a new version. They are meant to clarify the way version numbers change and their
relation to the current master branch. The process described is applied in a clean copy of the repository, after all bug
fixes and changes have been accepted and merged into the master branch for a new release.

- Run all unit tests
- Update [CHANGELOG.md][changelog-md] to include the new version number, the release date and a summary of the changes +
  Commit
- Update [version.txt][version-txt] (e.g. from `v0.1.2-dev` to `v0.1.2`) + Commit
- Tag the release and upload it to the public repository
- Update [version.txt][version-txt] again (e.g. from `v0.1.2` to `v0.1.3-dev`) + Commit
- Update [CHANGELOG.md][changelog-md] with new empty entry (e.g. `v0.1.3-dev`) + Commit
- Push all commits to the public repository.

!!! warning

    **Before** release, the update of the version number may include more than simply removing the `-dev` suffix. For
    example, if the new release includes a major change, the update could go from `v0.1.2-dev` to `v1.0.0`. *After* release,
    the update of the version number will always increment the patch number only and append the `-dev` suffix.

### Version tracking

Between releases, the [CHANGELOG.md][changelog-md] file should be kept up-to-date by filling in the new version entry
with a *summary* of the changes.
The *detailed* list of changes can be obtained by checking individual commits.

The [version.txt][version-txt] and [CHANGELOG.md][changelog-md] files are therefore the (humanly readable) landmarks of
the current version.

Please report the version number from [version.txt][version-txt] when opening an issue or emailing a question. If you're
using the version from the master branch, please also report the latest commit hash known on the public repository.

Note that only the version number contained in [version.txt][version-txt] is logged in the JSON files accompanying the
output of the hMRI Toolbox modules.

[changelog-md]: {{config.repo_url}}/blob/master/CHANGELOG.md

[version-txt]: {{config.repo_url}}/blob/master/version.txt

[master-branch]: {{config.repo_url}}

[releases-page]: {{config.repo_url}}/releases