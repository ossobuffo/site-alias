# SiteAlias

Manage alias records for local and remote sites.

[![Travis CI](https://travis-ci.org/consolidation/site-alias.svg?branch=master)](https://travis-ci.org/consolidation/site-alias)
[![Windows CI](https://ci.appveyor.com/api/projects/status/{{PUT_APPVEYOR_STATUS_BADGE_ID_HERE}}?svg=true)](https://ci.appveyor.com/project/consolidation/site-alias)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/consolidation/site-alias/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/consolidation/site-alias/?branch=master)
[![Coverage Status](https://coveralls.io/repos/github/consolidation/site-alias/badge.svg?branch=master)](https://coveralls.io/github/consolidation/site-alias?branch=master) 
[![License](https://img.shields.io/badge/license-MIT-408677.svg)](LICENSE)

## Overview

This project provides the implementation for Drush site aliases. It is used in Drush 9 and later. It would also be possible to use this library to manage site aliases for similar commandline tools.

### Alias naming conventions

Site alias names always begin with a `@`, and typically are divided in two parts: the site name, and the environment name, each separated by a dot. Neither a site name not an enviornment name may contain a dot. An example alias that referenced the `dev` envionment of the site `example` might therefore look something like:
```
@example.dev
``` 
It is also possible to use single-word aliases. These can sometimes be ambiguous; the site alias manager will resolve single-word aliases as follows:

1. `@self` is interpreted to mean the site that has already been selected, or the site that would be selected in the absence of any alias.
2. `@none` is interpreted as the empty alias--an alias with no items defined.
3. `@<env>`, for any `<env>` is equivalent to `@self.<env>` if such an alias is defined. See below.
4. `@<site>`, for any `<site>` is equivalent to the default environment of `<site>`, e.g. `@<site>.<default>`. The default environment defaults to `dev`, but may be explicitly set in the alias.

### Alias placement on commandline

It is up to each individual commandline tools how to utilize aliases. There are two primary examples:

1. Site selection alias: `tool @sitealias command`
2. Alias parameters: `tool command @source @destination`

In the first example, with the site alias appearing before the command name, the alias is used to determine the target site for the current command. In the second example, the arguments of the command are used to specify source and destination sites.

### Alias filenames and locations

It is also up to each individual commandline tool where to search for alias files. Search locations may be added to the SiteAliasManager via an API call. Alias files are only found if they appear immediately inside one of the specified search locations; deep searching is never done.

Aliases are typically stored in Yaml files, although other formats may also be used if a custom alias data file loader is provided. The extension of the file determines the loader type (.yml for Yaml). The base name of the file, sans its extension, is the site name used to address the alias on the commandline. Site names may not contain periods.

### Alias file contents

The canonical site alias will contain information about how to locate the site on the local file system, and how the site is addressed on the network (when accessed via a web browser).
```
dev:
  root: /path/to/site
  uri: https://example.com
```
A more complex alias might also contain information about the server that the site is running on (when accessed via ssh for deployment and maintenance).
```
dev:
  root: /path/to/site
  uri: https://example.com
  remote: server.com
  user: www-data
```

### 'Self' environment aliases

As previously mentioned, an alias in the form of `@<env>` is interpreted as `@self.<env>`. This allows sites to define a `self.site.yml` file that contains common aliases shared among a team--for example, `@stage` and `@live`.

## Getting Started

To get started contributing to this project, simply clone it locally and then run `composer install`.

### Running the tests

The test suite may be run locally by way of some simple composer scripts:

| Test             | Command
| ---------------- | ---
| Run all tests    | `composer test`
| PHPUnit tests    | `composer unit`
| PHP linter       | `composer lint`
| Code style       | `composer cs`     
| Fix style errors | `composer cbf`

### Development Commandline Tool

This library comes with a commandline tool called `alias-tool`. The only purpose
this tool serves is to provide a way to do ad-hoc experimentation and testing
for this library.

See `./alias-tool help` and `./alias-tool list` for more information.

## Release Procedure

To create a release:

- Edit the `VERSION` file to contain the version to release, and commit the change.
- Run `composer release`

## Built With

This library was created with the [g1a/starter](https://github.com/g1a/starter) project, a fast way to create php libraries and [Robo](https://robo.li/) / [Symfony](https://symfony.com/) applications.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [releases](https://github.com/consolidation/site-alias/releases) page.

## Authors

* **Greg Anderson**
* **Moshe Weitzman**

See also the list of [contributors](https://github.com/consolidation/site-alias/contributors) who participated in this project. Thanks also to all of the [drush contributors](https://github.com/drush-ops/drush/contributors) who contributed directly or indirectly to site aliases.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details
