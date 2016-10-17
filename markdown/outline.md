# Create PHPantastic Packages!

## Background

PHP's package ecosystem has evolved a ton over the years.

"Back in the day", we tended to post a tarball or zipball up on Freshmeat or
SourceForge or our personal websites, folks would download these, extract, and
then scratch their heads:

- How do you use the code?
- How do you keep the code up-to-date?
- What if the code has dependencies?
- How do you prevent dependency conflicts?

## PEAR

Around 2001, a group formed called PEAR, an acroynm for PHP Extension and
Application Repository. Interestingly, it was neither (PECL is a repository for
extensions that just so happens to use the PEAR installer); it was and is a
collection of packages. It largely solves a lot of the issues presented above:

- How do you use the code?
  PEAR requires usage documentation for each package it publishes.
  Packages are generally installed globally, but can be coaxed to install
  locally, and require that you either provide your own autoloader, or manually
  require class files.
- How do you keep the code up-to-date?
  The PEAR installer allows polling for updates.
- What if the code has dependencies?
  The PEAR installer will install any required dependencies.
- How do you prevent dependency conflicts?
  The PEAR installer does conflict resolution.

One problem with PEAR is that, at least for the first several years, it only
allowed installing packages from PEAR itself, and it had a huge barrier for
entry for adding new packages. In particular, anything potentially touching on
MVC, or integrating multiple packages, was a sure candidate for the ban hammer.

At some point, PEAR added support for "channel servers", which allowed you to
run your own PEAR channel, and allow developers to install packages from these.
This was great, but channel discovery was never implemented. Users can certainly
install your packages via PEAR, but how do they ever find them?

Another problem was installation. By default, PEAR is a global installer, which
means that it's really easy to break things system-wide, if code uses the
default include path. While you *can* install to a local directory, doing so
requires remembering arcane switches, and using them consistently. You can of
course automate these via tools like Phing or Make, but remembering to do so,
and ensuring developers use these project-specific tools can be problematic.

Yet another problem was that installation was not *repeatable*. This meant that
after installation, you needed to commit the installation artifacts to your
repository to ensure that they stay pinned to the versions you installed.
Otherwise you'd need to track what packages you installed as well as the
versions *manually*, and pass those to the PEAR installer. In other words, you
either ended up with a repository bloated with all the code you've installed, or
you had a wicked complex installation setup.

Due to these issues, many felt democratizing PEAR and improving its usability
was a large, uphill battle at best, and, in the worst case, that PEAR was a
failed experiment.

## Composer

In 2011, Nils Aderman and Jordi Boggiano started work on Composer. This project
aimed to simplify and democratize publication and consumption of PHP packages,
providing, specifically:

- An installer that would track both what was installed and what *versions* were
  installed, in order to allow repeatable installations, and thus help keep your
  own package smaller.
- The ability to generate autoloading rules for each package installed, and thus
  allow you to have a single autoloader covering all dependencies.
- The ability to specify additional repositories from which to locate and
  install packages.

Eventually the project expanded to offer more features, but the above answered
pretty much all of the issues found with PEAR.

When you install or update a package with Composer, it writes to a *lock file*.
This file contains a list of the packages installed, as well as versions or
revisions, in order to allow a repeatable installation.

Each package can and should specify autoloading rules. These can be one of
several known standards, including PSR-0 and PSR-4, or you can tell Composer to
generate a class map based on the code present in the package, or you can
specify one or more files to load — which could also include custom autoloaders,
if desired.

Your own package can *also* declare autoloading rules, which means you can have
Composer generate autoloading for your project as well!

In the end, this means your code can usually declare a single autoloader for
your entire project:

