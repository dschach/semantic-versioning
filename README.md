# Semantic Versioning for Salesforce projects

> This is not intended to be a catch-all README. It is intended for Salesforce projects, and will purposely exclude uploading anything to npm.

My rules for semantic versioning and automatically creating changelogs and releases

For more information, see https://github.com/semantic-release/semantic-release - it has instructions for platforms other than GitHub Actions. This repo is just my own rules.

## Release Rules

Based on Angular rules: https://github.com/angular/angular/blob/main/CONTRIBUTING.md#-commit-message-format

## Commit Title Format

All commits that you want considered for releasing must have a certain format. Additionally, there are some tools like commitlint that force every commit message into this format:

```
type(scope): message
```

The scope is optional.

### Type (MY IDEAL SETUP)

Must be one of the following (separated by version increment - see below)

Major

- A breaking change, indicated by any of the other types followed by `!`. (Example: feat(api)!: deprecate a method)

Minor

- `feat`: A new feature

Patch

- `fix`: A bug fix
- `perf`: A code change that improves performance
- `test`: Adding missing tests or correcting existing tests
- `refactor`: A code change that neither fixes a bug nor adds a feature

"No-Release"

Note that none of these changes affect the core code of the application (meaning in sfdx terms that nothing here would impact any org or package metadata).

- `build`: Changes that affect the build system, actions, or package/external dependencies (example scopes: gulp, broccoli, npm, actions, sfdx). These are usually local files and packages. Helper apps (such as a changelog application or documentation generation) would go in this type.
- `ci`: Changes to CI configuration files and scripts (examples: CircleCI, SauceLabs, GitHub Actions). Unless you use an automatic CI system, don't use this type.
- `chore`: Similar to build; usually refers to bot updates or creating a release. This sometimes won't show up in release notes. This could include releasing a new Salesforce package version.
- `docs`: Documentation only changes
- `revert`: Any reversion of a PR/commit within a given release. This should not affect the version number. If this is a reversion across releases, then it should be noted as a feat/fix.

While these will often result in a patch change (See Release-Please below), I see some conflicting advice in my research. Sure, updating outdated npm packages used for package development do not change the code in the Salesforce package itself, BUT they do affect how development proceeds in the company. And if people just want to wait until they have a "real" code change before releasing an ISV patch/version, then there's nothing wrong with working on documentation only until you have something to release. Sure, you'll have a long changelog, but that's why we break things into sections!

So my conclusion is that "No-Release" is just a way of thinking about very minor changes that are, technically, a part of a company/org's metadata, and that metadata can be expanded to include documentation and build extensions, etc. I'm happy to be flexible on that.

### Scope

Here are some examples, but you can choose your own:

- `npm`: Updated npm packages & dependencies (auto-scoped via dependabot file)
- `actions`: Updated GitHub actions versions (auto-scoped via dependabot file)
- `packaging`: updates for releasing a new package version that are not the creation of the version itself
- `scripts`: Updated scripts
- `core`: Central functionality (can often be omitted, as many changes will be 'core')
- `tests`: Changes to Apex/Jest or other tests - should probably not use `test` type if you use this
- `API`: Custom APIs or integrations. Can also be for updating API versions of Salesforce metadata on a new release.
- `localization`: translations
- `security`: If you track profiles and permissions in your Salesforce pipeline, this may be an appropriate scope
- `ux`: layouts and general user experience
- `style`: code changes that look nicer (fixing whitespace) but do nothing else
- `(package name)`: adding or configuring a vscode extension, npm module, etc.
- `changelog`: updating the changelog (usually done automatically with a release)
- `no-release`: Will not be included in determining the version change (currently not in Release-Please)
- `ApexDox`: Updates to pages included in, or code comment references to, ApexDox. This should usually have a commit title `docs(ApexDox): <whatever was done>`
- `sfdx`: Updates to sfdx CLI commands
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

## Pull Request types

There are (to over-simplify) two kinds of pull request merge strategies:

- Merge and Rebase types both include every commit somwhere in the repo history. Debating "rewriting history" is beyond this conversation.
- Squash removes all the commits from each branch and presents only one PR commit in main.

Tools like Gearset (my favorite) use the Merge strategy. This is so it can do cool things like analyzing only commits since the most recent successful commit... which is beyond the scope of this document.

When configuring Dependabot to auto-merge its updates, one may use Squash or Merge -- I like Squash because it looks better in the commit history, but there's nothing wrong with using Merge. Plus, if you're using Gearset, it's best to be consistent.

## SemVar changes

# Changelog/Release Plugins & Packages

## [Release-Please (by Google)](https://github.com/googleapis/release-please)

This one is by Google, and while I think it is the most difficult to set up and does not have quite the number of customizations I would like, it has a huge advantage: It allows you to see the in-progress release notes every time you run the command to update the release PR.

I have included a sample config file [here](release-please-config.json). You will need to bootstrap your repository before using this for the first time, and then please feel free to take my sample config file and use parts of that.

