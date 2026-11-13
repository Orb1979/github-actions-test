# Required github / repo settings

```
# you might need to request additional scope, so you are allowed to push workflows
gh auth status
gh auth refresh --hostname github.com --scopes workflow

# enable dependency scanning
github repo > settings > advanced security > Dependency graph > enable
```

# Workflow CI
**ci.yml** — runs on every push to main and every PR to main

```
- Set up JDK 21 (Temurin) + Gradle (with dependency caching)
- Run unit tests         (./gradlew test)
- Run integration tests  (./gradlew integrationTest) (Testcontainers -> needs Docker, no docker-compose)
- Build                  (./gradlew build) (tests already ran, Gradle skips up-to-date tasks)
- Upload test reports as artifact on failure (7-day retention)
- Superseded PR runs are cancelled
- dependency-submission job -> dependency graph -> Dependabot alerts
```

# Workflow Publish image (CD)
**publish-image.yml** — runs on push to main (`latest`) and on `v*` tags (`x.y.z`)

```
Step  Implementation
- ./gradlew check — tests must pass before anything is published
- docker login to ghcr.io (GITHUB_TOKEN, permission packages:write)
- Compute image ref: ghcr.io/<owner>/rest-api-books:<latest|x.y.z> (GHCR only accepts lowercase image names)
- Gradle bootBuildImage --publishImage — Cloud Native Buildpacks image, pushed to GHCR (GitHub Packages)
```

Note: the first push creates a *private* package; flip visibility in the
package settings if it should be pullable publicly.

# todo
```
Tag image using Git commit SHA
Update deployment.yaml with the new image tag
Commit and push GitOps repository
Argo CD deploys to local K3s (Rancher)
```
