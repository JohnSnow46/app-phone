---
name: security-audit
description: Check the project's dependencies for known vulnerabilities using the stack's native audit command and summarize findings by severity. Invoke on demand — the CI templates check build/tests, not vulnerable packages. Reports only; never upgrades anything.
disable-model-invocation: true
allowed-tools: Bash(dotnet list package *), Bash(npm audit *)
---

Audit the dependencies for known vulnerabilities:

1. Detect the stack from the repo: a `*.sln`/`*.csproj` means .NET, a `package.json` means Node. Both present → audit both.
2. Run the native audit:
   - .NET: `dotnet list package --vulnerable --include-transitive`
   - Node: `npm audit`
3. Summarize the output grouped by severity (critical → low): package, installed version, fixed version if the tool reports one, and advisory ID or URL. Say explicitly when there are no findings.
4. If the audit tool can't run (no restore, no network, missing lockfile), report that and the command that failed — don't guess at results.
5. Stop there. Don't bump versions or edit `.csproj`/`package.json`/lockfiles — upgrading can break the build and belongs to `builder`, not this skill.
