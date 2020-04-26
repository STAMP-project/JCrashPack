[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3766689.svg)](https://doi.org/10.5281/zenodo.3766689)

# JCrashPack: A Java Crash Reproduction Benchmark

This repository contains the following elements:
- `jcrashpack.json` describes the crashes;
- `applications/` contains, for each application and concerned version, the binaries of the version on which one or more crashes happened;
- `crashes/`contains for each application the stack traces for each crash.

## Reference

The methodology followed to build JCrashPack has been described in Soltani, M., Derakhshanfar, P., Devroey, X. and van Deursen, A. (2020). [A benchmark-based evaluation of search-based crash reproduction](https://doi.org/10.1007/s10664-019-09762-1). In *Empirical Software Engineering*. 25, 1 (Jan. 2020), pp. 96–138. ([open access](https://doi.org/10.1007/s10664-019-09762-1)). When using JCrashPack, please use the following reference:

```bibtex
@article{Derakhshanfar2020,
  author    = {Soltani, Mozhan and Derakhshanfar, Pouria and Devroey, Xavier and van Deursen, Arie},
  title     = {A benchmark-based evaluation of search-based crash reproduction},
  journal   = {Empirical Software Engineering},
  volume    = {25},
  number    = {1},
  pages     = {96--138},
  year      = {2020},
  doi       = {10.1007/s10664-019-09762-1}
}
```

The dataset is also released on Zenodo for long-term storage.

## Add a new crash

1. Adding the stack trace
  * Add a `<project>` directory to `crashes/`. For instance, `crashes/XWiki`.

  * Add a `<issue-name>` directory to `crashes/<project>`. For instance, `crashes/XWiki/XCOMMONS-928`. The `<issue-name>` should denote the issue in the `<project>` from which the stack trace has been collected.

  * Add a `<issue-name>.log` file to `crashes/<project>/<issue-name>`. For instance, `crashes/XWiki/XCOMMONS-928/XCOMMONS-928.log`. File `<issue-name>.log` contains the stack trace without any other information. For instance:

```
java.lang.NullPointerException: null
    at org.xwiki.extension.repository.internal.installed.DefaultInstalledExtensionRepository.applyInstallExtension(DefaultInstalledExtensionRepository.java:449)
    at org.xwiki.extension.repository.internal.installed.DefaultInstalledExtensionRepository.installExtension(DefaultInstalledExtensionRepository.java:691)
    at org.xwiki.extension.job.internal.AbstractExtensionJob.installExtension(AbstractExtensionJob.java:257)
    at org.xwiki.extension.job.internal.AbstractExtensionJob.applyAction(AbstractExtensionJob.java:204)
    at org.xwiki.extension.job.internal.AbstractExtensionJob.applyActions(AbstractExtensionJob.java:151)
    at org.xwiki.extension.job.internal.InstallJob.runInternal(InstallJob.java:150)
    at org.xwiki.job.AbstractJob.runInContext(AbstractJob.java:205)
    at org.xwiki.job.AbstractJob.run(AbstractJob.java:188)
    at org.xwiki.platform.wiki.creationjob.internal.ExtensionInstaller.installExtension(ExtensionInstaller.java:73)
    at org.xwiki.platform.wiki.creationjob.internal.steps.ProvisionWikiStep.execute(ProvisionWikiStep.java:78)
    at org.xwiki.platform.wiki.creationjob.internal.WikiCreationJob.runInternal(WikiCreationJob.java:80)
    at org.xwiki.job.AbstractJob.runInContext(AbstractJob.java:205)
    at org.xwiki.job.AbstractJob.run(AbstractJob.java:188)
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source)
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source)
    at java.lang.Thread.run(Unknown Source)
```
  

2. Adding dependencies
  * Add a `<project>` directory to `applications/`. For instance, `applications/XWiki`.

  * Add a `<version>` directory to `applications/<project>`. For instance, `applications/XWiki/7.2`. `<version>` is the version of the `<project>` that will be used by to replicate the stack trace.

  * Put the Jar file of version `<version>` of the `<project>` and all its dependencies in `applications/<project>/<version>/bin` and add a zip file (or a splitted zip file) with the source code of the application in `applications/<project>/<version>/src.zip` for future analysis.

3. Adding a new application to `jcrashpack.json`:
  * Edit `jcrashpack.json`.
  * Under `applications`, add an entry with `<project>` as the key.
  * Add a name and URL for the application.
  * Add an entry under versions with `<version>` as the key, provide a URL for the source code (optional), and set version value to `<version>`.

3. Adding a new crash to `jcrashpack.json`:
  * Edit `jcrashpack.json`.
  * Under `crashes`, add an entry with `<issue-name>` as the key.
  * Set `application` value to `<project>`.
  * If the information is available, set the `buggy_frame` value to the frame in the stack trace where the bug is, the commit where is was fixed (`fixed_commit`), the URL to the `issue` in an issue tracker, and the version of the project for which the issue has been fixed (`version_fixed`).
  * Set the `id` value to `<issue-name>`.
  * Provide a regular expression for `target_frames` designing the target frames that have to be considered in the stack trace for crash replication. For instance, `.*xwiki.*` allows to consider only target frames in the stack trace where `xwiki` appears. This allows to discard frames from other projects and JDK API.
  * Set the `version` value to `<version>`.

