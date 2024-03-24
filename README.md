[![Build status](https://badge.buildkite.com/7eca2ecc8cd4f571e1c2e26000c51030c808e7bca6b9b25e42.svg)](https://buildkite.com/datum/git-commit-buildkite-plugin)

# git-commit

A buildkite plugin to commit and push results of command(s) to a remote git repository

## Example

Add the following to your `pipeline.yml`:

```yml
steps:
  - command: task generate
    plugins:
      - datumforge/git-commit#v1.0.0: ~
```

The default options commit all changed/added files to `$BUILDKITE_BRANCH` and pushes to `origin`.

An example with fully customized options:

```yml
steps:
  - command: task generate
    plugins:
      - datumforge/git-commit#v1.0.0:
          add: app/
          branch: mitb
          create-branch: true
          message: "Task generate output"
          remote: upstream
          user:
            name: bender-rodriguez
            email: brodriguez@datum.net
```

## Configuration

### add (optional, defaults to `.`)

A pathspec that will be passed to `git add -A` to add changed files.

### branch (optional, defaults to `$BUILDKITE_BRANCH`)

The branch where changes will be committed. Since Buildkite runs builds in a detached HEAD state, this plugin will fetch and checkout the given branch prior to committing. Unless we're creating a new branch. See `create-branch`

### create-branch (optional, defaults to `false`)

When set to true the branch will be created, rather than fetched from the remote

### message (optional, defaults to `Build #${BUILDKITE_BUILD_NUMBER}`)

The commit message

### remote (optional, defaults to `origin`)

The git remote where changes will be pushed

### user.email (optional)

If given, will configure the git user email for the repo

### user.name (optional)

If given, will configure the git user name for the repo

### https (optional)

If set to true, commits will be pushed over `https` instead of `ssh`. The `user.name` configuration and `GITHUB_TOKEN` environment variable **must** also be set. 

## Developing

Requires [taskfile](https://taskfile.dev/installation/) - `task lint` and `task test` to validate updates to the plugin