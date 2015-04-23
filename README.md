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
	- [Technology and tools](#technology-and-tools)
		- [Backend](#backend)
			- [Testing](#testing)
			- [Persistent storage (ie. DB)](#persistent-storage-ie-db)
		- [Frontend](#frontend)
			- [Transpiling](#transpiling)
			- [Browser support](#browser-support)
			- [Testing](#testing-1)
	- [Testing](#testing-2)
	- [Mess Detection](#mess-detection)
	- [Continuous Integration](#continuous-integration)
		- [Build tools](#build-tools)
	- [Technical Design](#technical-design)
		- [Multitenancy](#multitenancy)
		- [Project participants](#project-participants)
		- [Code review](#code-review)
	- [Refactoring](#refactoring)
	    - [Opportunistic Refactoring](#opportunistic-refactoring)
	    - [Scheduled Refactoring](#scheduled-refactoring)
- [Deployment](#deployment)
	- [Installation Types](#installation-types)
	- [Process](#process)

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
| Deprecated      | Features which are marked for deletion. Any replacement features are expected to be suitably documented, along with any potential upgrade paths. Distance in time between deprecation and removal should be proportional to the scope of the feature.
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
All projects should use GIT for version control. Any deviation from this should be thoroughly documented and warranted.

All repositories should have a remote on Github.

All repositories should have a README.md to explain the purpose of the repo. If your repository is open for contributions from non-team members, or there's important factors not covered by SOFT MANDEL, you should also include a CONTRIBUTING.md to explain guidelines for contributing.

#### Naming repositories
Repositories should have short names that make it easy to locate a repository and to understand what it does. Our legacy JAVA-inspired package naming scheme is deprecated. It is important that all repositories has a description that explains the purpose to make it easier for others to find things e.g for DrFront `Web application for front page production`.

For repositories that are closely tied to a website, ie. `blog.aptoma.com`, use the domain name as the repository name, for all other repositories, separate words with dashes, ie. `silex-bootstrap` and `node-github-downloader`.

Be wary of including your product name in a repo that is not actually coupled to your product. Ie., if you create a lib that upload files to CDN, don’t call it `drfront-cdn-uploader`, but rather `cdn-uploader`. Of course, if your library _is_ tied to a product, including the name is warranted, ie. `drlib-rest-client`.

The name of a library should reflects it's purpose e.g `cdn-uploader`.

#### Branches
Every repository will have a `master` branch.

If your workflow requires multiple branches, the recommended practice is to have the branches `master` and `develop`, where master always reflects the most recent deployed version, and develop is the main development branch.

Any branch that is not master or develop is a feature branch, and should have a descriptive name. All feature branches should be based off of develop, and merged back into develop when done.

### Technology and tools
Our goal for technology and tools is to strike the right balance between smart defaults and flexibility.

Where there exists a clear best practice or standard, we should use that, where there are several more or less equivalent solutions, we should use the same solution every time, where there are new, emerging, or no established best practice, we should have room to explore, experiment, and learn.

#### Backend
Backend development should use PHP as the main programming language. We can assume PHP5.3, for all projects.

Standard PHP tools and frameworks:

- Composer for dependency management
- PHPUnit for unit test
- Silex as base micro framework
- Twig for templates
- Monolog for logging

New projects are encouraged to be based off of [Silex Bootstrap](https://github.com/aptoma/silex-bootstrap).

##### Testing
All tests should use PHPUnit, and projects should be set up so that you can run phpunit from the root directory to run all tests.

##### Persistent storage (ie. DB)
We have no standard rules for databases. MySQL is the default choice for SQL, while MongoDB should be the default choice for NoSQL. Some experimentation and exploration is encouraged, but needs to be thoroughly weighed against stability, maintenance, and hosting costs, and obviously coordinated with AMP.

#### Frontend
We have no strict guidelines for frontend frameworks. As frontend technologies are in constant development, and a lot less settled than PHP, we should encourage a certain degree of experimentation.

The “safe” choice is jQuery, Underscore/Lodash, and Backbone, as required. Plain old JavaScript is encouraged for projects that really don’t need the richer functionality provided by frameworks, but the threshold for duplicating functionality from established, well known frameworks should be high.

We shall write AMD compatible modules when using JavaScript module management. RequireJS is the prefered AMD loader.

__We use HTML5.__

##### Transpiling
As a general rule, we do not use languages that transpile.

Using CSS preprocessors are OK, and even encouraged. Maintainability and quality of processed code is still more important, though. Don’t use the most esoteric features, unless it clearly adds value (“because you can” != value).

The recommended CSS preprocessor is [SCSS](http://www.sass-lang.com/guide).

##### Browser support
Any customer projects accessible by end users need to support whatever browsers the customer wants to support. For customer admin tools (ie. only used by their internal staff), we should push for only guaranteeing support for latest versions of Chrome and Firefox. For internal projects, we can assume latest versions of Chrome and Firefox.

When adding support for additional browsers infers very little overhead, we should support as broadly as possible. The two latest versions of all major browsers is the industry standard, but that will currently not include IE8, which still has a large market share. We should be prepared to support IE8 when required.

##### Testing
[Karma](http://karma-runner.github.com/)/[Mocha](http://visionmedia.github.com/mocha/)/[Chai](http://chaijs.com/)/[Sinon](http://sinonjs.org/) seems to be the best combo ATM but since its an evolving technology it may change in very near future.

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
All projects that has some sorts of tests (e.g PHPUnit, linting, mess detection) shall be integrated with an continuous integration server. We suggest using [Travis CI](https://magnum.travis-ci.com/), where we have a pro account.

The use of other CI solutions shall be coordinated with AMP.

__Requirements__

- The CI project shall run automatically, usually by time interval / commit.
- Failed builds should be reported to project owner or other responsible persons.
- Resolving broken builds should be a high priority for all developers

#### Build tools
Some sort of build tool / script for a project is required for an easy integration with CI. We recommend using either [Ant](http://ant.apache.org/) or [Grunt](http://gruntjs.com/) to perform these tasks. For an example with Grunt look at the [silex bootstrap](https://github.com/aptoma/silex-bootstrap).

### Technical Design
Recommendations to consider when starting a new software project.

#### Multitenancy
If the project has the potential of getting more than one customer and if it is supposed to run in SaaS then it would be wise to have a multitenancy design.

#### Project participants
Designing and working alone on significant software project is not good. If this is the case go to management and kick them in the nuts.

#### Code review
Commits should always be read and reflected on by another developer within reasonable time.

### Refactoring

#### Opportunistic Refactoring

Refactoring should be part of the daily workflow. A good practice to follow is the boy-scout rule:

> always leave the code behind in a better state than you found it

More details and advice is found at [Martin Fowler's Bliki](http://martinfowler.com/bliki/OpportunisticRefactoring.html)

#### Scheduled Refactoring

When a larger refactoring project is needed, participants from other projects
should be involved both at the start and at the end of the project, in order to
get different perspectives and shared learning opportunities.

## Deployment

### Installation Types
Each product is encouraged to have at least four installation types:

1. Production (possibly one for each customer)
2. Staging (possibly one for each customer).
3. Sandbox (for internal/external bleeding edge testing and development, also cross product).
4. Demo (for showing off the product to new customers etc).

In addition, most systems are also likely to have local and/or shared development environments.

**Production** is quite simply the customer’s active production environment.

**Staging** should mimic production, and be used for previewing and validating new features before deploying to production. Staging may be an ad-hoc setup, that is only activated during development phases with customers, and just before deploying new code. In a proper multi-tenant setup, staging should always be active. Ideally, a full integration and system test suite (see below) should be run on the staging environment before deploy to production.

The need for staging environments will vary from product to product, and possibly from customer to customer too.

The only hard requirement is that your application is able to provide different environments, in effect meaning that no special environment config is hard coded throughout the application.

**Sandbox** is the place where can play around. The sandbox allows other internal teams to experiment with integrations in a production like environment without affecting users (ie. create test articles for DrPublish to show in DrFront/DrMobile/etc).

The sandbox environment may also be used in close collaboration with bleeding edge customers that we do co-development with.

In a multi-tenant design, the sandbox install might be “just another customer”, possibly present in both production and staging.

**Demo** should run the most recent demoable version, and only contain content that is suitable for
display in product demos for C-level executives. In a multi-tenant design, the demo install will most likely be “just another customer”.

### Process
All projects shall have the deployment process fully documented with requirements and dependencies.

All builds that are to be deployed should be tested before it is set into production. If the product has tests then it should be verified through the CI, this is preferably done automatically with some sort of deployment tool. For rare cases with products without tests there shall be a documented process on how to verify and or test the build upon deployment.

It is prefered that the deployment process is automated and contains rollback option and has documentation on how to verify the build as successful.

[Capistrano](http://capistranorb.com/) is one tool that is used by some projects to assist in automating the deploy process.


