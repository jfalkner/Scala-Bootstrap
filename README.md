# Scala-Bootstrap

> WIP: Just put this up so that I can add notes as they come up. If interested, please wait for v1!

Working with Scala and still learning stuff? Bootstrap examples for a reasonable coding workflow and team dev process

- Build, test and run a scala project from scratch (in less than a minute!)
  - Local setup. Changing your computer's config.
  - VM via Vargrant, Virtualbox and Ansible. Won't change your computer's config.
- Basic edit, test, code coverage and pack cycle
  - Build via `sbt clean run`
  - Test and Coverage via `sbt coverage test coverageReport`
  - Deploy via `sbt clean coverageOff pack`
- Improvements
  - Continuous Integration (CI)
  - Peer-review via GitHub PRs
  - [CHANGELOG.md based on `git tag -ln`](https://github.com/jfalkner/Scala-Bootstrap/blob/master/README.md#changelogmd-based-on-git-tag--ln)

Stuff to add when time permits
- Base repo that has a build.sbt, expected structure, test, coverage and pack
- IntelliJ setup
- Dep on other SBT single projects (RootRef) and multi projects (ProjectRef)
- Bamboo CI config


## CHANGELOG.md based on `git tag -ln`

["Semantic Versioning"](http://semver.org/) of the code is helpful. Just use it. Amongst other benefits, it allows a 1-to-1 mapping of a logical identifier such a "0.0.1" to a git commit that represents a release of the code. Adopting this convention means you force developers to `git tag` specific meaningful commits and maintain `built.sbt`'s `version in ThisBuild := "0.0.1"` versioning. 

For example, if my codes semantic version is 1.2.3 then the codebase must have these things.

```
# build.sbt must have this string
version in ThisBuild := "1.2.3"

# after pushing the code related to 1.2.3 then the commit must be tagged and have the tag pushed
git tag -a 1.2.3 -m "My changelog message. The code was updated to do xzy..."
git push -u origin 1.2.3

# Other use of the code can now checkout this version
git clone repo_url
git checkout 1.2.3

# other Scala projects can import the tagged version as follows in build.sbt 
lazy val my_project = (project in file(".")).dependsOn(other_project)
lazy val other_project = RootProject(uri("http://github.com/jfalkner/scala-bootstrap.git#v1.2.3"))
```

All of the above is helpful since it makes it convenient to peg builds of the code or projects that use your code to a specific version. The `git tag` use also provides a convenient message for a changelog that displays these logical versions.

You can now auto-generate your `CHANGELOG.md` with this line.

```bash
git tag -ln | sort -r > CHANGELOG.md
```

You'll now have a CHANGELOG.md such as the following.

```
v1.2.3          Fixed broken tests by importing correct libraries.
v1.2.2          Patch to fix bug Z. See scala-boostrap#789
v1.2.1          Patch to fix bug Y. See scala-boostrap#456
v1.2.0          Refactored API to use new framework xyz. 50% faster performance.
v1.1.94          Patch to fix bug X. See scala-boostrap#123
...
```

Don't like the tag messages? Edit them via git and put pressure on whoever is making crappy log messages to improve them. You can use `git log 1.2.3` to see the full log message, author, time and commit hash. 


## Private Forks of Public Repos

If you must, here is a strategy to fork public repos so that you have a private version. For example, your company uses BitBucket and you are using a GitHub-hosted scala API (e.g. [jfalkner/file_monitor](https://github.com/jfalkner/file_monitor)).

The main benefits here are that you will have a private company copy that you can edit it directly. It is not common that public repos are deleted, and worse case you will have the compiled JAR with any previous release. Regardless, a full source-code copy may be desired. Keep in mind that it may be a bad idea to edit the code without contributing back to the main repo. It fragments the codebase and becomes more code that you must maintain.

```
# start with the GH repo
git clone https://github.com/jfalkner/file_monitor.git
cd file_monitor

# checkout the branch from your company's BB
git remote add bb http://git@bitbucket.mycompany.com:7990/scm/itg/mycompany_file_monitor.git
git fetch bb
git checkout -b mycompany_fork bb/mycompany_fork

# do any edits, commits and merges

# push back to BB then make a PR there
git push -u bb mycompany_fork
```

The convention above is to have a `mycompany_fork` branch as the main branch in your company's copy. You can they make all builds dependent on that and otherwise leave version numbers the same. If you end up altering the source-code, then you'll need to track your own version numbers. e.g. if `1.2.3` is the public repo's version, append another minor for your company's copy: `1.2.3.1`.
