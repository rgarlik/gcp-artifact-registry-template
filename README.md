# GCP Artifact Registry Tempalte

This project contains a [GitHub Action](https://github.com/features/actions) that builds a [Docker](https://www.docker.com/) image and deploys it to [Google Cloud](https://cloud.google.com/)'s [Artifact Registry](https://cloud.google.com/artifact-registry).

The workflow is located in `.github/workflows/build-and-deploy.yml`. Copying the YAML file over into your own project is enough to get it working.

The action deploys the Docker image under two tags. The first one is `latest` and the other one is the current [commit's SHA number](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History).

## Prerequisites

Do the following in your Google Cloud project for this GitHub Action to work:

- [Enable](https://cloud.google.com/artifact-registry/docs/enable-service) the Artifact Registry service
- Create a [service account](https://cloud.google.com/iam/docs/service-accounts) with the _Artifact Registry Writer_ (`roles/artifactregistry.writer`) role
- Create a [repository](https://cloud.google.com/artifact-registry/docs/repositories) in Artifact Registry with the format of `Docker`.

## Configuration

The following [environmental variables](https://docs.github.com/en/actions/learn-github-actions/variables) need to be configured either through GitHub's settings page or by overriding them in the YAML file (not recommended).

| Variable       | Description                                                                                                 |
| -------------- | ----------------------------------------------------------------------------------------------------------- |
| `PROJECT_NAME` | Your Google Cloud [project name](https://cloud.google.com/resource-manager/docs/creating-managing-projects) |
| `IMAGE_NAME`   | Your Docker image name (used both locally and in the registry)                                              |
| `REGION`       | Your Google Cloud [region](https://cloud.google.com/compute/docs/regions-zones)                             |
| `REPOSITORY`   | Your Artifact Registry [repository](https://cloud.google.com/artifact-registry/docs/repositories)           |

The following [secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets) are used by the action, mostly to authenticate through `google-github-actions/auth`. Secrets can only be added through the settings page. This workflow uses the `credentials_json` method of authentication which is **less secure** than `workload_identity_provider` (though it's easier to set up). If you'd rather use Indentity providers, check out the commented out section in the YAML file or read about the alternative method [here](https://github.com/google-github-actions/auth).

| Secret            | Description                                                                                                                                                                   |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `GCP_CREDENTIALS` | Your [service account](https://cloud.google.com/iam/docs/service-accounts)'s [credentials](https://developers.google.com/workspace/guides/create-credentials#service-account) |
