This repo tests whether changing `client/main.less` while Meteor is running takes longer when `node_modules` directory contains more files.

https://github.com/meteor/meteor/issues/7253  
https://github.com/meteor/meteor/issues/7008  

Make sure meteor's npm is the latest before running the tests:

```bash
meteor npm --version
# if it is 2.* instead of 3.9.*, run this:
meteor npm install -g npm@3.9.6
```

## Test A (no devDependencies)

```bash
cd /path/to/project
rm -rf node_modules
meteor npm install --only=dev
METEOR_PROFILE=100 meteor
```

### Result for OSX

```txt
(#5) Profiling: ProjectContext prepareProjectForBuild
|
| ProjectContext prepareProjectForBuild                           151 ms (1)
|
| Top leaves:
|
| (#5) Total: 151 ms (ProjectContext prepareProjectForBuild)
|
| (#6) Profiling: Rebuild App
|
| files.stat                                                        0 ms (2)
| Rebuild App.....................................................674 ms (1)
| └─ files.withCache..............................................674 ms (1)
|    ├─ compiler.compile(the app).................................275 ms (1)
|    │  └─ files.withCache........................................275 ms (2)
|    │     └─ compileUnibuild (the app)...........................275 ms (2)
|    │        └─ files.readFile                                   169 ms (618)
|    └─ bundler.bundle..makeClientTarget..........................334 ms (1)
|       └─ Target#make............................................334 ms (1)
|          ├─ Target#_runCompilerPlugins                          126 ms (1)
|          └─ Target#_emitResources...............................171 ms (1)
|             └─ PackageSourceBatch.computeJsOutputFilesMap       159 ms (1)
|
| Top leaves:
| files.readFile.............................................211 ms (745)
|
| (#6) Total: 674 ms (Rebuild App)
|
```

## Test B (all dependencies)

```bash
cd /path/to/project
rm -rf node_modules
meteor npm install
METEOR_PROFILE=100 meteor
```

### Result for OSX

```txt
(#5) Profiling: ProjectContext prepareProjectForBuild
|
| ProjectContext prepareProjectForBuild                           169 ms (1)
|
| Top leaves:
|
| (#5) Total: 169 ms (ProjectContext prepareProjectForBuild)
|
| (#6) Profiling: Rebuild App
|
| files.stat                                                        0 ms (2)
| Rebuild App.....................................................738 ms (1)
| └─ files.withCache..............................................738 ms (1)
|    ├─ compiler.compile(the app).................................297 ms (1)
|    │  └─ files.withCache........................................297 ms (2)
|    │     └─ compileUnibuild (the app)...........................297 ms (2)
|    │        └─ files.readFile                                   179 ms (672)
|    └─ bundler.bundle..makeClientTarget..........................384 ms (1)
|       └─ Target#make............................................384 ms (1)
|          ├─ Target#_runCompilerPlugins                          160 ms (1)
|          └─ Target#_emitResources...............................190 ms (1)
|             └─ PackageSourceBatch.computeJsOutputFilesMap       179 ms (1)
|
| Top leaves:
| files.readFile.............................................230 ms (818)
|
| (#6) Total: 738 ms (Rebuild App)
|
```
