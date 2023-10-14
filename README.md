# pipeline-challenge

A basic pipeline pulling the ubuntu image and ensuring it is secure before pushing it to ECR. Trivy is used to scan for image vulnerabilities and will fail the pipeline if any are found.

## Improve Security

- Add a solution to scan containers at runtime
- In general, a minimal base image should be used to reduce attack surface e.g. Chainguard's

## Versioning

When using conventional commits, the pipeline can be modified to automate semantic releases. A [good example](https://medium.com/agoda-engineering/automating-versioning-and-releases-using-semantic-release-6ed355ede742).

## Full pipeline

The following assumes a Blue/Green deployment strategy and an image with some custom code.

### Dev branch

A testing job should be run for every push to a dev branch: `testing (unit, integration, quality)`. A matrix should be used to run the tests in parallel.

### Staging

Once a PR is opened, the following CI/CD pipeline should be triggered:
`testing (unit, integration, quality)` -> `build` -> `scan` -> (manual step) `deploy (ECR w/ continuous scan + ECS/EKS)`

The pipeline will have a deployment environment of `staging`, with all the related variables env variables pointing at staging infrastructure (hopefully deployed with Terraform).

### Production

Once the PR is approved and merged to `master` (which should be protected), the above pipeline should be triggered with a deployment environment of `production`. An optional stage should be present to rollback the changes if any issues are found.
