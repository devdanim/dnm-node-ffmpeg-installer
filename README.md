# dnm-node-ffmpeg-installer (forked from kribblo/node-ffmpeg-installer)

Platform independent binary installer of [FFmpeg](https://ffmpeg.org/) for node projects. Useful for tools that should "just work" on multiple environments.

Installs a binary of `ffmpeg` for the current platform and provides a path and version. Supports Windows 64bit and Mac OS/X.

A combination of package.json fields `optionalDependencies`, `cpu`, and `os` let's the installer only download the binary for the current platform. See also "Warnings during install", below.

## Install

    npm install --save dnm-node-ffmpeg-installer
    
## Usage examples

```javascript
const ffmpeg = require('dnm-node-ffmpeg-installer');
console.log(ffmpeg.path, ffmpeg.version);
```

### [process.spawn()](https://nodejs.org/api/child_process.html#child_process_child_process_spawn_command_args_options)

```javascript
const ffmpegPath = require('dnm-node-ffmpeg-installer').path;
const spawn = require('child_process').spawn;
const ffmpeg = spawn(ffmpegPath, args);
ffmpeg.on('exit', onExit);
```

### [fluent-ffmpeg](https://github.com/fluent-ffmpeg/node-fluent-ffmpeg)

```javascript
const ffmpegPath = require('dnm-node-ffmpeg-installer').path;
const ffmpeg = require('fluent-ffmpeg');
ffmpeg.setFfmpegPath(ffmpegPath);
```

## Known issues

### Warnings during install

To automatically choose the binary to install, [optionalDependencies](https://docs.npmjs.com/files/package.json#optionaldependencies) are used. This currently outputs warnings in the console, an issue that is [tracked by the npm team here](https://github.com/npm/npm/issues/9567).

### AWS and/or Elastic Beanstalk

If you get permissions issues, try adding a .npmrc file with the following:

    unsafe-perm=true
    
See [issue #21](https://github.com/kribblo/node-ffmpeg-installer/issues/21)

### Wrong path under Electron with Asar enabled

It's a [known issue](https://github.com/electron-userland/electron-packager/issues/740) that Asar breaks native paths. As a workaround, if you use Asar, you can do something like this:

```javascript
const ffmpegPath = require('dnm-node-ffmpeg-installer').path.replace('app.asar', 'app.asar.unpacked');
```

## The binaries

Downloaded from the sources listed at [ffmpeg.org](https://ffmpeg.org/download.html):

* Mac OS/X (98989-g2c0d6ac9ae): https://evermeet.cx/ffmpeg/
* Windows 64-bit (20200831-4a11a6f): https://ffmpeg.zeranoe.com/builds/win64/static/

For version updates, submit issue or pull request.

## Upload new versions

In every updated `platforms/*` directory:
 
    npm run upload
    
## See also

* [node-ffprobe-installer](https://www.npmjs.com/package/@ffprobe-installer/ffprobe) - fork of this project that does the same thing for FFprobe
