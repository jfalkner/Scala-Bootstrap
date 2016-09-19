# Scala-Bootstrap

> WIP: Just put this up so that I can add notes as they come up. If interested, please wait for v1!

Working with Scala and still learning stuff? Bootstrap examples for a reasonable coding workflow and team dev process


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
