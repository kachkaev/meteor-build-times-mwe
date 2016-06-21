```bash
# make sure meteor's npm is the latest:
meteor npm --version
# if it is 2.* instead of 3.9.*, run this:
meteor npm install -g npm@3.9.6

# launch app
rm -rf node_modules
meteor npm install
METEOR_PROFILE=100 meteor

# now change client/main.css and see the time profile
```

Result for `dev-no` branch when a less file changes:

```txt
| (#5) Profiling: ProjectContext prepareProjectForBuild
|
| ProjectContext prepareProjectForBuild                           157 ms (1)
|
| Top leaves:
|
| (#5) Total: 157 ms (ProjectContext prepareProjectForBuild)
|
| (#6) Profiling: Rebuild App
|
| files.stat                                                        0 ms (3)
| files.readFile                                                    2 ms (1)
| Rebuild App...................................................1,098 ms (1)
| ├─ compiler.compile(the app)....................................676 ms (1)
| │  └─ compileUnibuild (the app).................................675 ms (2)
| │     ├─ files.readdir                                          123 ms (462)
| │     ├─ files.stat                                             154 ms (11358)
| │     └─ other compileUnibuild (the app)                        299 ms
| └─ bundler.bundle..makeClientTarget.............................356 ms (1)
|    └─ Target#make...............................................356 ms (1)
|       ├─ Target#_runCompilerPlugins                             116 ms (1)
|       └─ Target#_emitResources..................................213 ms (1)
|          └─ PackageSourceBatch.computeJsOutputFilesMap          198 ms (1)
|
| Top leaves:
| other compileUnibuild (the app)............................299 ms (2)
| files.stat.................................................197 ms (11848)
| files.readdir..............................................123 ms (462)
| files.realpath.............................................104 ms (280)
|
| (#6) Total: 1,101 ms (Rebuild App)
|
```

Result for `dev-yes` branch a less file changes:

```txt
| (#5) Profiling: ProjectContext prepareProjectForBuild
|
| ProjectContext prepareProjectForBuild...........................249 ms (1)
| ├─ _resolveConstraints..........................................124 ms (1)
| │  └─ Select Package Versions                                   120 ms (1)
| └─ _downloadMissingPackages                                     117 ms (1)
|
| Top leaves:
|
| (#5) Total: 249 ms (ProjectContext prepareProjectForBuild)
|
| (#6) Profiling: Rebuild App
|
| files.stat                                                        0 ms (3)
| files.readFile                                                    2 ms (1)
| Rebuild App...................................................3,823 ms (1)
| ├─ compiler.compile(the app)..................................3,457 ms (1)
| │  └─ compileUnibuild (the app)...............................3,457 ms (2)
| │     ├─ files.realpath                                         654 ms (2152)
| │     ├─ files.readdir                                          819 ms (4308)
| │     ├─ files.stat                                             498 ms (36424)
| │     ├─ files.readFile                                         248 ms (672)
| │     ├─ files.lstat                                            118 ms (602)
| │     └─ other compileUnibuild (the app)                      1,118 ms
| └─ bundler.bundle..makeClientTarget.............................320 ms (1)
|    └─ Target#make...............................................320 ms (1)
|       ├─ Target#_runCompilerPlugins                             121 ms (1)
|       └─ Target#_emitResources..................................174 ms (1)
|          └─ PackageSourceBatch.computeJsOutputFilesMap          165 ms (1)
|
| Top leaves:
| other compileUnibuild (the app)..........................1,118 ms (2)
| files.readdir..............................................819 ms (4308)
| files.realpath.............................................701 ms (2203)
| files.stat.................................................521 ms (36935)
| files.readFile.............................................312 ms (819)
| files.lstat................................................118 ms (602)
|
| (#6) Total: 3,825 ms (Rebuild App)
|
```

See commit history to know the difference between the branches.

OS: Ubuntu 16.06 (should not matter)
