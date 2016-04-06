# SOFT MANDEL Guidelines

SOFTware MANagement and DEveLopment Standards in Aptoma.

- [Development](#development)
	- [Versioning](#versioning)
		- [The basics of semantic versioning](#the-basics-of-semantic-versioning)
	- [Release notes](#release-notes)
	- [Coding Style](#coding-style)
	- [Source Control](#source-control)
		- [Naming repositories](#naming-repositories)
		- [Branches](#branches)
		- [Commits](#commits)
		- [Pull Requests](#pull-requests)
	- [Technology and tools](#technology-and-tools)
		- [Backend](#backend)
		- [Frontend](#frontend)
	- [Testing](#testing-2)
	- [Mess Detection](#mess-detection)
	- [Continuous Integration](#continuous-integration)
		- [Build tools](#build-tools)
- [Application Design and Life Cycle](#application-design-and-life-cycle)
	- [Project participants](#project-participants)
	- [Code review](#code-review)
	- [Design Considerations](#design-considerations)
		- [Multitenancy](#multitenancy)
		- [Modularity and Microservices](#modularity-and-microservices)
	- [Refactoring](#refactoring)
	    - [Opportunistic Refactoring](#opportunistic-refactoring)
	    - [Scheduled Refactoring](#scheduled-refactoring)
	- [Deployment Process](#deployment-process)

## Development

### Versioning
If versioning is used in software, it should follow the [Semantic Versioning Standard](http://semver.org).

#### The basics of semantic versioning
Consider a version format of X.Y.Z (Major.Minor.Patch). Bug fixes not affecting the API increment the patch version, backwards compatible API additions/changes increment the minor version, and backwards incompatible API changes increment the major version.

For more details about semantic version see [http://semver.org/](http://semver.org/)

### Release notes

Every versioned release should have release notes detailing the changes, additions, removals and fixes contained in the release. Release notes should use the following terms to describe changes:

| Keyword         | Description |
|-----------------|-------------|
| Added           | Descriptions of new features. Try to keep it brief. If any additional information is required, add it to the "Notes" section. |
| Changed         | Information about alterations done to existing features. Be explicit about the difference between old and new behavior. |
| Removed         | Information about features which have been removed. Make sure to refer to other parts of the release notes if a feature has been removed in favor of something else. |
| Improved        | Minor tweaks and improvements that neither change features nor fix specific bugs. Performance improvements, slight style tweaks and similar adjustments all fall under this category. |
| Fixed           | Known or reported issues which have been resolved. When detailing the fix, prefer "should not" to "will not", "issue" to "bug" and "resolved" to "fixed". |
| Deprecated      | Features which are marked for deletion. Any replacement features are expected to be suitably documented, along with any potential upgrade paths. Distance in time between deprecation and removal should be proportional to the scope of the feature. For projects following semantic versioning, deprecated features should be scheduled for removal in the next major version.
| Notes           | Additional information about something covered in one of the other points. A typical example might be instructions for a new feature or details regarding an altered workflow. |
| Developer Notes | Additional information which might be useful for the customer's developers and is related to a change in this version. Typical examples are changes made to public APIs, or alterations to how the system handles custom skins or scripts. |

#### Other guidelines
Both "Notes" and "Developer Notes" should be kept brief. If explanations are lengthy, it's an indication that this explanation should live in the docs instead. When in doubt, err on the side of updating your docs.

The target audience for "Additions", "Changes", "Removals", "Improvements", "Issues" and "Notes" is the **end user**. Adjust wording accordingly.

The target audience for "Developer Notes" is the **technical staff** of the customer.

### Coding Style

When there’s an industry standard coding style for a language, we should adopt that. When multiple established standards exists, we should pick the one that most matches our current style, unless we all agree that some other standard is preferred.

#### PHP
All PHP code should conform to the [PSR-2 standard](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md)

#### JavaScript
JavaScript code should follow the [Yandex styleguide](https://github.com/yandex/codestyle/blob/master/javascript.md), with the following changes:

- Variables should be declared on the top of methods. Each var statement should only initialize one variable, but may declare several unitialized variables. [*](http://benalman.com/news/2012/05/multiple-var-statements-javascript/)
- No hard maximum line length, less than 120 characters is recommended.
- Tabs are used for indentation.

A [JSCS](http://jscs.info/) preset config is available in [Aptoma Bootstrap](https://github.com/aptoma/aptoma-bootstrap/blob/master/.jscsrc).

Apps using AngularJS should follow [John Papa's styleguide](https://github.com/johnpapa/angular-styleguide).

#### SCSS
SCSS code should use the default rules of [SCSS Lint](https://github.com/causes/scss-lint) with the exception of four spaces for indenting. A customized config file is located [here](https://github.com/aptoma/aptoma-bootstrap/blob/master/.scss-lint.yml). `scss-lint` can also be used for linting pure CSS files, and any recommendations for SCSS is also valid for CSS.

#### Other Languages
For other languages you must conform to some defined coding style, preferably
with one that has tools. Here are a few recommendations:

* Twig: http://twig.sensiolabs.org/doc/coding_standards.html
* Ruby: https://github.com/bbatsov/ruby-style-guide
* Scala: http://docs.scala-lang.org/style/

### Source Control
All projects should use GIT for version control.

All repositories should have a remote on GitHub.com.

All repositories should have a README.md that explains the purpose of the repo and helps new developers get started.

If your repository is open for contributions from non-team members, or there's important factors not covered by SOFT MANDEL, you should also include a CONTRIBUTING.md to explain guidelines for contributing.

#### Naming repositories
Repositories should have short names that make it easy to locate a repository and to understand what it does.

For repositories that are closely tied to a website, ie. `blog.aptoma.com`, use the domain name as the repository name, for all other repositories, separate words with dashes: `silex-bootstrap` and `dredition-frontend`.

#### Branches

The `master` branch should represent a stable version of the code. Non-production ready code should live in separate branches.

It's recommended to have a `develop` branch as the base for ongoing development.

GitHub should be configured to use the main development branch as the default branch.

#### Commits

Every commit should represent an atomic discrete change. Split unrelated changes into multiple commits, and try to make every commit able to stand on it's own. Every single commit should represent a working state of the application, so that regressions can easily be located with tools like [git bisect](https://git-scm.com/docs/git-bisect).

Commit messages should include an informative title, be written in the imperative style, and preferably be no longer than 50 characters. If a message body is required to explain more about the commit, add a blank line between the title and the body. See [A Note About Git Commit Messages](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) for more about the reasoning behind this.

If relevant, use [GitHub flavoured markdown](https://help.github.com/articles/basic-writing-and-formatting-syntax/) to link to commits, issues and repos. For example, include "Fix #13" in the message body to have issue #13 of the repository be automatically closed when the commit is merged into the main development branch.

#### Pull Requests

GitHub pull requests is the recommended tool for code review.

Similar to commit messages, each pull request should cover an atomic change, but it's OK for a pull request to cover a set of related commits. As a pull request will either be approved or rejected as a whole, changes that are not linked to each other is best split into multiple PRs. What belongs together in a pull request is not always clear. Use your best judgment, and be pragmatic.

While a PR is being reviewed, feel free to add a lot of commits or just amend and force push. Before merging, the branch should be rebased into a set of atomic discrete commits, as described above.

### Technology and tools
Our goal for technology and tools is to strike the right balance between smart defaults and flexibility.

Where there exists a clear best practice or standard, we should use that, where there are several more or less equivalent solutions, we should use the same solution every time, where there are new, emerging, or no established best practice, we should have room to explore, experiment, and learn.

Experimenting is encouraged, but should be scoped to low-risk increments. As experience increases, the situation should be reassessed, and the pros and cons should be discussed with other developers in Aptoma.

#### Backend
We have two standard languages for backend services: PHP and Node.js. Proficiency in these can be assumed from all developers in Aptoma.

When using other languages than these, the benefits should clearly outweigh the extra costs related to maintenance and resource flexibility. As you cannot assume general knowledge in Aptoma, you must take extra steps to ease onboarding of potential new team members. As soon as it moves beyond a simple experiment, consult with Head of Technology to ensure a sustainable approach.

##### PHP
Our baseline PHP version is 5.3 with Apache 2.2. New projects can consult with AMP in order to run 5.5 with Apache 2.4.

Standard PHP tools and frameworks:

- Composer for dependency management
- PHPUnit for unit test
- Silex as base micro framework
- Twig for templates
- Monolog for logging

New projects are encouraged to be based off of [Silex Bootstrap](https://github.com/aptoma/silex-bootstrap).

All tests should use PHPUnit, and projects should be set up so that you can run phpunit from the root directory to run all tests.

You should use namespaces and Composer for autoloading.

##### Node.js

Node.js projects are free to decide the version of Node.js to use, including io.js. We have no hard requirements regarding Node.js.

[Hapi.js](http://hapijs.com/) is a recommended framework to use, but you are free to experiment with other options.

Prefer using `npm` ask a task runner, and always define the `start` and `test` tasks.

##### Persistent storage (ie. DB)
We have no standard rules for databases. MySQL is the default choice for SQL, while MongoDB should be the default choice for NoSQL. Some experimentation and exploration is encouraged, but needs to be thoroughly weighed against stability, maintenance, and hosting costs, and obviously coordinated with AMP.

#### Frontend
We have no requirements for frontend JavaScript frameworks. As frontend technologies are in constant development, we encourage responsible experimentation.

Recommended options include Angular, Backbone, jQuery and React. Plain old JavaScript is encouraged for projects that really don’t need the richer functionality provided by frameworks, but the threshold for duplicating functionality from established, well known frameworks should be high.

##### Module Loading
Module loading in JavaScript is a moving target. With modules becoming a language feature in ES2015, the long term recommended solution is to use native ES2015 modules. [Look here](http://www.2ality.com/2014/09/es6-modules-final.html) for a thorough explanation. As ES2015 modules are not currently supported by any browsers, a transpiler will be required to use this syntax.

For projects that can't/won't rely on a transpiler, the recommended module syntax is [CommonJS](http://www.commonjs.org/). This is the same syntax used by Node.js, and is more similar to ES2015 than AMD.

##### Transpiling
Transpilers should only be used when they bring clear benefits. An example of this is using [TypeScript](http://www.typescriptlang.org/) or [Babel](https://babeljs.io/) to allow using EcmaScript 6 features. When using transpilers, be mindful of polyfills in the generated code, and ensure that performance is good across all supported browsers.

Using CSS preprocessors is OK, and even encouraged. Maintainability and quality of processed code is still more important, though. Don’t use the most esoteric features, unless it clearly adds value (“because you can” != value).

The recommended CSS preprocessor is [SCSS](http://www.sass-lang.com/guide).

##### Browser support
Any customer projects accessible by end users need to support whatever browsers the customer wants to support. For customer admin tools (ie. only used by their internal staff), we should push for only guaranteeing support for latest versions of Chrome and Firefox. For internal projects, we can assume latest versions of Chrome and Firefox.

When adding support for additional browsers infers very little overhead, we should support as broadly as possible. The two latest versions of all major browsers is the industry standard. Legacy versions of IE must be supported according to requirements from our customers.

##### Testing
[Karma](http://karma-runner.github.com/)/[Mocha](http://visionmedia.github.com/mocha/)/[Chai](http://chaijs.com/)/[Sinon](http://sinonjs.org/) seems to be the best combo ATM but since its an evolving technology it may change in the very near future.

### Testing
All projects should have unit tests. Unit tests should cover as much of the code base as is practically possible. The recommended minimum level of code coverage is 70%.

It is recommended that all projects have functional tests covering the main usage of the service.

We also encourage higher level testing, outlined in [Software Testing Practices](https://docs.google.com/a/aptoma.com/document/d/1Sy6lmekbB7ZH-upI99HhJzPJNDXtBrz5cnSf3f0hTKs/edit#).

A few guidelines for unit tests:

- For PHP, PHPUnit is the recommended assertion library and test runner.
- The test suite should be easy to run from the command line and from a CI server.
- Tests should generally only test public methods.
- Tests should be able to run without a network connection, any external dependencies should be mocked.

### Mess Detection
Mess detection is required since they forces us to recognize questionable elements of our code. The limit values used should be optimized for each project, and should change as time goes on and we learn more about how they affect our code.

For PHP application we use [PHPMD](http://phpmd.org).
For JavaScript we use [JSHint](http://jshint.com).
We recommend using [PHPCPD](https://github.com/sebastianbergmann/phpcpd) for Copy/Paste detection also.

Default configuration files for both PHPMD and JSHint is available at [https://github.com/aptoma/aptoma-bootstrap](https://github.com/aptoma/aptoma-bootstrap)

### Continuous Integration
All projects shall be integrated with a continuous integration server. Unless you have non-negotiable needs that [Travis CI](https://magnum.travis-ci.com/) can't meet, use Travis CI.

__Requirements__

- The CI project shall run automatically, usually by time interval / commit.
- Failed builds should be reported to project owner or other responsible persons.
- Resolving broken builds should be a high priority for all developers

#### Build tools
Some sort of build tool / script for a project is required for an easy integration with CI. We recommend using either [Ant](http://ant.apache.org/) or [Grunt](http://gruntjs.com/) to perform these tasks. For an example with Grunt look at the [silex bootstrap](https://github.com/aptoma/silex-bootstrap).


## Application Design and Life Cycle

### Project Participants

Designing and working alone on significant software project is not good. If this is the case go to management and kick them in the nuts.

If circumstances finds you working alone, designate a "co-pilot" who has basic familiarity with the project and can provide input and code reviews.

### Code review

Commits should always be read and reflected on by another developer within reasonable time. Use pull requests on GitHub.

### Design Considerations

#### Multitenancy

Always consider and prefer a multitenant design. If a full multitenant approach is not possible, consider [hybrid-tenancy](http://samnewman.io/patterns/deployment/hybrid-tenancy/).

#### Modularity and Microservices

You should structure your project into modules with clearly defined responsibilites. Consider breaking certain modules out as separate loosely coupled microservices, possibly reusable across products.

### Refactoring

#### Opportunistic Refactoring

Refactoring should be part of the daily workflow. A good practice to follow is the boy-scout rule:

> always leave the code behind in a better state than you found it

More details and advice is found at [Martin Fowler's Bliki](http://martinfowler.com/bliki/OpportunisticRefactoring.html)

#### Scheduled Refactoring

When a larger refactoring project is needed, participants from other projects
should be involved both at the start and at the end of the project, in order to
get different perspectives and shared learning opportunities.

### Deployment Process

All projects shall have the deployment process fully documented with requirements and dependencies.

All builds that are to be deployed should be tested before it is set into production. If the product has tests then it should be verified through the CI, this is preferably done automatically with some sort of deployment tool. For rare cases with products without tests there shall be a documented process on how to verify and or test the build upon deployment.

It is preferred that the deployment process is automated and contains a rollback option, and has documentation on how to verify the build and deployment as successful.

[Capistrano](http://capistranorb.com/) is one tool that is used by some projects to assist in automating the deploy process.
