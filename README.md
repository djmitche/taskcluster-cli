# TaskCluster CLI Client

[![Task Status](https://github.taskcluster.net/v1/repository/taskcluster/taskcluster-cli/master/badge.svg)](https://github.taskcluster.net/v1/repository/taskcluster/taskcluster-cli/master/latest)

## Overview

TaskCluster CLI is a command-line client offering control and access to
taskcluster from the comfort of your command-line. It provides utilities
ranging from direct calls to the specific API endpoints to more complex and
_practical_ tasks like listing and cancelling scheduled runs.

## Usage

For a list of all commands run `taskcluster help`, detailed information about
each command is available with
`taskcluster help <command> [<sub-command> [...]]`. You can also use the `-h`
or `--help` parameter to get a command's help information.

### Installation

To install, download the `taskcluster` binary for the latest release your
platform, and run it!  On POSIX platforms you will need to `chmod +x` of
course.

<!--
These no longer work since the artifact has expired

 * [darwin-amd64](https://index.taskcluster.net/v1/task/project.taskcluster.taskcluster-cli.latest/artifacts/public/darwin-amd64/taskcluster)
 * [freebsd-386](https://index.taskcluster.net/v1/task/project.taskcluster.taskcluster-cli.latest/artifacts/public/freebsd-386/taskcluster)
 * [freebsd-amd64](https://index.taskcluster.net/v1/task/project.taskcluster.taskcluster-cli.latest/artifacts/public/freebsd-amd64/taskcluster)
 * [linux-386](https://index.taskcluster.net/v1/task/project.taskcluster.taskcluster-cli.latest/artifacts/public/linux-386/taskcluster)
 * [linux-amd64](https://index.taskcluster.net/v1/task/project.taskcluster.taskcluster-cli.latest/artifacts/public/linux-amd64/taskcluster)
 * [openbsd-amd64](https://index.taskcluster.net/v1/task/project.taskcluster.taskcluster-cli.latest/artifacts/public/openbsd-amd64/taskcluster)
 * [windows-386](https://index.taskcluster.net/v1/task/project.taskcluster.taskcluster-cli.latest/artifacts/public/windows-386/taskcluster.exe)
 * [windows-amd64](https://index.taskcluster.net/v1/task/project.taskcluster.taskcluster-cli.latest/artifacts/public/windows-amd64/taskcluster.exe)
-->
 * [linux-amd64](https://github.com/taskcluster/taskcluster-cli/releases/download/v0.9.0/taskcluster-linux-amd64)
 * [darwin-amd64](https://github.com/taskcluster/taskcluster-cli/releases/download/v0.9.0/taskcluster-darwin-amd64)

## Development

### Requirements

We currently support Go versions starting at 1.8; the project may or may not
build on earlier versions.

Please note that the default repositories for popular Linux distributions (e.g.
Ubuntu) do not carry that version of Go. We recommend you download the latest
version of the language from [the official website](https://golang.org/dl/).

### Building

Getting the source is as simple as running the following command in your shell.
Go will download the source and set up the repository in your `$GOPATH`.

```
go get -d github.com/taskcluster/taskcluster-cli
```

To actually build the application, simply run `make` in
`$GOPATH/github.com/taskcluster/taskcluster-cli` which will generate the
executable `taskcluster` in the root of the source.

### Dependency vendoring

The dependencies are managed through the
[govendor](https://github.com/kardianos/govendor) tool, but its use should be
transparent when building the project. After cloning the project, running
`govendor sync` will download the various dependencies and ensure that they
are at the version specified in the _vendor/vendor.json_ file, so that
everyone uses the same dependencies at the same version. The `make` process
automatically runs that command before building.

To add a new dependency to the project, simply run
`govendor fetch <go-import-url>` to add it to the list of dependencies. To
update all dependencies to their latest version, run `govendor fetch`. More
commands are described on the govendor project page.

### APIs

The API-related commands (`apis/`) are generated from the TaskCluster reference
data.  When that data changes, the commands can be updated automatically:

```
make generate-apis
```

### Commands

We are using [cobra](https://github.com/spf13/cobra) to manage the various
commands and sub-commands that are implemented in taskcluster-cli.

Each command is a instance of the `cobra.Command` struct, and is dynamically
registered at run-time in the command tree (in `func init() {...}`). Thus,
commands are registered as an import side-effect. Commands are implemented in
sub-packages.

To add a new command, create a new sub-package under `cmds` and add an import
for that sub-package to `subtree_import.go`, keeping the imports in order.
