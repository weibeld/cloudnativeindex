# Scripts

The sripts in this directory perform validations on the data files in the root directory of the repo. They are invoked from the [_Validate YAML files_](../.github/workflows/validate-yaml-files.yml) GitHub Action workflow as well as from the [pre-commit](../hooks/pre-commit) hook.

## General notes

1. All scripts must be executed from the root directory of the repository and the scripts may assume the current working directory to be the root directory.
1. Each script may have specific dependencies which are declared in its leading comment.
1. The [`_commons.sh`](_commons.sh) file contains utilities (such as shell functions) that are used by multiple scripts. This file must be sourced by each script that wants to make use of it.

## Portability considerations

The scripts may be run in very different environments, such as:

- On a personal workstation running Linux or macOS (if invoked from the pre-commit hook).
- In a container with a possibly very stripped down environment such as [Alpine](https://www.alpinelinux.org/) or [BusyBox](https://busybox.net/) (if invoked from GitHub Actions).

To make the scripts as portable as possible, they should adhere to the following restrictions:

1. Run on sh rather than Bash (i.e. use `#!/bin/sh` rather than `!#/bin/bash`) since Bash is not available in environments such as Alpine or Busybox.
1. Avoid `echo -e`, `echo -n`, etc. since these options are not supported in some environments (e.g. in sh on macOS). Use `printf` instead.
1. Avoid using shell arrays since they are not supported by sh.