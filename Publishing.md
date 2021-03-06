How to publish node-hid
========================


1. Merge all changes and new features into master
2. Fill out `changelog.md`
3. Bump up npm version in `package.json`
4. Update the `README.md` to reference this current version
5. Commit then generate new tags based on package.json version number with `git tag 0.5.4 -a` and include the change log in the tag's annotation.
6. Push tags to Github with `git push --tags`
7. Switch to node v4 and npm 2
8. `rm -rf node_modules build && npm install`
9. Publish to npm after builds finish. Builds can take half an hour and occasionally fail for seemingly no reason. Restart any failures in the travis or appeveyor ui. While you wait, remove the content of the Github release message so the tag's text shows. When the entire matrix succeeds and all binaries exist run `npm publish`.


## Hand publish method using `node-pre-gyp-github`
* Install tools globally to make things easier:
```
npm install -g node-pre-gyp
npm install -g node-pre-gyp-github
npm install -g rimraf
```
* Checkout clean node-hid:
```
rm -rf node-hid
git clone https://github.com/node-hid/node-hid.git
cd node-hid
npm run prepublish
```

* Clean checkout before each build:
```
npm run clean
npm install
```

* Do simple sanity tests
```
node ./src/show-devices.js
node ./src/test-blink1.js
```

* Test out node-pre-gyp packaging and publish draft:
```
node-pre-gyp configure
node-pre-gyp build
node-pre-gyp package
node-pre-gyp-github publish
```

* Do the above for each version of node:
  * v6.9.1 LTS
  * v4.6.2 LTS
  * v0.12.17


### Windows notes:
* Using nvm-windows, install each version of node needed (32-bit & 64-bit):
```
nvm install v0.12.7 32
nvm install v0.12.7 64
nvm install v4.6.2 32
nvm install v4.6.2 64
nvm install v6.9.1 32
nvm install v6.9.1 64
```

* Then, before each build:
```
nvm use v0.12.7 32
node install -g node-pre-gyp
node install -g node-pre-gyp-github
node install -g rimraf
npm run clean
npm install

```



## Old publish method

(cribbed from node-serialport)

## Setup for Linux, Windows and OSX

1. Merge all changes and new features into master.
2. Fill out changelog.md.
3. Bump up npm version *AND* binary host version in `package.json`, commit and push.
4. Update the README.md to reference this current version and to previous major version docs.
5. Generate new tags based on package.json version number with `git tag 4.0.0 -a` and include the change log in the tag's annotation.
6. Push tags to Github with `git push --tags`
7. Switch to node v0.12 and npm 2
8. `rm -rf node_modules build && npm install`
9. Publish to npm after builds finish. Builds can take half an hour and occasionally fail for seemingly no reason. Restart any failures in the travis or appeveyor ui. While you wait, remove the content of the Github release message so the tag's text shows. When the entire matrix succeeds and all binaries exist run `npm publish`.

Differences for beta release
* Tag like: `git tag 4.0.1-beta1 -a` and include the change log in the tag's annotation.
* Publish with `npm publish --tag beta`


## Binary Hand-publish Notes
* On Mac/Linux, have a ~/.node_pre_gyprc with Amazon AWS S3 accessKeyId & secretAccessKey
* On Windows, put that file in /c/Users/{username} when using MinGW

## Hand-publish steps
1. Pick node version (e.g. "nvm use v4" or "nodist use v4")
2. Update node-hid from master: `git pull`
3. Build: `npm install`
4. Package using node-pre-gyp: `npm run gyppackage`
5. Publish to AWS S3 using node-pre-gyp: `npm run gyppublish`
6. Clean up build products: `npm run clean`
