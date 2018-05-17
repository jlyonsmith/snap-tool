# Snap: Node Project Build & Release Manager

A tool that makes building, testing, starting, deploying and releasing multi-package NodeJS projects a snap!

Snap is an _opinionated_ tool for managing a NodeJS project tree.  It is most useful if your development environment:

- Uses [npm](http://npmjs.org) as the [NodeJS](http://nodejs.org) package manager
- Git to manage your source code
- If macOS, uses [iTerm2](https://www.iterm2.com/) as a scriptable replacement for the macOS Terminal app
- Uses my [stampver](https://www.npmjs.com/package/stampver) tool to manage your [semantic versioning](https://semver.org/)

You don't _have_ to use any or all of the above, but if you _do_ then `snap-tool` is the package for you!

## Installation

Install the package globally or use `npx` to run the latest version:

```
npm install -g snap-tool
snap help
```
or:

```
npx snap-tool help
```

## Build

To build all projects recursively use:

```
snap build
```

You can also specify `--install` and `--clean` with this command.

## Install

To install all `npm` packages recursively use:

```
snap install
```

If you specify `--clean` it will perform the `clean` command before installation.


## Clean

To recursively clean out all `node_modules`, `dist`, `build` and `package-lock.json` files use:

```
snap clean
```

## Update

To recursively `npm update` all packages use:

```
snap update
```

Remember that `npm update` will only update according to the version rules in the `package.json`.  To update to a different version, you can either do `npm install@M.m.p` where `M.m.p` is a semantic version number, or `npm update -f <package>`.

## Test

To run all tests recursively use:

```
snap test
```

Each command will search recursively through your project tree looking for `package.json` files to process.  They each ignore `node_modules` directories.

## Start

It's common to have a NodeJS based product comprised of a website, a server and perhaps mobile apps.  You can run everything at once just typing:

```
snap start
```

Snap is also intended to be used with NodeJS servers consisting of multiple _actor_ services (or node sub-processes.)  If this is your system and you want to start the individual actors instead of the main (typically monitoring or watchdog) process use:

```
snap start --actors
```

Either way, `snap` will start each process using a new iTerm2 console so that you can shut down your entire product by simply closing down the iTerm2 window.

## Release

If you are release products to `npm` then `snap` is there to help too.  Simply run the `release` command and everything is done for you, including adding a tag so your GitHub repository shows a new release.

The tool uses `stampver` to update version information for the build.  Just tell it which part of the version to update with `--patch`, `--minor` or `--major`.

If you add the `--npm` it will push the build to [npm](http://npmjs.org), but you must have already set up your credentials for doing so with `npm publish`.

## Deploy

Deployment refers to uploading code to a backend system, such as for a website or API service.  `snap` doesn't manage the actual deployment process, as there are too many different ways that can be done.  But assuming that you have a `deploy` script in each of your packages, you can run them recursively with:

```
snap deploy
```

You can set `SNAP_DEPLOY_USER` and `SNAP_DEPLOY_HOST` in your `.bashrc` file to set default values for the deploymnet user and host.  These values are typically used in for your `ssh` user &amp; host.

Here's an example `deploy` script in a `package.json`:

```
  ...
  "deploy": "rsync -vr -e ssh --exclude-from .rsync-exclude * $SNAP_DEPLOY_USER@$SNAP_DEPLOY_HOST:my-server/ && ssh $SNAP_DEPLOY_USER@$SNAP_DEPLOY_HOST 'cd my-app && npm install && sudo systemctl restart my-app'",
  ...
```

This is just an example.  You may have more sophisticated needs for your deployments.
