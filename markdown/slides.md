## Who is this guy?

- Serial OSS creator and contributor
- OSS project lead
- (Co-)Maintainer of triple digit numbers of packages

#### Note

- I've been contributing to open source projects since 2001.
- I'm project lead for Zend Framework, Apigility, and Expressive.
- The ZF and Apigility projects alone number over a gross of packages.

---

# Some History

---

## The Bad Old Days

#### Note

- I'd like to do fragment images for each of a tarball; a combo image of the
  Freshmeat and SourceForge logos; and an RSS feed icon.

---

## Problems

- How do you use the code? <!-- .element: class="fragment" -->
- How do you keep the code up-to-date? <!-- .element: class="fragment" -->
- What if the code has dependencies? <!-- .element: class="fragment" -->
- How do you prevent dependency conflicts? <!-- .element: class="fragment" -->

#### Note

- Many libraries tackled one or more of these, but there was no *consistency*,
  which is what led to issues.

---

## PEAR

### **P**HP **E**xtension
### and **A**pplication **R**epository

#### (It's a component library) <!-- .element: class="fragment" -->

#### Note

- Interestingly, other than being the installer for PECL, the PHP Extension
  Community Library, it does not host extensions.
- Nor does it host applications.
- It is, however, a solid component library.

---

## Solutions

- Documentation is required for all new code. <!-- .element: class="fragment" -->
- PEAR installer can update code. <!-- .element: class="fragment" -->
- PEAR installer will install required dependencies. <!-- .element: class="fragment" -->
- PEAR installer detects and resolves dependency conflicts. <!-- .element: class="fragment" -->

#### Note

- How do you use the code?
- How do you keep the code up-to-date?
- What if the code has dependencies?
- How do you prevent dependency conflicts?

---

## Huzzah!

#### Note

- Some celebratory picture here.

---

## But...

#### Note

- I want images for each of:
  - monolith (representing single, monolithic repository)
  - gatekeeper (barrier to entry for new packages)
  - each of the words "Applications" and "Integrations", with a red, forbidden
    circle around them.

---

## Channel servers

- Added to the installer in 2006. <!-- .element: class="fragment" -->
- Pirum's introduction in 2009 simplified channel maintenance. <!-- .element: class="fragment" -->
- Channel discovery was never solved. <!-- .element: class="fragment" -->

---

## Installer issues

- Global by default. <!-- .element: class="fragment" -->
- Non-repeatable installations. <!-- .element: class="fragment" -->

#### Note

- You *can* install per-project, but it's difficult to accomplish, and
  essentially requires that you write a script for invoking the commands.
- You had to either commit the artifacts, or manually track versions and
  script installation of those specific versions.

---

placeholder

#### Note

- Maybe a "Rejected" stamp?
- Democratization of PEAR, usability of the installer, and channel discovery
  were going to be difficult to accomplish from within the organization.

---

# Composer

#### Note

- Get the logo here.
- Introduced in 2011 by Nils Aderman and Jordi Boggiano.

---

## Initial Features

- Per-project installer. <!-- .element: class="fragment" -->
- Dependency resolution and conflict mitigation. <!-- .element: class="fragment" -->
- Repeatable installs. <!-- .element: class="fragment" -->
- Open, unmoderated package submission. <!-- .element: class="fragment" -->
- Ability to use other repositories for package discovery and installation. <!-- .element: class="fragment" -->
- Generation of autoloading for installed dependencies. <!-- .element: class="fragment" -->

#### Note

- Installation generates a "lock file", which details exactly which packages
  were installed, and at which versions. This allows repeatable installation,
  without requiring the actual installation artifacts be present.
- Packagist by default; specify repositories in your root package.
- Autoload with existing standards such as PSR-0 and PSR-4, or ask it to scan
  the package for classes in order to generate a static class map, or specify
  one or more files to load.

---

## Solutions

- Documentation is not covered at all. <!-- .element: class="fragment" -->
- Composer can update code. <!-- .element: class="fragment" -->
- Composer will install required dependencies. <!-- .element: class="fragment" -->
- Composer detects and resolves dependency conflicts. <!-- .element: class="fragment" -->

#### Note

- How do you use the code?
- How do you keep the code up-to-date?
- What if the code has dependencies?
- How do you prevent dependency conflicts?

---

# Package Quality

#### Note

- Composer ushered in modern PHP development, along with GitHub for project
  hosting. It solves the problem of package democratization.
