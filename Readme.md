
# Required Inputs: *

**service_account_key** - A Google service account key that has access to artifact registry in the configure project.

**project** - The Google account project

**zone** - The Zone that is required

# Optional Inputs:

Based on what you wish to do in this action.

**cluster** - The k8s cluster name that gcloud auth and skaffold will use, this input is used to toggle on skaffold deployment, without it cluster auth will not be run.

**skaffold_profile** - The skaffold Profile that is run, when the above is in use, is required when above is on.

**docker_slug** - Only required when building a docker image e.g: europe-docker.pkg.dev/remotecompany/autossl-caddy/autossl-caddy

**docker_file** - Only required when building a docker image e.g: docker/mailerlite/Dockerfile "defaults too Dockerfile"

**docker_tag** - if specified will allow yu to overide using the defualt git tag.

**docker_build_extra** - if specified will add docker build extra commands (see example).

**artifact_registry** - The artifact_registry to setup auth agains e.g: us-east1-docker.pkg.dev (see example)

# Examples
Running skaffold to deploy in the project?

``` yaml
- uses: remotecompany/gcloud-setup-deploy-action@v1.0.14
  with:
    service_account_key: ${{ secrets.GOOGLE_SERVICE_KEY }}
    project: "remotecompany"
    zone: "europe-west1"
    cluster: "mailerlite-v2"
    skaffold_profile: "staging"
```

Just Building a docker image? the version will be grabbed from the last tag (make sure this is something like v1.0.1 etc)
``` yaml
- uses: remotecompany/gcloud-setup-deploy-action@v1.0.14
  with:
    service_account_key: ${{ secrets.GOOGLE_SERVICE_KEY }}
    project: "remotecompany"
    zone: "europe-west1"
    docker_slug: "europe-docker.pkg.dev/remotecompany/autossl-caddy/autossl-caddy"
    docker_file: "Dockerfile"
```

Overide git tag
``` yaml
- uses: remotecompany/gcloud-setup-deploy-action@v1.0.14
  with:
    service_account_key: ${{ secrets.GOOGLE_SERVICE_KEY }}
    project: "remotecompany"
    zone: "europe-west1"
    docker_slug: "europe-docker.pkg.dev/remotecompany/autossl-caddy/autossl-caddy"
    docker_file: "Dockerfile"
    docker_tag: "v1.0.2"
```

Add docker extra args for github token etc
``` yaml
- uses: remotecompany/gcloud-setup-deploy-action@v1.0.14
  with:
    service_account_key: ${{ secrets.GOOGLE_SERVICE_KEY }}
    project: "remotecompany"
    zone: "europe-west1"
    docker_slug: "europe-docker.pkg.dev/remotecompany/autossl-caddy/autossl-caddy"
    docker_file: "Dockerfile"
    docker_build_extra: "--build-arg GITHUB_TOKEN=${{ secrets.GITHUBTOKEN }}"
```

Use Diffrent artifact registry
``` yaml
- uses: remotecompany/gcloud-setup-deploy-action@v1.0.14
  with:
    service_account_key: ${{ secrets.GOOGLE_SERVICE_KEY }}
    project: "remotecompany"
    zone: "europe-west1"
    cluster: "mailerlite-v2"
    skaffold_profile: "staging"
    artifact_registry: "us-east1-docker.pkg.dev"
```

Add docker extra args for multi Arch build extra steps needed
``` yaml
- name: Set up QEMU
  uses: docker/setup-qemu-action@master
  with:
    platforms: amd64,arm64

- name: Set up Docker Buildx
  id: buildx
  uses: docker/setup-buildx-action@master

- uses: remotecompany/gcloud-setup-deploy-action@v1.0.14
  with:
    service_account_key: ${{ secrets.GOOGLE_SERVICE_KEY }}
    project: "remotecompany"
    zone: "europe-west1"
    docker_slug: "europe-docker.pkg.dev/remotecompany/autossl-caddy/autossl-caddy"
    docker_file: "Dockerfile"
    docker_build_extra: "--platform=linux/amd64,linux/arm64"
```
