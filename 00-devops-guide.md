# AEM DevOps guide

Adobe Experience Manager (AEM) DevOps encompasses the practices, processes and tools used in the development, testing, deployment and maintenance of applications built on Adobe Experience Manager. Adobe Experience Manager is a comprehensive content management solution enabling organizations to create, manage, and optimize digital customer experiences across various channels, including web, mobile, email and forms.

# Working with GitHub

For more information about the working ways of GitHub, please refer to the file below.
https://github.com/sede-x/oneshell-complete/blob/develop/README.md

## Instructions for DEPLOYMENT.md

1. If the `DEPLOYMENT.md` file is not empty and contains instructions:

   - The Yarn `release-it` command targets the `DEPLOYMENT.md` file and generates the `DEPLOYMENT.html`.
   - Once the `DEPLOYMENT.html` is generated, the script proceeds to the next step.

2. In the next step, if the `DEPLOYMENT.md` file is to be emptied:

   - The script clears the contents of `DEPLOYMENT.md`, commits with the message 'chore: clean up the DEPLOYMENT.md file', and pushes the commit to the repository.

3. If the `DEPLOYMENT.md` file is empty, no `DEPLOYMENT.html` file is generated, indicating that there are no specific instructions for that particular release.

## Feature branch creation

Follow the standard practice when creating a feature branch. Please refer to the format below for guidance.

- Format: `feature/ADO ticket number-short name`.
- For example: `feature/653991-documentation-CICD-tool-devops`.
- The branch name should start with `feature/` (or other markers), followed by an Azure DevOps (ADO) ticket number (if one exists), and then a short name without any special characters (like punctuation or spaces), separated by hyphens.
- Exceptions do exist, especially for changes created by automated tools like Dependabot, Whitesource Bot, or Shell-Repo-Bot. We cannot prevent these exceptions, but consistency is preferable.

### Branch protection rules

Branch protection rules in GitHub are settings that repository administrators can utilize to enforce certain workflows or prevent specific actions in certain branches. These rules are not exclusive to GitHub Actions but apply to the entire repository. However, they can influence how workflows are run, especially if the workflows involve pushing code to protected branches.

- Require pull request reviews before merging. This rule ensures that changes in a branch must receive approval through a pull request before they can be merged into the protected branch. According to the IRM, two mandatory approvals are required before any pull request can be merged.
- Approvals from code owners are required to merge the pull request. This practice helps maintain code quality and prevents the introduction of bugs or issues into the codebase.
- Require status checks to pass before merging. This rule is particularly relevant to GitHub Actions. If you have workflows that run checks on your code, such as running tests or formatting checks, you can require these checks to pass before code can be merged into the protected branch. This practice helps maintain the quality of your code by preventing changes from being merged until certain conditions are met. These checks are automated processes that run against code in the repository. Moreover, this step ensures that nothing gets merged into the protected branch that would break the build or violate the defined quality standards.
  Specifically, the following checks must pass:

  - Main build, test, deployment
  - Codecept tests for RIO
  - SonarQube Code Analysis
  - Format check
  - Playwright tests for Amidala
  - Playwright runtime tests for Amidala

- Require branches to be up-to-date before merging. This rule ensures that the branch being merged into the develop branch is current with the latest code from the develop branch. Before merging your branch (`feature-branch`) into the target branch (`develop`), you must first pull the latest changes from `develop` into your `feature-branch`.
- Require conversation resolution before merging. This rule stipulates that all conversations in a pull request must be resolved before the pull request can be merged. A conversation in a pull request typically begins when someone leaves a review comment on the code. This could be a question, a suggestion for improvement, or a request for changes. The conversation is considered 'unresolved' until the person who initiated the conversation marks it as 'resolved'.

For branch protection rules, please refer to the following document: https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule.

### Code owners

This is a feature in GitHub that allows you to designate certain individuals or teams as 'code owners' for specific parts of the codebase. When you enable this feature in your branch protection rules, any changes to the code owned by these individuals or teams will require their review and approval before they can be merged into the protected branch.

This is how it works:

- Create a file named `.github/CODEOWNERS` in the repository. This file identifies the individuals or teams that are the 'owners' of certain files or directories in codebase. Specifically, it should contain a rule that restricts the modification of the `CODEOWNERS` file itself to the administrators of the repository.
- In the `CODEOWNERS` file, list the paths to the files or directories along with the GitHub usernames or team names of their owners. For example, you might have a line like this: `/src @username`. This indicates that the user with the given username is the owner of the `src` directory and all files within it.
- When a pull request is opened that modifies any files owned by a certain user or team, that user or team will automatically be requested for review.
- If you have enabled the 'Require review from code owners' option in your branch protection rules, the pull request cannot be merged until the code owners have reviewed and approved the changes.

For information about code owners, please refer to the following document: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners.
