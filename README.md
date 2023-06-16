# deutschland-generator-action
Github action to generate a Python client library based on a remote OpenAPI specification for use in the `deutschland` package

## Inputs

| Input                | Required | Default      | Description                                                       | Example                              |
|----------------------|----------|--------------|-------------------------------------------------------------------|--------------------------------------|
| `openapi-uri`       | true     |  | The URI of the OpenAPI document to generate a client library for | https://search.dip.bundestag.de/api/v1/openapi.yaml                   |
| `commit-to-git`      | false    | false        | Should the generated code be added to git                                | true                                 |
| `upload-to-pypi`     | false    | false        | Should package be uploaded to pypi                                | false                                |
| `upload-to-testpypi` | false    | false        | Should package be uploaded to testpypi                              | false                                |
| `pypi-token`         | false    | 'undefinded' | Token to upload package to pypi                                | `${{ secrets.PYPI_API_TOKEN }}`      |
| `testpypi-token`     | false    | 'undefined'  | Token to upload package to testpypi                                | `${{ secrets.TEST_PYPI_API_TOKEN }}` |
| `python-version`     | true     | 3.7          | Python version to run on                               | 3.8                                  |

If you want to use your custom templates, put them into a folder called ```deutschland_templates``` inside the root of your own repostitory. If it exists, the action will use your templates, if not it will copy over the default templates from this action's repository.

> Note: the downloaded OpenAPI specification will always be saved as `openapi.yaml` on the root level of your repository.

## Example

```yaml
on: [push, pull_request]
jobs:
  deutschland_generation:
    name: "Deutschland Generation"
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false
      matrix:
        python-version: ['3.7.8' ]
    steps:
      - name: "Generate deutschland code"
        uses: wirthual/deutschland-generator-action@v0.8.0
        with:
          openapi-uri: https://search.dip.bundestag.de/api/v1/openapi.yaml
          commit-to-git: true
          upload-to-pypi: false
          upload-to-testpypi: true
          pypi-token: ${{ secrets.PYPI_API_TOKEN }}
          testpypi-token: ${{ secrets.TEST_PYPI_API_TOKEN }}
          python-version: ${{ matrix.python-version }}
```
