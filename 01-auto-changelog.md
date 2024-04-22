# Auto-Changelog guide

An auto-changelog is a process that automatically generates a log of changes for each new version of software, based on Git history. This can be particularly beneficial in projects where updates are frequent and manually tracking changes would be time-consuming.

Previously, developers needed to make their entry for the appropriate pull request, and the reviewer had to confirm the same. The new auto-changelog will eliminate the need for manual updates of `CHANGELOG.md`. The auto-changelog uses the commit message provided while merging the pull request into `develop`. This message is selected when the release workflow is initiated. Currently, we are using the `keepachangelog` template to format the message in `CHANGELOG.md` and the release notes.

- The `changelog` command under `git` uses Yarn to run auto-changelog, which generates a changelog based on Git history. The options specify that it should output to stdout, include verbose commit details, not limit the number of commits, only include unreleased changes, use a specific tag prefix, use the `keepachangelog` template and sort commits by subject.
- The `tagName` field specifies the format of the Git tag for each release, which uses the version number. This tag is utilized by auto-changelog to determine which commits to include in the changelog.
- The `after:bump` hook under `hooks` also triggers auto-changelog after the version number is bumped. It specifies the latest version, starting version and tag prefix, which are used to determine which commits to include in the changelog.

So, when a new release is created, auto-changelog is executed to generate a changelog that includes all commits from the starting version to the latest version. The changelog, formatted using the `keepachangelog` template, includes detailed information about each commit.

For more information, please refer to the following link: https://github.com/sede-x/oneshell-complete/blob/develop/package.json

## Rules

- Upon creating the PR to merge the feature branch into `develop`:

  - Briefly explain the PR in a sentence.
  - Ensure that the PR is linked to one or more tasks.
  - Adhere to the specified notation.

- Include a link to the successful runtime test execution:

  - Address any instabilities in the test run – either fix them or raise a ticket.

- If a migration script is present, follow these steps and include a link to the migration run:

  - Request a package for a production site.
  - Install the package on the non-production environment (NPE) assigned to PR.
  - Execute the migration script twice.
  - Provide a link to the migration history for the reviewer's verification.

- When merging the branch into develop after completing all checks:

  - Provide a clear explanation of the commit message in a few words.
  - Start the commit message with keywords such as "Added," "Fixed," "Changed," "Removed," etc., based on the nature of the PR.
  - Stick to the specified notation.
  - Refer to the following documentation for more details: https://github.com/cookpete/auto-changelog
