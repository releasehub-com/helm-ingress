# How to Host Helm Charts on GitHub
This is the poor person's version of hosting a chart on GitHub, most examples show how to use GitHub Actions with GitHub Pages, but that's not necessary for the short term.

The Helm chart only requires that the file `index.yaml` be accessible from the Helm URL that is provided. The `index.yaml` can then point anywhere you like to host the bundled tar file with the chart.

Therefore, this example just relies on giving a GitHub `raw` link to the index.yaml and we put the tar file in the same location. We could easily publish a GitHub version and use the version url for the tar file but that requires more steps. See https://github.com/helm/chart-releaser It looks easier to use but you need to generate a token. Would be nice if it used your ssh credentials from the local repository or something.

## Update the Chart
Create a branch, edit the template, values, Chart, whatever. Commit the changes and push.

## Change the Version
This is important: make sure you edit `Chart.yaml` and update the semver. It is possible to use `helm` commands to update the version too.

## Bundle the Artifact
This will create a tar file with the contents of the directory, like `releaes-ingres-0.0.0.tgz`. It will include the previous tar version too :P This is the world's lamest version of `tar cvfz .` written in Go for people who don't know Un\*x.

```bash
helm package .
```

## Generate a New Index
The repo index will be overwritten by default, so we use `--merge`. The URL is the GitHub `raw` link to the repository root where the `index.yaml` will be.

```bash
helm repo index . --merge index.yaml --url https://raw.githubusercontent.com/releasehub-com/helm-ingress/main/
```

## Verify
Check `git diff` to see changes to index.yaml. Deprecate previous versions as necessary.

## Publish
Push your changes to the branch, ask for a code review and then merge.

## Profit
