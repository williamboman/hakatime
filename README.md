# hakatime

[![CircleCI](https://circleci.com/gh/mujx/hakatime.svg?style=svg)](https://circleci.com/gh/mujx/hakatime)
[![Docker build](https://img.shields.io/docker/cloud/build/mujx/hakatime)](https://hub.docker.com/r/mujx/hakatime/builds)
[![License: Unlicense](https://img.shields.io/badge/license-Unlicense-blue.svg)](http://unlicense.org/)

Hakatime is a server implementation of [Wakatime](https://wakatime.com/). It
provides a single API endpoint (`/api/v1/users/current/heartbeats.bulk`) that
the Wakatime client can use to send heartbeats containing info about your coding
activity.

It comes together with a simple dashboard which provides a graphical
representation of the collected data.

## Building

### Server

Requirements:

- [GHC](https://www.haskell.org/ghc/) (tested with 8.8)
- [libpq](https://www.postgresql.org/docs/11/libpq.html) (for PostgreSQL bindings)
- [cabal-install](https://www.haskell.org/cabal/) (If building with cabal)

Using [nix](https://nixos.org/nix/) requires the least amount of manual
intervention (installing packages etc)

#### nix

```bash
nix-build release.nix
```

#### cabal

```bash
cabal build
```

### Dashboard

Requirements:

- Node.js
- npm / yarn

```bash
cd dashboard

npm install # yarn install

npm run prod # Optimized build for production usage.

npm run dev # Development server with hot reloading.
```

## Running

### Database

The server needs a database to store its data, so we will have to create a
PostgreSQL instance and initialize it with the schema found in
[docker/001-init.sql](docker/001-init.sql). You can use the provided solution
using docker-compose (`docker-compose up -d`) or do it manually, depending on
your system.

### Server

Start the server and point it to the database.

```bash
# We assume that the docker-compose setup is used.
# Change these values according to your actual setup.
export HAKA_DB_USER=test
export HAKA_DB_PASS=test
export HAKA_DB_NAME=test
export HAKA_DB_HOST=localhost
export HAKA_DB_PORT=5432

hakatime run
```

### Dashboard

1. Point your browser to [http://localhost:8080](http://localhost:8080)
2. Create a new user.
3. Create an API token and set up your Wakatime client with it.

_NOTE_: In order to start sending data from your editor, you'll have to change the
original URL on the Wakatime client with the one of your Hakatime instance.

## CLI options

```
hakatime :: v0.1.0

Usage: hakatime COMMAND
  Wakatime server implementation

Available options:
  -h,--help                Show this help text

Available commands:
  create-token             Create a new auth token
  create-user              Create a new user account
  run                      Start the Server
```

## Screens

### Overview

![Overview Page](img/overview.png "Overview Page")

### Projects

![Projects Page](img/projects.png "Projects Page")


## Contributing

Any kind of contribution is greatly appreciated. This could be:

- Bug fixes
- Suggesting/Implementing new features
- UI/UX improvements/suggestions
- Code refactoring