# semantic-versioning for Salesforce projects

> This is not intended to be a catch-all README. It is intended for Salesforce projects, and will purposely exclude uploading anything to npm.

My rules for semantic versioning and automatically creating changelogs and releases

For more information, see https://github.com/semantic-release/semantic-release - it has instructions for platforms other than GitHub Actions. This repo is just my own rules.

## Release Rules

Based on Angular rules: https://github.com/angular/angular/blob/main/CONTRIBUTING.md#-commit-message-format

### Type

Must be one of the following (separated by version increment - see below)

Major

- A breaking change, indicated by any of the other types followed by `!`. (Example: feat(api)!: deprecate a method)

Minor

- `feat`: A new feature (Minor)

Patch

- `fix`: A bug fix (Patch)
- `perf`: A code change that improves performance (Patch)
- `test`: Adding missing tests or correcting existing tests (Patch) - `refactor`: A code change that neither fixes a bug nor adds a feature (Patch)

No-Release

- `build`: Changes that affect the build system, actions, or external dependencies (example scopes: gulp, broccoli, npm, actions). (No-Release)
- `ci`: Changes to CI configuration files and scripts (examples: CircleCi, SauceLabs). Unless you use an automatic CI system, don't use this type. (No-Release)
- `chore`: Similar to build; usually refers to bot updates or creating a release. This generally won't show up in release notes. (No-Release)
- `docs`: Documentation only changes (No-Release)
- `revert`: Any reversion of a PR/commit. This should not affect the version number. If this is a reversion across releases, then it should be noted as a feat or chore. (No-Release)

### Scope

Here are some examples, but you can choose your own:

- `npm`: Updated npm packages & dependencies
- `actions`: Updated GitHub actions versions
- `packaging`: updates for releasing a new package version that are not the creation of the version itself
- `scripts`: Updated scripts
- `core`: Central functionality
- `API`: Custom APIs or integrations. Can also be for updating API versions of Salesforce metadata on a new release.
- `localization`: translations
- `security`: If you track profiles and permissions in your Salesforce pipeline, this may be an appropriate scope
- `UX`: layouts and general user experience
- `style`: code changes that look nicer (fixing whitespace) but do nothing else
- `(package name)`: adding or configuring a vscode extension, npm module, etc.
- `changelog`: updating the changelog
- `no-release`: Will not be included in determining the version change (No-Release)
- `ApexDox`: Updates to pages included in, or code comment references to, ApexDox. This should usually have a commit title `docs(ApexDox): <whatever was done>`
- `README`: Updating the project README. For updating instructions, use `docs(README)` and if you're running a script to update a release ID for a Salesforce package version, that's `build(README)` or `chore(README)`.

### Behavior

- Major: Update the first number in the version (e.g. 2.5.0 -> 3.0.0)
  This usually happens only for breaking changes
- Minor: Update the second number
- Patch: Update the third number
- No-Patch: No new release number needed

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

# Changelog/Release Plugins & Packages

## [Release-Please (by Google)](https://github.com/googleapis/release-please)

This one is by Google, and while I think it is the most difficult to set up and does not have quite the number of customizations I would like, it has a huge advantage: It allows you to see the in-progress release notes every time you run the command to update the release PR.

I have included a sample config file [here](release-please-config.json). You will need to bootstrap your repository before using this for the first time, and then please feel free to take my sample config file and use parts of that.

### [Release Please Action](https://github.com/google-github-actions/release-please-action)

And my GitHub Actions file is [here](/.github/workflows/release.yml).

Another huge advantage is that it allows for monorepos - so you can have multiple package directories in a single project. For SFDX, this means that you can modularize your org in one repo and the release notes should handle all the changes across each package, each with its own version number!

## [Semantic-Release Plugin](https://semantic-release.gitbook.io/semantic-release/)

This package does a great job of automating release creation. It bundles release notes and npm/GitHub packaging together. I like that it is flexible on which of the [Angular Commit Message Conventions](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#-commit-message-format#type) it allows. As with other plugins, the scope is completely free-form, so don't worry about having to adhere to Angular's scopes.

I have included my release configuration file [here](/.releaserc).

While the package comes bundled with a few plugins, these are the ones we will use:

- @semantic-release/commit-analyzer
- @semantic-release/github
- @semantic-release/release-notes-generator
- semantic-release

- @semantic-release/changelog
- @semantic-release/git
- Note: The first four are bundled together in semantic-release, but I have included them for completeness.
