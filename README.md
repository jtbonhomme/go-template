# Go Clean template
Clean Architecture template for Golang services


[![Go Report Card](https://goreportcard.com/badge/github.com/jtbonhomme/go-template)](https://goreportcard.com/report/github.com/jtbonhomme/go-template)
[![License](https://img.shields.io/github/license/jtbonhomme/go-template.svg)](https://github.com/jtbonhomme/go-template/blob/master/LICENSE)
[![Release](https://img.shields.io/github/v/release/jtbonhomme/go-template.svg)](https://github.com/jtbonhomme/go-template/releases/)
[![codecov](https://codecov.io/gh/jtbonhomme/go-template/branch/master/graph/badge.svg?token=XE3E0X3EVQ)](https://codecov.io/gh/jtbonhomme/go-template)

## Overview
The purpose of the template is to help you to bootstrap your go projects:
- well organized repository, to prevent it from turning into spaghetti code
- separated business logic, so that it remains independent, clean, and extensible
- basic plumbery for tests, builds, releases, lint, dockerization, ...

## Quick start
Local development:
```sh
# launches external components
$ make compose-up
# Run app
$ make run
```

## Project structure
### `cmd/app/main.go`
Configuration and logger initialization. Then the main function "continues" in `app.go`.

There is always one _Run_ function in the `app.go` file, which "continues" the _main_ function.

This is where all the main objects are created.
Dependency injection occurs through the "New ..." constructors (see Dependency Injection).
This technique allows us to layer the application using the [Dependency Injection](#dependency-injection) principle.
This makes the business logic independent from other layers.

Next, we start the server and wait for signals in _select_ for graceful completion.
If `app.go` starts to grow, you can split it into multiple files.

For a large number of injections, [wire](https://github.com/google/wire) can be used.

### `internal/config`
Configuration. First, `config.yml` is read, then environment variables overwrite the yaml config if they match.
The config structure is in the `config.go`.
The `env-required: true` tag obliges you to specify a value (either in yaml, or in environment variables).

For configuration, we chose the [cleanenv](https://github.com/ilyakaznacheev/cleanenv) library.
It does not have many stars on GitHub, but is simple and meets all the requirements.

Reading the config from yaml contradicts the ideology of 12 factors, but in practice, it is more convenient than
reading the entire config from ENV.
It is assumed that default values are in yaml, and security-sensitive variables are defined in ENV.

### `pkg/logger`
Package that exposes a logging interface to be used in all the project. It relies on [zerolog](https://github.com/rs/zerolog)

### `internal/server`
Dummy demonstration http server.

## Useful links
- [The Clean Architecture article](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-architecture.html)
- [Twelve factors](https://12factor.net/ru/)