- But it doesn't dictate anything whatsoever about package quality. That's where PEAR's 
  gatekeeper mentality had benefits.
- What makes a good package?

---

## Criteria

### Developer Experience (DX) <!-- .element: class="fragment" -->
### Collaboration <!-- .element: class="fragment" -->
### Automation <!-- .element: class="fragment" -->

#### Note

- composer.json. Documentation. License. Changelog.
- how do you accept patches? do you have tests? do you have a style guide?
- are tests and CS sniffs run automatically? is documentation built
  automatically? are new releases available immediately?

---

# Developer Experience

#### Note

- We'll first turn to the developer experience, because it's likely the first
  aspect of your package that anybody will encounter.

---

## composer.json

```json
{
  "name": "your/package",
  "description": "The primary purpose.",
  "license": "BSD-3-Clause",
  "support": {
    "docs": "https://your-org.github.io/your-project",
    "irc": "irc://irc.freenode.net/your-project",
    "issues": "https://github.com/your-org/your-project/issues",
    "source": "https://github.com/your-org/your-project"
  },
  "require": {
  },
  "autoload": {
  }
}
```

#### Note

- Many more fields, but those above will help users find out more about your
  package when they are searching for functionality.
- Don't forget to register the package on Packagist, or with your organization's
  Composer repository!

---

## README.md

<ul>
  <li class="fragment">Name of the package <em>as it will be installed</em>.</li>
  <li class="fragment">Badges.</li>
  <li class="fragment">Installation.</li>
  <li class="fragment">Basic Usage.</li>
  <li class="fragment">Links to documentation.</li>
</ul>

#### Note

- Think of the README as your chance to make a first impression; you need to
  give the user enough information that they will want to come back and
  explore in more depth.
- Badges can give some meaningful information, such as how will maintained the
  package is, current development status, and more.
- Installation should usually be simply `composer require`.
- Speaking of documentation...

---

## Documentation

- DocBook XML <!-- .element: class="fragment" -->
- reStructured Text (rST) <!-- .element: class="fragment" -->
- Markdown <!-- .element: class="fragment" -->

#### Note

- Narrative documentation: use cases, tutorials, etc.
- Each format has pros and con.
- Current preference is GitHub-flavored Markdown using Mkdocs.

---

## Markdown

- Image showing rendering in GitHub
- Image showing rendering via MkDocs

#### Note

- One pro of Markdown is you can read it *in* GitHub, and even edit it in-place.
- And yet you can render that documentation in a variety of ways, including with
  tables of contents.

---

## Semantic Versioning

### X.Y.Z

<ul>
  <li class="fragment">Patch releases (Z): always backwards compatible.</li>
  <li class="fragment">Minor releases (Y): always backwards compatible, but with new features.</li>
  <li class="fragment">Major releases (X): major additions or changes to the project; <em>may</em> introduce compatibility breaks.</li>
</ul>

#### Note

- Part of user experience is messaging *how* releases happen, and *what* will be in releases.
- Using semantic versioning helps users plan how and when to update your package.

---

## CHANGELOG.md

### http://keepachangelog.com/

<ul>
  <li class="fragment">Logs are kept in <code>CHANGELOG.md</code></li>
  <li class="fragment">Logs are written in Markdown</li>
  <li class="fragment">Logs have a <em>standard structure</em></li>
  <li class="fragment">Logs are <em>human readoable</em></li>
  <li class="fragment">Logs detail changes <em>impacting users</em></li>
</ul>

#### Note

- How does a user find out what new features are available? what bugs were fixed?
- Moreover, *when*?

---

## Keep A ChangeLog

```markdown
# Change Log
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/) 
and this project adheres to [Semantic Versioning](http://semver.org/).

## X.Y.Z - DATE

### Added

### Changed

### Deprecated

### Removed

### Fixed
```

#### Note

- Update the changelog *as you introduce changes to the repository*.
  - Require that as part of contribution
  - Or do it as part of merging.
- Filling out the changelog often helps us better understand the change being introduced, too!

---

## What to log

<ul>
  <li class="fragment">New API features.</li>
  <li class="fragment">Deprecations, and <em>how to resolve them</em>.</li>
  <li class="fragment">API removals.</li>
  <li class="fragment">Changes in <em>behavior</em>, <em>signatures</em>, etc.</li>
  <li class="fragment">Bugfixes: <em>what the bug was</em>, and how it was resolved.</li>
