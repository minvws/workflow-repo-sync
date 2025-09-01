# Repo sync

> [!WARNING]
> This repository is no longer maintained.
> 
> We have migrated to automated synchronization solutions outside of GitHub Workflows.
> This workflow-based approach is no longer actively developed or supported.
> If you wish to continue using this solution, please fork this repository to maintain your own version.

This repository provides a reusable GitHub Workflow which synchronizes a branch on a private repository with a branch on a public repository by merging all commits from the configured branch to the shadow branch. This shadow branch is then pushed to the public repository. All tags on the private repository can optionally be pushed the public repository as well.

> [!CAUTION]
> This pipeline pushes all commits from a branch on the private repository to a branch on the public repository.
> Ensure that the private repository does not contain any sensitive or unwanted commits before running the sync.

## Usage

To use the workflow, add it to the **private** repository like so:


```yml
name: Public repo sync

on:
  workflow_dispatch:
  push:
    tags:
      - v*

jobs:
  sync-public:
    uses: minvws/workflow-repo-sync/.github/workflows/repo-sync.yml@v1
    with:
      repository: git@github.com:minvws/<repo>.git
      ref: <branch>
      push_tags: true
    secrets:
      REPO_SYNC_PUSH_KEY: ${{ secrets.REPO_SYNC_PUSH_KEY }}
```

Replace `<repo>` with the repo name and `<branch>` with the branch you want to sync. Most of the time the branch is `main`.

In this basic example, the workflow is executed automatically when a tag is pushed. And thanks to the `workflow_dispatch` trigger it can also be executed manually from the repository's Actions tab.

### Configuration

This workflow has the following inputs:

- `repository` (**required**): An SSH URL, like `git@github.com:user/repo.git`.
- `branch` (optional): Branch to sync. Default: `'main'`.
- `shadow_branch` (optional): Private shadow branch of public repository. Default: `'main-public'`.
- `ref` (optional): Ref to sync. Default: `'main'`.
- `push_tags` (optional): If tags should be pushed to the target repository. Default: `'false'`.
- `allow_public_dependabot` (optional): Allow dependabot.yml on the public repository. Default: `'false'`.

The secret that is needed is `REPO_SYNC_PUSH_KEY`. This is a SSH Key. In the public repository a deployment key with Read-write permissions can be configured.
The private key should be added to the secrets of the private repository.

More information about deployment keys can be found in [the GitHub documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys#set-up-deploy-keys).

## Contributing

If you want to contribute a new pipeline, please check the reusable workflow guidelines in the
[GitHub documentation](https://docs.github.com/en/actions/using-workflows/reusing-workflows#creating-a-reusable-workflow).

## License

This repository is released under the EUPL 1.2 license. [See LICENSE.txt](./LICENSE.txt) for details.

## Part of iCore

This package is part of the iCore project.
