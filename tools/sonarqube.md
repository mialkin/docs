# SonarQube

[SonarQube](https://www.sonarqube.org) is a quality management platform focusing on continuous analysis of source code quality.

SonarQube detects bugs in the code automatically and alerts developers to fix them before rolling it out for production. SonarQube also highlights the complex areas of code that are less covered by unit tests

## Table of contents

- [SonarQube](#sonarqube)
  - [Table of contents](#table-of-contents)
  - [Installing a local instance of SonarQube](#installing-a-local-instance-of-sonarqube)
  - [SonarScanner](#sonarscanner)
  - [SonarCloud](#sonarcloud)
  - [SonarLint](#sonarlint)
  - ["Clean as you code" approach](#clean-as-you-code-approach)
  - [Quality gate](#quality-gate)
    - [Recommended quality gate](#recommended-quality-gate)
    - [Getting notified when a quality gate fails](#getting-notified-when-a-quality-gate-fails)
  - [Pull request analysis](#pull-request-analysis)

## Installing a local instance of SonarQube

See [↑ Try out SonarQube](https://docs.sonarqube.org/latest/setup/get-started-2-minutes) article.

## SonarScanner

Once the SonarQube platform has been installed, to enable CI-based analysis you have to install and configure a piece of software called a *scanner*.

Typically, the scanner is configured to run as part of your continuous integration pipeline so that whenever your build process is triggered, the scanner is invoked and performs a scan on the code.

[↑ SonarScanner for .NET](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-msbuild)

## SonarCloud

[↑ SonarCloud](https://sonarcloud.io) is a cloud-based code analysis service designed to detect code quality issues in 25 different programming languages, continuously ensuring the maintainability, reliability and security of your code.

SonarCloud uses state-of-the-art techniques in *static code analysis* to find problems, and potential problems, in the code that you and your team write.

A **static analysis** is a type of analysis that does not rely on actually running the code: analysis of running code is called **dynamic analysis**. As a result, SonarCloud offers an additional layer of verification, different from automated testing and human code-review.

[↑ Sonar community: SonarCloud vs SonarQube](https://community.sonarsource.com/t/sonarcloud-vs-sonarqube/9557)

## SonarLint

[↑ SonarLint](https://www.sonarlint.org) is a free IDE extension to find and fix bugs, vulnerabilities and code smells.

## "Clean as you code" approach

While SonarCloud does provide an overall picture of the quality of your entire codebase, it focuses on highlighting issues found on incoming changes as they arrive. This strategy is tied to a principle of coding referred to as "clean as you code". The idea behind this principle is that the efforts of a software development team in remediating code issues should always be focussed on preventing issues in incoming new code and fixing issues in the areas where those new changes occur, as opposed to digging through old code for the sole purpose of finding issues. In most software projects the natural progress of change will eventually touch the entire codebase, so remediation will even encompass the entire body of the code in any case.

## Quality gate

A **quality gate** is a set of conditions that tells you whether or not your project is ready for release. With the Clean as You Code approach, your Quality Gate should:

- **Focus on new code metrics**: new features will be delivered cleanly. As long as your quality gate is green, your releases will continue to improve.
- **Set and enforce high standards**: you aren't worried about having to meet those standards in old code and having to clean up someone else's code. You can take pride in meeting high standards of *your* code. If a project doesn't meet these high standards, it won't pass the quality gate, and it's obviously not ready to be released.

### Recommended quality gate

We recommend the built-in `Sonar way` quality gate for most projects. It focuses on keeping new code clean, rather than spending a lot of effort remediating old code. Out of the box, it's already set as the default profile.

[↑ Quality Gates](https://docs.sonarqube.org/latest/user-guide/quality-gates)

### Getting notified when a quality gate fails

Thanks to the notification mechanism, users can be notified when a quality gate fails. To do so, subscribe to the New quality gate status notification either for all projects or a set of projects you're interested in.

## Pull request analysis

You can use pull request analysis and decoration to make sure your code is meeting your standards before you merge. Pull request analysis lets you see your pull request's quality gate in the SonarQube UI.

[↑ Pull Request Analysis](https://docs.sonarqube.org/latest/analysis/pull-request)
