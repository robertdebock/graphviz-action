# Graphviz action

A GitHub action to generate a PNG out of your [Ansible](https://www.ansible.com/) roles requirements.yml.

# Requirements

This action will pickup `requirement.yml` (in the root of you role) and generate two files:

- requirements.dot (A translation of `requirements.yml` to GraphViz' language.)
- requirements.png

## Example usage

```yaml
---
on:
  - push

jobs:
  build:
    runs-on: ubuntu-20.04
    steps:
      - name: checkout
        uses: actions/checkout@v2
        with:
          path: "${{ github.repository }}"
      - name: graphviz
        uses: robertdebock/graphviz-action@1.0.0
      - name: savepng
      - uses: actions/upload-artifact@v2
        with:
          name: requirementsdot
          path: requirements.*
```