</ul>

---

## What *not* to log

- Testing additions. <!-- .element: class="fragment" -->
- Coding standards fixes. <!-- .element: class="fragment" -->
- Changes to tooling or infrastructure. <!-- .element: class="fragment" -->
- Typo fixes. <!-- .element: class="fragment" -->

#### Note

- The point is that the changelog is *user facing* and *user focussed*. If a
  change does not impact how they use the code, don't detail it. That's what
  your version control logs.

---

## LICENSE.md

How does a user know what they are *allowed* to do with your code?

- GNU Public License (GPL) <!-- .element: class="fragment" -->
- Apache License <!-- .element: class="fragment" -->
- MIT and BSD licenses <!-- .element: class="fragment" -->
- Commercial License <!-- .element: class="fragment" -->

#### Note

- GPL governs linking and distribution, which has unexpected implications for deploying sites for clients.
- Apache requires noting all changes within each file changed when redistributing.
- MIT/BSD are permissive, and only require copyright notices are retained. BSD 2 vs 3 clause: 3 clause indicates name of the author or org cannot be used to promote derivative products.

---

- Show screenshot of license display in github

#### Note

- Most licenses will be auto-detected by GitHub and GitLab, and displayed as part of the project dashboard

---

# Collaboration

#### CONTRIBUTING.md <!-- .element: class="fragment" -->

#### Note

- An open source project is only as viable as the community of contributors it builds.
- Bus factor
- Document how collaborators can work with your project, via a CONTRIBUTING.md
  file.

---

## Templates

How to *report* issues, and how to *submit* changes.

- Detail the problem. <!-- .element: class="fragment" -->
- Provide the minimal code required to reproduce the problem. <!-- .element: class="fragment" -->
- Detail the expected result. <!-- .element: class="fragment" -->
- Detail the actual result. <!-- .element: class="fragment" -->

#### Note

- For new features:
  - Detail what use case it solves
  - Detail usage
  - Detail what the code will do

---

## Coding Standard

<ul>
  <li class="fragment"><strong>Do not roll your own.</strong></li>
  <li class="fragment">Use PSR-1/2</li>
</ul>

#### Note

- You want code to look like it has a single author.
  - Less cognitive overhead
  - Reduces non-related changes based on developer preferences
- Writing your own is narcissistic. You are not a special flower.
- PSR-1/2 were the result of surveying *how existing projects already coded*.
  It's a standard built on empirical knowledge.

---

## Provide CS Tooling

- php-cs-fixer <!-- .element: class="fragment" -->
- phpcs <!-- .element: class="fragment" -->
- Provide configuration. <!-- .element: class="fragment" -->
- Provide Composer scripts to invoke them. <!-- .element: class="fragment" -->
- Document usage in CONTRIBUTING.md <!-- .element: class="fragment" -->

---

## Tests

Require tests. Period.

- Living usage document. <!-- .element: class="fragment" -->
- Demonstrates code does what it should. <!-- .element: class="fragment" -->
- Prevents breakage by later changes. <!-- .element: class="fragment" -->

---

## Types of tests

- Unit tests. <!-- .element: class="fragment" -->
- Integration tests. <!-- .element: class="fragment" -->
- Behavior tests. <!-- .element: class="fragment" -->

#### Note

- There are other testing types; for reusable packages, however, these are what
  you will typically encounter.
- BDD let's you write usage stories; typical usage is for domain modeling or
  business-facing code. Behat and phpspec.

---

## composer.lock

Commit it!

- Contributors test against known working versions. <!-- .element: class="fragment" -->
- Allows testing latest and lowest dependencies. <!-- .element: class="fragment" -->
  - Identify when you are using newer features. <!-- .element: class="fragment" -->
  - Identify when a dependency breaks. <!-- .element: class="fragment" -->

---

## Documentation

<ul>
  <li class="fragment">Detail how documentation is <em>written</em>.</li>
  <li class="fragment">Detail <em>what</em> should and should not be in documentation.</li>
  <li class="fragment">Detail how to <em>build</em> documentation.</li>
  <li class="fragment"><em>Require</em> documentation for any new features.</li>
</ul>

---

## Being Human

CONDUCT.md

- Contibutor Covenant <!-- .element: class="fragment" -->
- Code Manifesto <!-- .element: class="fragment" -->