Another huge advantage is that it allows for monorepos - so you can have multiple package directories in a single project. For SFDX, this means that you can modularize your org in one repo and the release notes should handle all the changes across each package, each with its own version number!

Currently, my preferred way to use this (because I know it works) is to use the CLI via VSCode. I can get the action to create a release~~, but I think it takes a manual run of the workflow for that to happen~~ once I merge the release pull request.

My biggest gripe with this tool is that if you use merge commits (not squash) then EVERY commit goes into the changelog if it is titled properly. That's just plain annoying. It gives a double-entry for pull requests, so if a dependency i sautomatically updated, there's a line for the PR and for the commit in that PR. (Example: [PR #5](https://github.com/dschach/semantic-versioning/issues/5) as seen in [Version 1.1.0](https://github.com/dschach/semantic-versioning/releases/tag/v1.1.0)).

I fully admit that the last point is ridiculous because I should just title commits properly only if I want them in the changelog.

### Release-Please tips

It will prefix every release tag with the package name. If you don't specify a package name, it will take one from the sfdx-project file, which can contain spaces. This is not good and will break Release-Please. Instead, be sure to include the `package-name` value in each package's release please config section. Here is a sample for a single package covering an entire repo:

```json
{
	"packages": {
		".": { "release-type": "salesforce", "package-name": "" }
	},
	"$schema": "https://raw.githubusercontent.com/googleapis/release-please/main/schemas/config.json"
}
```

The Salesforce release strategy doesn't handle multiple package directories well, so it's probably best to keep the name as blank for the top-level package (the entire repo) and then to give a package name to each sub-package. That way, once it works better, you'll get the right tag prefixes. As an example, in my campaign-member-status repo, I could have put "cm-status" as the package name for "force-app" had I wanted to publish multiple packages from the repo.

The place I'm still unclear is that Salesforce pacakges can have spaces in their names, so if the `package` attribute in sfdx-project.json overrides other package names, then I don't know if Release-Please can tag releases properly. This will take more investigation.

#### Release Types and Versioning Strategy

I plan to learn to write an extension file to change the release types that trigger minor and patch releases so that release-please will look past just the basic types in its default strategy:

- Major Change - breaking changes
- Minor Change - `feat` type
- Patch Change - `fix` and any other type

I'm annoyed by this because I think that test changes should be a patch or minor change. I'm looking into how to set up Release-Please so I can include a node class in my sfdx projects to override the versioning strategy.

#### Complex Commit Messages

There is a line in the code that says

```
If someone wishes to aggregate multiple, complex commit messages into a single commit, they can include one or more `BEGIN_NESTED_COMMIT`/`END_NESTED_COMMIT` blocks into the body of the commit
```

and I think that it could be neat to work on this, as it's not well-documented.

### [Release Please Action](https://github.com/google-github-actions/release-please-action)

And my GitHub Actions file is [here](/.github/workflows/release.yml). You'll note that I made a personal access token and put it into the action. I don't know exactly how secure this is, so I assume that you'll want to put some security around who can close the Release Please PR (which should trigger a new Package/version).

### Use with Gearset

Because Gearset does not Squash and Merge, you WILL have duplicate entries for some of your pull requests (meaning that you will have a line in the changelog for EVERY commit). So be careful on your commit messages.

You MAY want to avoid setting a type for commit messages so that they don't show up at all in the changelog - this is probably a good idea. Then, release-please will only include the PR title in the changelog. If, however, you do want to put any commits into the changelog, just format the messages properly and you're good to go!

## [Semantic-Release Plugin](https://semantic-release.gitbook.io/semantic-release/)

I DO NOT RECOMMEND THIS PRODUCT.

From their notes:

> semantic-release is meant to be executed on the CI environment after every successful build on the release branch.

When configured through GitHub Actions, it runs semantic-release on EVERY push to main. If the push type is a releasable artifact (see the [release config](.releaserc) file) where release is not false, then it will automatically update the changelog and (if configured) create a release. And it will increment the version number. I'm not okay with that, as I like to use multiple builds (the rare fourth number) in my versioning, and while this package does allow that, it does not follow Salesforce's format of 0.0.0-0. It seems to force some character(s) before the build number, and that's not acceptable. Plus, I want to use SFDX to update the build, so this is just weird.

Yes, I could do all my work in a branch and then branch from that branch and keep going until my branch (which I could name my new version) is perfect. Maybe then I would either merge the branch to update the main version or I would create the promoted package version and then merge... it's too complicated.

This package focuses on automating release creation for Git and npm. It bundles release notes and npm/GitHub packaging together. I like that it is flexible on which of the [Angular Commit Message Conventions](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#-commit-message-format#type) it allows. As with other plugins, the scope is completely free-form, so don't worry about having to adhere to Angular's scopes.

I have included my release configuration file [here](/.releaserc).

While the package comes bundled with a few plugins, these are the ones we will use:

These are included in the main package but are included for completeness

- @semantic-release/commit-analyzer
- @semantic-release/github
- @semantic-release/release-notes-generator
- semantic-release

These have to be installed separately

- @semantic-release/changelog
- @semantic-release/git
