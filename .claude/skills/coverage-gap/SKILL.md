---
name: coverage-gap
description: Find production files with no matching test file and draft the missing test file, copying the structure of the nearest existing test. Invoke on demand with a file or directory — writing-tests covers how to write a test, this covers what is untested.
disable-model-invocation: true
allowed-tools: Bash(git ls-files *), Read, Glob, Grep, Write
---

Find and fill test gaps for the given file or directory:

1. Infer the project's test naming convention from existing pairs (e.g. `Foo.cs` ↔ `FooTests.cs`, `foo.ts` ↔ `foo.test.ts`, mirrored under `tests/` or next to the source). If there are no pairs, stop and ask which convention to use.
2. For each production file in scope, check whether its matching test file exists. List the ones with no match, grouped by directory.
3. For the gaps the user confirms (or the one they named), find the nearest existing test in the same area and read it. Draft the missing test file using that test's structure: same framework, fixtures, naming, and assertion style.
4. Write the draft only for the files the user asked about. Don't write tests for every gap unprompted.
5. Don't run the tests or change production code. Report the file path(s) written and the test command to run them.
