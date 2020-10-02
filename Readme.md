
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

# Examples
Running skaffold to deploy in the project?

``` yaml
- uses: remotecompany/gcloud-setup-deploy-action@v1.0.10
with:
    service_account_key: ${{ secrets.GOOGLE_SERVICE_KEY }}
    project: "remotecompany"
    zone: "europe-west1"
    cluster: "mailerlite-v2"
    skaffold_profile: "staging"
```

Just Building a docker image? the version will be grabbed from the last tag (make sure this is something like v1.0.1 etc)
``` yaml
- uses: remotecompany/gcloud-setup-deploy-action@v1.0.10
with:
    service_account_key: ${{ secrets.GOOGLE_SERVICE_KEY }}
    project: "remotecompany"
    zone: "europe-west1"
    docker_slug: "europe-docker.pkg.dev/remotecompany/autossl-caddy/autossl-caddy"
    docker_file: "Dockerfile"
```
