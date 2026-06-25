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
- Both code bases are implemented in Javascript, C++, and Rust.

## Version Control

Both projects are using git repositories.

The Firefox repository was checked out to source/ and the Thunderbird
repository was checked out to comm/ inside of source. Almost all modifications
needed when working on Thunderbird will be in the comm/ directory.

## Searching

- When searching for symbols, prefer searching just in the comm/ directory.
- To use `searchfox-cli` for the comm/ repository, use the `--repo comm-central` command line argument.
- Prefer ripgrep (`rg`) for text searching.
- When searching files locally, limit searches to the comm/ directory.
- Unless otherwise instructed, searches can ignore the comm/suite and comm/third\_party directories.

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

### Code Review Guidelines

* Identify deviations from language-specific best practices.
* Identify unhandled error conditions.
* Identify undocumented public-facing interfaces.
* Read code surrounding code even if it's not in the patch by reading files from the filesystem.
* Look at function definitions, class definitions, etc., even if they are in other files to ensure they are being correctly used.

## Making Changes

When starting a new task, always present the user with a plan before proceeding
with any changes.

## Automated Tests

There are multiple kinds of tests:

* xpcshell tests, often referred to as unit tests, are implemented in
  javascript and do not have an application window. They are used to test
  components at lower levels.
* mochi tests, often referred to as browser tests, are also implemented in
  javascript and allow scripting of UI actions such as opening windows,
  clicking on UI elements.

The testing framework is not any of the commonly used testing frameworks, so to
understand how to write tests and check assertions, it is necessary to look at
similar tests rather than assuming any particular testing or mocking framework.

For both mochi tests and xpcshell tests, tests are added with the following
structure:

```
add_task([async] function test_testName() {
  ... // test logic
});
```

Similar to `add_task`, `add_setup` can be used to add a function to be called
before starting any tests in a file.

