Render a README.Rmd to README.md then open a PR with the changes (UNTESTED)

```yaml
on:
  schedule:
    # run on Monday day at 11 PM
    - cron: '0 23 * * 1'

jobs:
  name: Render readme
  runs-on: macOS-latest
  steps:
    - uses: actions/checkout@v1
    - uses: r-lib/actions/setup-r@master
    - name: Install dependencies
      run: Rscript -e 'install.packges(c("remotes"))' -e 'remotes::install_deps(dependencies = TRUE)'
    - name: Render README
      run: Rscript -e 'rmarkdown::render("README.Rmd")'
    - name: Commit results
      run: |
        git checkout -b build-readme
        git commit README.md -m 'Re-build readme' --author 'GitHub Actions <actions@github.com>'
    - name: Create Pull Request
      uses: peter-evans/create-pull-request@v1.5.1-multi
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
