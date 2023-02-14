# semantic-versioning for Salesforce projects

> This is not intended to be a catch-all README. It is intended for Salesforce projects, and will purposely exclude uploading anything to npm.

My rules for semantic versioning and automatically creating changelogs and releases

For more information, see https://github.com/semantic-release/semantic-release - it has instructions for platforms other than GitHub Actions. This repo is just my own rules.

## Release Rules

Based on Angular rules: https://github.com/angular/angular/blob/main/CONTRIBUTING.md#-commit-message-format

### Type

Must be one of the following:

- `build`: Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm). (No-Release)
- `chore`: Similar to build; usually refers to bot updates.
- `ci`: Changes to our CI configuration files and scripts (examples: CircleCi, SauceLabs) (No-Release)
- `docs`: Documentation only changes (No-Release)
- `feat`: A new feature (Minor)
- `fix`: A bug fix (Patch)
- `perf`: A code change that improves performance (Patch)
- `refactor`: A code change that neither fixes a bug nor adds a feature (Patch)
- `revert`: Any reversion of a PR/commit. This should not affect the version number. If this is a reversion across releases, then it should be noted as a feat or chore. (No-Release)
- `test`: Adding missing tests or correcting existing tests (Patch)

### Scope

Here are some examples, but you can choose your own:

- `npm`: Updated npm packages & dependencies
- `actions`: Updated GitHub actions versions
- `packaging`: updates for releasing a new package version that are not the creation of the version itself
- `scripts`: Updated scripts
- `core`: Central functionality
- `API`: Custom APIs or integrations. Can also be for updating API versions of Salesforce metadata on a new release.
- `localization`: translations
- `UX`: layouts and general user experience
- `style`: code changes that look nicer (fixing whitespace) but do nothing else
- `(package name)`: adding or configuring a vscode extension, npm module, etc.
- `changelog`: updating the changelog
- `no-release`: Will not be included in determining the version change (No-Release)
- `ApexDox`: Updates to pages included in, or code comment references to, ApexDox. This should usually have a commit title docs(ApexDox): <whatever was done>
- `README`: Updating the project README

### Behavior

- Major: Update the first number in the version (e.g. 2.5.0 -> 3.0.0)
  This usually happens only for breaking changes
- Minor: Update the second number
- Patch: Update the third number

### Summary

Use the summary field to provide a succinct description of the change:

- use the imperative, present tense: "change" not "changed" nor "changes"
- don't capitalize the first letter
- no dot (.) at the end

### Commit Message Body

Just as in the subject, use the imperative, present tense: "change" not "changed" nor "changes".

Explain the motivation for the change in the commit message body. This commit message should explain why you are making the change. You can include a comparison of the previous behavior with the new behavior in order to illustrate the impact of the change.

### Commit Message Footer

The footer can contain information about breaking changes and deprecations and is also the place to reference GitHub issues, Jira tickets, and other PRs that this commit closes or is related to. For example:

```
BREAKING CHANGE: <breaking change summary>
<BLANK LINE>
<breaking change description + migration instructions>
<BLANK LINE>
<BLANK LINE>
Fixes #<issue number>
```

or

```
DEPRECATED: <what is deprecated>
<BLANK LINE>
<deprecation description + recommended update path>
<BLANK LINE>
<BLANK LINE>
Closes #<pr number>
```

Breaking Change section should start with the phrase "`BREAKING CHANGE: `" followed by a summary of the breaking change, a blank line, and a detailed description of the breaking change that also includes migration instructions.

Similarly, a Deprecation section should start with "`DEPRECATED: `" followed by a short description of what is deprecated, a blank line, and a detailed description of the deprecation that also mentions the recommended update path.

### Revert commits

If the commit reverts a previous commit, it should begin with `revert: `, followed by the header of the reverted commit.

The content of the commit message body should contain:

information about the SHA of the commit being reverted in the following format: `This reverts commit <SHA>`,
a clear description of the reason for reverting the commit message.

## SemVar changes

## Semantic-Release Plugins

While the package comes bundled with a few plugins, these are the ones we will use

###