#### Note

- Humans are messy. "Don't be a dick" seems reasonable, but specificity often
  matters.
  - *Exclusion* can be as simple as, "hey, guys" (vs the *inclusive*, "hey, folks")
  - Separation in time and space is not a license to be rude.
- CC v1.4 details acceptable and unacceptable behavior, and provides scope for
  behavior.
- Code Manifesto focuses on values of inclusiveness.
- Link the CONDUCT.md from your CONTRIBUTING.md

---

# Automation

#### Note

- Why is automation important to a project?
- Because it allows you to:
  - run repetitive tasks easily or without effort
  - verify independently the results of changes
  - publish documentation without intermediary steps
  - etc.
- So, where do we start?

---

## Composer scripts

Automate the tools you use!

```json
"scripts": {
    "build": [
        "@cs-check",
        "@test"
    ],
    "cs-check": "phpcs --colors",
    "cs-fix": "phpcbf --colors",
    "test": "phpunit --colors=always",
    "test-coverage": "phpunit --colors=always --coverage-clover clover.xml",
    "upload-coverage": "coveralls -v"
}
```

#### Note

- These are just some ideas. The main thing is that collaborators:
  - do not need to know which tools you're specifically using
  - do not need to know what switches to use
  - can get tab completion (most shells offer Composer command tab completion
    that will pick these up!

---

## Automate development

Shift test and CS runners to the background.

```javascript
var gulp = require('gulp');
var shell = require('gulp-shell');

gulp.task('default', ['watch']);
gulp.task('php_check', shell.task('composer check'));
gulp.task('watch', function () {
  gulp.watch(
    ['phpunit.xml.dist', 'phpcs.xml', 'src/**/*.php', 'test/**/*.php'],
    ['php_check']
  );
});
```

#### Note

- There are other ways to accomplish this, but this is one: use grunt or gulp to
  automate running of unit tests and CS checks.
- Run `gulp` or `grunt` in a terminal you can see, and then start editing; when
  changes are discovered, they'll run the sniffs and tests.
  - You can even use notification libraries so that errors are raised via your
    system notifications!
- This sort of automation helps you while developing; you don't need to context
  switch *unless you cause a build failure*.

---

## Continuous Integration

Ensure tasks are run automatically, *against all supported platforms*.

- Trigger builds on commits <!-- .element: class="fragment" -->
- Provide build status to pull/merge requests <!-- .element: class="fragment" -->

#### Note

- We've already chosen our coding standard sniffer and unit test library,
  configured each, and provided a script for running them.
- CI automates that so we can run them on each supported platform.

---

## Continuous Integration

### Platforms

- coveralls.io <!-- .element: class="fragment" -->
- Circle CI <!-- .element: class="fragment" -->
- Scrutinizer CI <!-- .element: class="fragment" -->
- Travis CI <!-- .element: class="fragment" -->

#### Note

- Coveralls *processes* coverage reports *provided by other CI platforms*
- Circle CI is primarily targeting acceptance testing and deployment.
- Scrutinizer runs a plethora of tools, and gives you a "grade".
- Travis CI lets you run anything you want. More work, but more flexibility.

---

## Documentation Automation

Documentation is no good unless published.

- Simplest: use rtfd.org <!-- .element: class="fragment" -->
- Harder: create a custom webhook, and build when triggered <!-- .element: class="fragment" -->
- Harder: use your CI server to build and deploy <!-- .element: class="fragment" -->

#### Note

- rtfd.org supports mkdocs and reStructuredText. You can even point a DNS CNAME
  at the final rendered docs. However, you have little control over the design.
- Hosting a webhook yourself requires that you have a public server that also
  has your entire build chain and toolset.
- Building on your CI server requires ensuring you install your build chain and
  toolset, as well as allow it to deploy the final documentation.

---

# Some final thoughts

---

## Package Starters

- johnathantorres/construct <!-- .element: class="fragment" -->
- thephpleague/skeleton <!-- .element: class="fragment" -->
- roll your own! <!-- .element: class="fragment" -->

---

## PHPantastic Packages

- Developer Experience <!-- .element: class="fragment" -->
- Collaboration <!-- .element: class="fragment" -->
- Automation <!-- .element: class="fragment" -->

---

## Thank You!

Matthew Weier O'Phinney

@mwop

https://mwop.net/

Feedback? https://joind.in/talk/5c756
