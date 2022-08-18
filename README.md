# Forge Shared Automation

Shared automation and actions workflows utilized by the Tyler Forge team.

## Referencing Reusable Workflows

For a complete overview of reusable workflows, see official github documentation: [reusing-workflows](https://docs.github.com/en/actions/learn-github-actions/reusing-workflows).

## Workflow Files

As a general standard, all reusable workflows stored in this repository should be prefixed with `wf-`. While this isn't a technical requirement, it helps maintainers and consumers separate workflows designed to be consumed by external callers, from those which are used for local automation and management.

The following shared workflow files are maintained in this repository.

### auto-pr-check
 
The [auto-pr-check](./.github/workflows/wf-auto-pr-check.yml) workflow will execute the [auto pr-check](https://intuit.github.io/auto/docs/generated/pr-check) command against a project configured with the [auto](https://intuit.github.io/auto/index) release framework.

### build-and-test

The [build-and-test](./.github/workflows/wf-build-and-test.yml) workflow supports executing builds and running tests in node projects.

### build-release

The [build-release](./.github/workflows/wf-build-release.yml) workflow supports building and releasing projects integrated with the [auto](https://intuit.github.io/auto/index) release framework. Since `auto` handles most of the heavy lifting and release logic, the workflow itself can remain relatively standard.

### cleanup-gh-pages

The [cleanup-gh-pages](./.github/workflows/wf-cleanup-gh-pages.yml) workflow is a simple reusable workflow that is responsible for cleaning up a branch from the `gh-pages` branch in repository (branch name is configurable). Typically used after a pull request is merged.

### configuration

The [configuration](./.github/workflows/wf-configuration.yml) workflow can be used as the first step in a project workflow to gather common release and build information prior to running subsequent jobs.

### publish-cloudfront-assets

The [publish-cloudfront-assets](./.github/workflows/wf-publish-cloudfront-assets.yml) workflow supports publishing assets to an s3 bucket (via `s3 sync`) and generating cloudfront invalidations for updated objects. This workflow is not responsible for building or packing the assets, instead it assumes the assets were built and packaged in a previous job within the GitHub execution pipeline and then archived (`<file>.tar.gz`) and uploaded via the [upload-artifact](https://github.com/actions/upload-artifact) github action.
 
### publish-gh-pages

The [publish-gh-pages](./.github/workflows/wf-publish-gh-pages.yml) workflow is responsible for publishing assets to GitHub Pages, typically used on a pull request for a preview of the deployment to make for easier reviewing.

## Versioning and Release

This project follows semver, and new releases are published via creating a new release in GitHub.

## Using a workflow

See the `uses` key from the snippet below for an example on how to properly reference a workflow. Take note of the version specified at the end.
You can view versions and their release notes [here](https://github.com/tyler-technologies-oss/forge-automation-shared/releases).

The following example of a job will use the [build-and-test](./.github/workflows/wf-build-and-test.yml) workflow to run a build in node and execute tests:

```yaml
build:
  name: Build and Test
  needs: wf-config
  uses: tyler-technologies-oss/forge-automation-shared/.github/workflows/wf-build-and-test.yml@v1.0.0
  with:
    TESTS_ENABLED: true
  secrets:
    NPM_TOKEN: ${{ secrets.MY_NPM_TOKEN }}
```
