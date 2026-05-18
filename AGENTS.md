# Mozilla Thunderbird Rules

This directory contains the codebase for Mozilla Thunderbird, which is contained in
an additional repository that is checked out inside the Mozilla Firefox codebase.

## Code Structure

- The Thunderbird code is contained in source/comm/. That is the primary
  code that these rules apply to.
- The Firefox code is contained in all of the other subdirectories of
  source/.
- source/Agents.md contains guidance for working with both the Firefox
  and Thunderbird code bases. However, there are additional rules
  and exceptions for Thunderbird that are described in this file.
  When Firefox and Thunderbird guidance are at odds, prefer
  Thunderbird guidance.

## Version Control

Both projects are using mercurial repositories.

The Firefox repository was checked out to source/ and the Thunderbird
repository was checked out to comm/ inside of source. Almost all modifications
needed when working on Thunderbird will be in the comm/ directory.

To move files that are version controlled, use the `hg mv` command to ensure
the metadata for the move is recorded in Mercurial.

## Searching

- When searching for symbols, prefer searching just in the comm/ directory.
- To use `searchfox-cli` for the comm/ repository, use the `--repo comm-central` command line argument.
- Prefer ripgrep (`rg`) for text searching.

## Build System

Thunderbird uses the same `mach` build command as Firefox. There are, however, some
additional commands:

To format source code appropriately for Thunderbird, use the `./mach commlint` command
as follows:

```
$ ./mach commlint -l <formatter> [file paths...]
```

Where `<formatter>` is one of `clang-format`, `rustfmt`, or `eslint` depending
on whether the files are C++, Rust, or Javascript files respectively.

## Reviewing Changes

To get a diff that is suitable for reviewing the current working changes, run
the following command in the source/comm/ directory:

```
hg diff
```

To get a diff of the most recently committed changes suitable for reviewing, run
the following command in the source/comm/ directory:

```
hg diff -c .
```

## Making Changes

When starting a new task, always present the user with a plan before proceeding
with any changes.
