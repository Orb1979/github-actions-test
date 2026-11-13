# github-actions-test

```
# you might need to request additional scope, so you are allowed to push workflows
gh auth status
gh auth refresh --hostname github.com --scopes workflow

# enable dependency scanning
github repo > settings > advanced security > Dependency graph > enable
```



```
gradle.yml
- Set up JDK 21
- Setup Gradle
- Run unit tests
- Run integration tests
- build project
- Upload test reports on failure
- dependency submission
```

The dependency-submission action:
1. Runs Gradle to resolve the project's dependencies.
2. Collects the resolved dependency information, including transitive dependencies.
3. Generates a dependency graph snapshot.
4. Submits that snapshot to GitHub.
   It does this without actually building and testing your application.
   GitHub+1

You can find it in: Repository → Insights → Dependency graph

Suppose a vulnerability is discovered in a library you're using
GitHub can compare your dependency graph against its vulnerability database.
It can then notify you that your project uses a vulnerable version.

Depending on the dependency and configuration, Dependabot can also suggest or create updates.