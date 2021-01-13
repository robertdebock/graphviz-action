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

name: Ansible Graphviz

jobs:
  build:
    runs-on: ubuntu-20.04
    steps:
      - name: checkout
        uses: actions/checkout@v2
        with:
          path: "${{ github.repository }}"
      - name: create png
        uses: robertdebock/graphviz-action@1.0.7
```

In the example above the PNG is created, but nothing is done to store the PNG.

Here is a full example that saves the generated file (`.dot` and `.png`) to a branch called `png`.

```yaml
---
on:
  - push

name: Ansible Graphviz

jobs:
  build:
    runs-on: ubuntu-20.04
    steps:
      - name: checkout
        uses: actions/checkout@v2
        with:
          path: "${{ github.repository }}"
      - name: create png
        uses: robertdebock/graphviz-action@1.0.7
      - name: Commit files
        run: |
          cd ${{ github.repository }}
          git config --local user.email "github-actions[bot]@users.noreply.github.com"
          git config --local user.name "github-actions[bot]"
          git add requirements.dot requirements.png
          git commit -m "Add generated files"
      - name: save to png branch
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          directory: ${{ github.repository }}
          force: true
          branch: png
```