```php
require 'vendor/autoload.php`;
```

In terms of democratization, another aspect to Composer is the site
packagist.org. this site allows anybody to register a package; there is no
gatekeeper organization.

Composer searches for packages on Packagist by default, so this is the
recommended location to register open source packages.

You can also create your own Composer repository, and root packages can then
specify repositories to search in addition to Packagist. If you really want, you
can also disable lookups against Packagist, if your security requirements
dictate it.

Composer essentially ushered in modern PHP development, and, along with GitHub
for project hosting, has democratized package development and publication.

But what makes a *good* package?

## Package quality

I am a serial open source author. My day job has me leading the Zend Framework,
Apigility, and Expressive projects, and contributing to projects on which we
depend. I also publish stuff on my own, and contribute to projects I find
interesting or use.

I've both seen and authored a lot of good and bad packages. What follows are my
personal recommendations; I definitely urge you to do some research and soul
searching of your own to define quality.

I tend to think of "quality" with relation to open source packages in a few
terms:

- Ease of usage: does the package provide documentation on how to use it?
  Can potential users determine if they are *allowed* to use the code in their
  project? Can users identify what has changed between versions, and how that
  change might impact their code?
- Ease of contribution: can potential contributors determine how to create
  patches that will be accepted? Can contributors ensure their patches will not
  break established use cases? Can contributors understand how to make code
  consistent?
- Project automation: are tests run automatically? is code sniffed against
  established coding standards automatically? are quality metrics run
  automatically? is documentation built automatically?

I will call these, respectively:

- Developer Experience (DX)
- Collaboration
- Continuous Integration

## DX

### composer.json

I established earlier that Composer has revolutionized how PHP packages are
created, distributed, and consumed. So, the first thing you need to do is create
a `composer.json` that details:

- your project name
- a little bit about what it does
- the license it operates under (more on that later)
- links to support and documentation
- package requirements
- optionally, autoloading rules

Once you have, register your package on Packagist, so folks can start using it!

### README

You need a README file.

Really.

This will be the introduction to your package for many people. They'll find the
project via Packagist or Google, and be taken to the repository, and that
inevitably displays the README file.

No README file? Then the potential user has to browse through the files to see
which one might contain the info they need.

Create one of the following files:

- README
- README.txt
- README.md

(I prefer markdown, as it renders nicely in most VCS browsers.)

What goes in a README file?

- Name of the package. This should be the name users will use to install it!
- Badges. Actually, I find this questionable in most cases, but badges that
  demonstrate current build status or documentation status and links can give
  users some information about your project immediately.
- Installation. Most of the time, this should be `$ composer require
  vendor/package`.
- Basic usage. This should be a quick start at best; just show the most basic
  features, so that potential users can get an idea of the API.
- A link to documentation. Where does the user go for more information?

Speaking of documentation...

### Documentation

Documentation is a must for any package. Yes, you can dive into the source code
to understand how it works, or run PHP Documentor over the source code to
generate API documentation, but *narrative* documentation with copius examples
will help users far better and more quickly get them up-to-speed.

How do you write narrative documentation?

I've played with several formats over the years:

- DocBook XML. This has always been touted as the "gold standard" of
  documentation, as it allows for all sorts of transformations. You can export
  to HTML, or PDF, or ePub, or man pages, or info pages, ... the list goes on.
  The problems, however, include:
  - The documentation sources are not terribly human readable.
  - The documentation sources have a ton of extra markup and nesting that
    distracts from the actual content when editing.
  - You can use a number of extensions, but knowing which ones are present, how
    to use them, or how they might interact can be difficult.
  - The tools for transforming documentation are quite unwieldy, and, in most
    cases, incredibly slow and resource hogs.
- reStructured Text (rST). This format builds on Markdown, and offers a number
  of extensions, providing a fair amount of the flexibility and power of
  DocBook, but with more readable and easily editable source. It comes with its
  own problems, too:
  - As with DocBook, knowing which extensions are available, how they are
    configured, and how they interact is often frustrating.
  - There's still a fair amount of verbosity in the markup, and trivial things
    like indentation can lead to malformed output.
  - The toolchain requires Python, and requires you to install all extensions
    either local to your user or globally, which can lead to inability by
    contributors to render documentation, and thus verify validity of structure.
- Markdown. Markdown is literally everywhere anymore: it's used on GitHub, in
  Slack and Gitter, in StackOverflow. It's universal. It's deliberately
  minimalist, which means some things, like generating a table of contents or
  linking between documents, can require a bit of special knowledge or
  additional tools. However, it can be read by humans, and rendered pretty much
  anywhere.

My personal preference is to use Markdown along with a tool called MkDocs, which
provides the ability to create a table of contents and navigation system for
your documentation. With these two in place, you can then do one of two things:

- Publish your documentation via ReadTheDocs.org, a free service that will
  render your documentation on their site. It can use either rST or MkDocs.
- Publish your documentation via GitHub Pages. To do this, you create an orphan
  gh-pages branch in your project, and use MkDocs to generate the documentation
  that you then commit and push to that branch.

One nice thing about using Markdown for the documentation is that it remains
readable *within GitHub*. (show screenshot)

### Semantic Versioning

A pet peeve of mine, and probably everyone in here, is when they upgrade a
library, only to discover that now their code breaks.

One way you can solve that problem for your users is to adopt semantic
versioning, or semver.

Semver essentially creates three categories of releases:

- Patch releases, also known as maintenance, bugfix, or security releases. These
  should always be backwards compatible, and represent *fixes* to existing
  functionality.
- Minor releases, sometimes known as feature releases. These should also always
  be backwards compatible, and usually represent the addition of new features to
  the library.
- Major releases. These *can* include backwards incompatible changes, though
  they do not need to; however, they typically indicate major changes to the
  project.

Putting it all together, we get versions that look like:

```
MAJOR.MINOR.PATCH
```

This allows developers to pin their dependencies to:

- only patch releases
- patch and minor releases
- all releases

and provides a ton of confidence that updates will not break their code,
*unless* they opt-in to a major release.

Commit to semantic versioning. It can sometimes be difficult, but it keeps you
user focused, and without users, your project is just sheets in the wind.

### Changelog

How does a user find out what new features are available? what bugs were fixed?
what breaking changes were introduced? Moreover, how do they find out *when*
these changes occurred?

Keep a change log.

Actually, that's great advice: http://keepachangelog.com/

The keepachangelog project suggests that:

- Logs are kept in CHANGELOG.md
- Logs are written in Markdown
- Logs have a standard structure (more on that below)
- Logs are *human readable*, and targeted at *changes impacting users*

The structure of a log is essentially this:

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

Each section represents a single version, each with headings indicating the
*type* of change. 

Update these *as you add changes to the repository*. You can require
contributors to add documentation to the change log as part of the acceptance
process, or have maintainers write entries for changes they merge and push to
the repository.

Things to log:

- New API features: new classes, new methods, etc.
- Items that have been deprecated, *and what to do instead*.
- Items that have been removed.
- Changes in behavior: different return types, additional arguments, etc.
- Bugfixes: detail the bug itself, and then any relevant details of the fix.

Things NOT to log:

- Additional tests written
- Coding standards fixes
- Changes to tooling or infrastructure (unless it has a user impact!)
- Typo fixes

In general, keep the change log *user focused*, detailing what may impact
*consumers* of your code.

### License

How does a potential user of your package know whether or not they are *allowed*
to do so?

Via its license.

Every package should have a license. As a consumer, *you should not use
unlicensed code*, as it potentially opens you up to legal problems.

So, how should you license your code?

- GNU Public License (GPL), the original "copyleft" license. Use this if you
  want to require that those who extend your code or fork it publish the source
  code, under the same license. For some businesses, this can be problematic,
  particularly as the key concepts of linking and distribution have not been
  fully tested, and could have broad implications.
- Apache License. This allows redistriution without requiring the same license,
  but requiring preservation of copyright/trademark/attribution notices, and, if
  any changes were made to a file, a list of changes introduced by the new
  distribution.
- MIT and BSD. These are permissive licenses that essentially only say that
  redistribution requires retainment of the original copyright notices. These
  are generally considered business friendly. The BSD license has two versions,
  the 2-clause and 3-clause variants; the 3-clause variant includes a clause
  indicating that the name of the author/organization may not be used to promote
  derivative products.
- Commercial license. In this particular case, typically an end-user needs to
  enter a contract with you in order to use the software.

Which should you choose? 

It depends on the goals you have for your package, what values you promote for
your contributors and users, and how you envision users using your software in
their products.

Most PHP software tends to be MIT or BSD licensed, as these are permissive, and
do not preclude usage within commercial works.

Include your license as the file:

- LICENSE
- LICENSE.txt
- LICENSE.md

GitHub and GitLab will recognize these files, and often be able to determine the
general license type in order to display that information for users as part of
the project dashboard. [Show a screenshot here]

## Collaboration

We've talked about features for consumers, but now it's time to talk about the
other side of the community: collaborators and contributors.

An open source project is only truly viable once you have multiple contributors.
Otherwise, it's open to the bus factor; if you step in front of a bus, who will
take up the project?

So, we need to make contribution painless and fun for developers, so they can
help improve the project.

The first thing you should do is create a CONTRIBUTING.md file, in which you
will document the process for contributing to your project. What should it
contain?

### Coding standards

There's nothing quite as infuriating as opening a file, only to find the
formatting is completely inconsistent throughout:

- classes, methods, and variables named using both camelCase and snake_case
- varying amounts of space around operators
- varying brace positions
- varying indentation

etc. etc. etc.

These sorts of things make contribution hard, as the reader has additional
cognitive overhead in trying to decipher the structure of the code.

On top of this, you'll often get unrelated changes, where a contributor is
simply changing code formatting, but not intent or functionality, simply so they
can better read it, or out of personal preference.

So, pick a coding standard, and enforce it.

What should you use?

First, **do not write your own standard**. Writing your own standard is one of
the most egotistical things you can do, and can and will blow up in your face.
Instead, use an existing standard. Which one?

Use PSR-1 and PSR-2.

Why?

Because:

- they are essentially codifying commonalities found in many existing project
  standards.
- they are not tied to a specific project, but rather a standards body; there's
  less of a political statement to be made ("We choose Framework X!").
- there are existing tools for both linting against the standards, as well as
  fixing common issues.

Pick a standard, and link to it in your CONTRIBUTING.md file.

And provide a tool for verifying CS. These are great ones:

- php-cs-fixer. This started off as a pet-project of Fabien Potencier, and later
  was spun into the FriendsOfPHP organization. It has support for PSR-1 and -2,
  though, by default, it does a **lot** more than that, which can lead to a lot
  of extra configuration to restrict to those standards.
- phpcs is the grandparent of all CS tools. In recent years, it has been adopted
  by the content agency Squiz, and is being continually updated. Its PSR-1/2
  checkers do *only* those checks, but you can extend the ruleset as well.

Provide the tool you'll use to check that code follows the standard, include
configuration for that tool, and document how to execute the tool.

### Testing

Require tests. Period.

If you don't have tests:

- you don't know that the code does what it's supposed to
- you don't even know for sure what the code is supposed to do, as there is no
  code actively showing usage.
- you don't know if a change breaks existing use cases
- you don't know if a change fixes an existing bug

Tests are not just a tedious task getting in the way of writing your code. They
are a way to *specify* how your code should be consumed.

So, how should you test? It depends on your project goals.

- Unit testing is a way to exercise the API and demonstrate expected behavior
  when invoking the code. For this, we have PHPUnit. (Yes, other unit test
  software exists, but this one is the most well known and widely used. Just use
  it.)
- Behavior testing is a way to write *usage stories* that details the usage
  context, the actions taken, and the expected outcomes, generally using a
  domain specific language; proponents often claim that BDD focuses on behavior
  and design of code versus verification and structure. BDD is common
  particularly for domain modeling and business-facing code. Behat and PHPSpec
  are excellent tools for this type of testing.

Document what tool you're using, where tests go, and how to run them. Provide a
configuration file for your chosen testing tool(s). Most importantly, provide
tests!

### composer.lock

Remember how Composer keeps track of what you've installed? This is in a "lock
file".

We usually think about keeping the lock file only when considering an
application. However, having a lock file in your package can help *contributors*
as well:

- Contributors then know exactly what version you've tested against.
- You can find out if code works against both the latest dependencies as well as
  the earliest supported dependencies, letting you discover when there may be
  breaking changes in code you depend upon, or you've started using newer
  features of them.

Some project leads feel committing the composer.lock in library code to be
unwieldy. My experience is that it has helped us narrow down exactly where
breaks related to dependencies occur, so we can make informed decisions about
what we do and do not support.

**tl;dr**: commit your `composer.lock`.

### Documentation

We already talked about this with regards to developer experience, but you also
need to think about documentation from the contributor's point of view. Choose a
format that does not require too much additionaly learning on the part of the
developer, and provide expectations about what should and should not be
documented. Make sure that the contributor knows where documentation goes, and
how to build documentation in order to preview changes.

### Templates

GitHub offers the ability to provide templates for issues and pull requests.
Essentially, when a user opens a new issue or pull request, it will be
pre-populated with the template, and the user then fills in details.

Why would you do this?

One of the most interesting use cases is provided by the PHP project itself: to
ensure that the reporter do due diligence. The PHP project requires that a
reporter:

- Detail the problem
- Provide the minimal code required to reproduce the problem
- Detail the expected result
- Detail the actual result received

This kind of information is invaluable to somebody trying to *fix* an issue.
Later, somebody can reference this information as well to understand if their
use case has been fixed, or to understand how what they are doing varies, so
they can determine if a new report is required.

In the case of new features, you an omit the "actual result" aspect, and you now
have the beginnings of documentation.

Think about what information you feel would be most helpful when new bug reports
or features are provided, and consider providing a template for reporters to
fill out.

This leads us to our next subject.

### Being Human

Humans are messy. While we, as developers, like to think we're logical and
rational, the fact is, we typically are not when interacting with one another.

Additionally, on the internet, it's easy to use the fact that the recipient of
your words is not directly in front of you, and thus say something you'd never
say one on one. And this is how projects start to fall apart.

On top of this, our choice of words often can have unfortunate consequences.
It's easy to say, "hey, guys," meaning everyone, but this can feel like you're
excluding women.

Ideally, you and everyone contributing to your project should be actively
working towards inclusivity, creating a welcoming environment for everybody to
participate.

So, while many consider it a touchy subject, I always advocate for a code of
conduct. Include it in CONDUCT.md, and link to it from your contributor's guide.

It can be as simple as Wil Wheaton's "Don't be a dick!"

But I'd recommend getting a bit more specific. Generally, you should be
detailing what behaviors you value, and focus less on how you will deal with
transgressions. Use the code of conduct as an opportunity to help educate the
folks working on your project.

As with coding standards, it's often best to choose an existing code of conduct.
Some popular ones include:

- Contributor Covenant has been adopted by many projects, across many languages,
  and the most recent version (1.4) details both acceptable and unacceptable
  behavior, and provides scope for behavior.
- Code Manifesto is also well-received, and focuses entirely on values of
  inclusiveness.

For Zend Framework, we adopted Code Manifesto.

tl;dr: create or adopt a code of conduct, document it in CONDUCT.md, and link to
it from your contributor's guide.

## Continuous Integration

Having documentation is great. But if the rendered documentation is never
updated, you'll get frustrated users.

Having tests is great. But if you never run them, how can you be certain they're
passing?

Having a coding standard is great, but how can you verify the code actually
follows it?

We need to automate.

### Composer scripts

The first automation you need is for yourself and your contributors. Your tools
rarely change, but when they do, muscle memory often leads us to use the old
tool for some time. Additionally, some of the commands can be quite verbose.

Use composer scripts to reduce the cognitive load and reduce repetition!

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

You can also provide build automation. As an example:

```javascript
module.exports = function (grunt) {
  require('load-grunt-tasks')(grunt);

  grunt.initConfig({
    exec: {
      php_check: {
        cmd: "composer check"
      }
    },
    watch: {
      php: {
        files: ['src/**/*.php', 'test/**/*.php', 'phpunit.xml.dist', 'phpcs.xml'],
        tasks: ['exec:php_check']
      }
    }
  });

  grunt.loadNpmTasks('grunt-contrib-watch');
  grunt.loadNpmTasks('grunt-exec');

  grunt.registerTask('default', ['watch:php']);
};
```

Users can then open a terminal, run `grunt`, and unit tests and CS checks will
be performed each time a PHP file is changed in the src or test directories, or
if PHPUnit or PHPCS configuration changes!

### Continuous Integration Services

In the collaboration section, we indicated you should:

- Pick a coding standard, *as well as a verification tool and its configuration*.
- Write tests, *pick one or more testing tools, and provide their configuration*.
- Write documentation, and *provide configuration for tools to render it*.

If you've done that, you can also setup continous integration.

What is continuous integration? Essentially, it's a server that gets notified
when you make a change, and then runs the tools you specify in order to validate
the project. Many of these systems can also be configured to run against pull or
merge requests, giving your collaborators an idea of whether or not more work is
necessary, and giving you more confidence in the changes introduced.

What should you use?

- Coveralls will run unit tests and create code coverage reports; it can also
  accept code coverage reports from other CI services. These two tasks are all
  it does.
- Circle CI is aimed primarily at acceptance testing and deployment, but can
  also run PHPUnit; support for other tools is more difficult, however.
- Scrutinizer CI will run PHPUnit, phpspec, behat, and phpcs automatically if
  rules are found for them already; it also runs tools for analyzing code
  coverage, cyclomatic complexit, and more, in order to give your code a
  "grade"; you can easily find yourself going down the rabbit hole with these.
  Finaly, you can specify your own tools to run as well.
- Travis CI is the most-used CI tool I've seen by PHP projects. It provides a
  huge array of PHP versions to test against, the ability to specify exactly
  what should happen during the installation phase as well as when running the
  script, and a host of integrations.

With all of these, you can integrate directly with GitHub, GitLab, or BitBucket
in order to do two things:

- Trigger builds on commits
- Provide build status to pull or merge requests

Each has its own configuration file to drop into the project as well. Make sure
you do this, and that you setup the integration with your repository hosting
provider.

### Documentation automation

This one is a bit harder, as it generally requires both a build process, as well
as publishing the build artifacts.

Three solutions are generally used:

- Use rtfd.org. This is the simplest solution, and ensures that documentation is
  always up-to-date; you can even point a DNS CNAME to the project, making it
  appear to be part of your project domain. However, you have very little
  control over the look-and-feel of the generated documentation.
- Use a git hook to build on push. Essentially, whenever you are pushing to a
  specific branch, trigger a documentation build as well. This might, for
  instance, introduce another commit with built documentation in the `docs/`
  folder (which GitHub Pages now recognizes), push built docs to another
  branch of the repo, or deploy the documentation.
- Use your CI server to build documentation and deploy it. I blogged extensively
  on this option.

Automating documentation deployment is a great way to get new collaborators
excited, as they can see the results of their changes very quickly. These same
collaborators often then continue on to committing patches later.

### Approvals

Ideally, every change shold be reviewed by somebody else. I can't tell you how
many times I've had reviewers point out errors in my code, or suggest
improvements to my changes that have a huge impact on the code base or usage.

You can automate this in a few ways.

- Since this past month, GitHub now provides a setting that will require reviews
  before merging (https://help.github.com/articles/enabling-required-reviews-for-pull-requests/);
  if this is specified, somebody with write access is required to approve
  changes before a merge is allowed.
- LGTM (looks good to me) allows you to specify which users can approve a pull
  request, and how they provide approval within comments; it only works with
  GitHub protected branches.
- pullapprove.com is an integration service that allows you to provide
  fine-grained review workflows; if combined with GitHub's feature to require
  passing status checks, it can be used to enforce review.

The GitHub integrations are the easiest to setup, and pullapprove gives greater
control overall.

## Resources

Project structure and tools vary between projects. Today, I've shown you have
options with regards to:

- Licensing
- Documentation format and build tools
- Contribution guides and codes of conduct
- QA tooling

Figuring out what all you should have present can often be a chore, and it's
easy to forget something.

One option available for you is a new tool called "Construct". You install it
globally first:

```bash
$ composer global require jonathantorres/construct
```

and then use its vendor binary to generate a new project:

```bash
$ construct generate my/cool-project
```

A variety of flags or an opt-in interactive mode allow you to customize the
generated project, including the ability to:

- specify a namespace for your PHP code
- select a license
- select a testing framework
- generate php-cs-fixer configuration
- generate LGTM configuration
- generate GitHub issue/PR templates
- generate stub documentation in the docs/ dir
- opt in to contributor covenant

The project is still relatively new, and adding options regularly; keep an eye
on it.

Another option is cloning thephpleague/skeleton, which provides configuration
for the various tools used by League Packages, and which include many of the
items discussed in this talk.

I recommend that once you find a set of tools you prefer, you setup a skeleton
repository that you can clone for any new package you crate.

## Summary

PHPantastic packages focus on:

- Developer Experience
- Collaboration
- Continuous Integration

## Thank you!
