# SOFT MANDEL Guidelines

SOFTware MANagement and DEveLopment Standards in Aptoma.

- [Development](#development)
	- [Versioning](#versioning)
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
		- [System identification](#system-identification)
	- [Testing](#testing-2)
	- [Mess Detection](#mess-detection)
	- [Continuous Integration](#continuous-integration)
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

All services should use versioning, and follow the [Semantic Versioning Standard](http://semver.org).


### Release notes

Every versioned release should have release notes detailing the changes, additions, removals and fixes contained in the release. Release notes should use the following terms to describe changes:

| Keyword         | Description |
|-----------------|-------------|
| Added           | Descriptions of new features. Link to relevant doc(s) instead of being verbose in release notes. |
| Changed         | Information about alterations done to existing features. Be explicit about the difference between old and new behavior, and link to relevant doc(s). |
| Removed         | Information about features which have been removed. Link to relevant deprecation doc(s). Make sure to refer to other parts of the release notes if a feature has been removed in favor of something else. |
| Improved        | Minor tweaks and improvements that neither change features nor fix specific bugs. Performance improvements, slight style tweaks and similar adjustments all fall under this category. |
| Fixed           | Known or reported issues which have been resolved. When detailing the fix, prefer "should not" to "will not", "issue" to "bug" and "resolved" to "fixed". |
| Deprecated      | Features which are marked for deletion. Add link to any replacement features' doc(s), along with any potential upgrade paths. For projects following semantic versioning, deprecated features should be scheduled for removal in the next major version, and should adhere to [SAMBA Upgrade terms](https://docs.google.com/document/d/1H5e3c5LTOrB3s9MaoRGz0AGDTkmMTsdQuks7SxpWR_o/edit#heading=h.ay7frjyq9xs6).

#### Other guidelines

Both "Notes" and "Developer Notes" should be kept brief. If explanations are lengthy, it's an indication that this explanation should live in the docs instead. When in doubt, err on the side of updating your docs.

The target audience for "Additions", "Changes", "Removals", "Improvements", "Issues" and "Notes" is the **end user**. Adjust wording accordingly.

The target audience for "Developer Notes" is the **technical staff** of the customer.

### Coding Style

When there’s an industry standard coding style for a language, we should adopt that. When multiple established standards exists, we should pick the one that most matches our current style, unless we all agree that some other standard is preferred.

#### PHP

All PHP code should conform to the [PSR-2 standard](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md)

#### JavaScript

JavaScript code should implement Aptoma's ESLint config using [@aptoma/eslint-config](https://github.com/aptoma/eslint-config).

Using [Prettier](https://prettier.io/) for consistent formatting is encouraged.


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

#### Naming repositories

Repositories should have short names that make it easy to locate a repository and to understand what it does.

For repositories that are closely tied to a website, ie. `blog.aptoma.com`, use the domain name as the repository name, for all other repositories, separate words with dashes: `silex-bootstrap` and `dredition-frontend`.

#### Branches

The `master` branch should represent a stable version of the code. Non-production ready code should live in separate branches.

#### Commits

Every commit should represent an atomic discrete change. Split unrelated changes into multiple commits, and try to make every commit able to stand on its own. Every single commit should represent a working state of the application, so that regressions can easily be located with tools like [git bisect](https://git-scm.com/docs/git-bisect).

Commit messages should include an informative title, be written in the imperative style, and preferably be no longer than 50 characters. If a message body is required to explain more about the commit, add a blank line between the title and the body. See [A Note About Git Commit Messages](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) for more about the reasoning behind this.

If relevant, use [GitHub flavoured markdown](https://help.github.com/articles/basic-writing-and-formatting-syntax/) to link to commits, issues and repos. For example, include "Fix #13" in the message body to have issue #13 of the repository be automatically closed when the commit is merged into the main development branch.

#### Pull Requests

GitHub pull requests is the recommended tool for code review.

Similar to commit messages, each pull request should cover an atomic change, but it's OK for a pull request to cover a set of related commits. As a pull request will either be approved or rejected as a whole, changes that are not linked to each other is best split into multiple PRs. What belongs together in a pull request is not always clear. Use your best judgment, and be pragmatic.

While a PR is being reviewed, feel free to add a lot of commits or just amend and force push. Before merging, the branch should be rebased into a set of atomic discrete commits, as described above.

The person opening a pull request should also be the one to merge it, once reviewers have approved. Once merged, the changes should be deployed as soon as possible. If there are any special considerations to made during deployment (like database migration, dependencies on other services being updated as well, complicated rollback), these should be documented in the pull request.

### Technology and tools

Our goal for technology and tools is to strike the right balance between smart defaults and flexibility.

Where there exists a clear best practice or standard, we should use that, where there are several more or less equivalent solutions, we should use the same solution every time, where there are new, emerging, or no established best practice, we should have room to explore, experiment, and learn.

Experimenting is encouraged, but should be scoped to low-risk increments. As experience increases, the situation should be reassessed, and the pros and cons should be discussed with other developers in Aptoma.

#### Backend

The default choice for all backend services is Node.js.

When starting new projects, always use Node.js unless there are very compelling reasons not to. When choosing a different language, the benefits should clearly outweigh the extra costs related to maintenance and resource flexibility. As you cannot assume general knowledge in Aptoma, you must take extra steps to ease onboarding of potential new team members. As soon as it moves beyond a simple experiment, consult with Head of Technology to ensure a sustainable approach.

All Node.js projects should use a supported version. Target the latest LTS, and update your projects before the version you use is EOL.

[Hapi.js](http://hapijs.com/) is the recommended backend framework.

Use `npm` ask a task runner, and always define the `start` and `test` tasks.

##### Persistent storage (ie. DB)

We have no standard rules for databases. MySQL is the default choice for SQL, while MongoDB should be the default choice for NoSQL. Some experimentation and exploration is encouraged, but needs to be thoroughly weighed against stability, maintenance, and hosting costs, and obviously coordinated with AMP.

Prefer database engines supported by Amazon RDS. Consider using DynamoDB.

#### Frontend

We have no requirements for frontend JavaScript frameworks. As frontend technologies are in constant development, we encourage responsible experimentation. Talk to your colleagues! Be careful about using shiny new tech for projects that may have short a development time, but a long maintenance lifetime, as future you will dislike the shiny new tech that is now old, broken and unsupported.

##### Transpiling

Transpilers should only be used when they bring clear benefits. When a build step is already required by your project, you can use TypeScript.

Using CSS preprocessors and postprocessers is OK, and even encouraged. Maintainability and quality of processed code is still more important, though. Don’t use the most esoteric features, unless it clearly adds value (“because you can” != value).

The recommended CSS preprocessor is [SCSS](http://www.sass-lang.com/guide). Also consider using [PostCSS](https://github.com/postcss/postcss).

##### Browser support

Any customer projects accessible by end users need to support whatever browsers the customer wants to support. Customer admin tools (ie. only used by their internal staff) and our internal tools should support latest versions of Chrome and Firefox.

When adding support for additional browsers infers very little overhead, we should support as broadly as possible. The two latest versions of all major browsers is the industry standard. No version of Internet Explorer is formally supported.

#### System identification

All HTTP backends should identify themselves via a properly formatted `User-Agent` header. The first identifier in the header should be `aptoma`, followed by service name and optional version. Additionally, backends may add arbitrary data as key-value pairs in parentheses, of which the minimum requirement is to describe the deployment environment. This format is specified by [RFC9110](https://www.rfc-editor.org/rfc/rfc9110#field.user-agent).

Examples:

```
User-Agent: aptoma drpublish/1.2.3 (env:production)
User-Agent: aptoma squid/3.2.1 (env:development;source:580e0c33b5ec424880a39254)
```

As a general rule, avoid including information in `User-Agent` which already exists elsewhere in the request, or can be extracted from information in the request. Examples include the user account (which should be extracted from the authentication), or trace information (which, if included, should exist as standardized [trace headers](https://www.w3.org/TR/trace-context/)). Frontends should not overwrite the `User-Agent` set by the browser.

### Testing

All projects should have tests. Your test coverage should give all contributing developers high confidence of the correctness of the code.

### Mess Detection

Mess detection is required since they forces us to recognize questionable elements of our code. The limit values used should be optimized for each project, and should change as time goes on and we learn more about how they affect our code.

For PHP application we use [PHPMD](http://phpmd.org) with [https://github.com/aptoma/aptoma-bootstrap/blob/master/phpmd.xml](https://github.com/aptoma/aptoma-bootstrap/blob/master/phpmd.xml)

For JavaScript we use [ESLint](http://eslint.org) with [@aptoma/eslint-config](https://github.com/aptoma/eslint-config).

### Continuous Integration

All projects should be integrated with a continuous integration server. Unless you have non-negotiable needs that [Travis CI](https://magnum.travis-ci.com/) can't meet, use Travis CI. Never merge or deploy failing builds.

## Application Design and Life Cycle

### Project Participants

Designing and working alone on a significant software project brings risks. We aspire to always have more than one set of eyeballs on every problem. In reality, we will often have projects with a lead developer that has so much domain specific knowledge that it's unrealistic for other developers to contribute on an ongoing basis.

You should work to make it possible for someone else to take over in your abscence. Following the processes described in SOFT MANDEL is our main tool for that. If you find yourself doing long-lived solo projects, keep management informed, so that risks can be accounted for.

### Code review

Commits should always be read and reflected on by another developer within reasonable time. Use pull requests on GitHub.

### Design Considerations

#### Multitenancy

Always consider and prefer a multitenant design.

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

All builds that are to be deployed should be tested before it is set into production. If the product has automated tests, it should be verified through the CI. This is preferably done automatically with some sort of deployment tool. For rare cases with products without tests, there shall be a documented process on how to verify and or test the build upon deployment.

It is preferred that the deployment process is automated and contains a rollback option, and has documentation on how to verify the build and deployment as successful.

It is the responsibility of the deployer to ensure that the deployed changes work as expected after deployment. This may include manual testing, automated testing, and monitoring error logs. Deployments should not be made unless the deployer can be available for monitoring and possible rollback until they are condifident about the correctness of the deployed code.
