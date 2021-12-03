# deutschland-generator-action
Github action for the generation process of generating docs and code for the deutschland package

## Inputs

| Input                | Required | Default      | Description                                                       | Example                              |
|----------------------|----------|--------------|-------------------------------------------------------------------|--------------------------------------|
| `openapi-file`       | true     | openapi.yaml | The path to the OpenAPI document to generate a client library for | ./api/openapi.json                   |
| `commit-to-git`      | false    | false        | Should the generated code be added to git                                | true                                 |
| `upload-to-pypi`     | false    | false        | Should package be uploaded to pypi                                | false                                |
| `upload-to-testpypi` | false    | false        | Should package be uploaded to testpypi                              | false                                |
| `pypi-token`         | false    | 'undefinded' | Token to upload package to pypi                                | `${{ secrets.PYPI_API_TOKEN }}`      |
| `testpypi-token`     | false    | 'undefined'  | Token to upload package to testpypi                                | `${{ secrets.TEST_PYPI_API_TOKEN }}` |
| `python-version`     | true     | 3.7          | Python version to run on                               | 3.8                                  |

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
          openapi-file: ${{ github.workspace }}/openapi.yaml
          commit-to-git: true
          upload-to-pypi: false
          upload-to-testpypi: true
          pypi-token: ${{ secrets.PYPI_API_TOKEN }}
          testpypi-token: ${{ secrets.TEST_PYPI_API_TOKEN }}
          python-version: ${{ matrix.python-version }}
```