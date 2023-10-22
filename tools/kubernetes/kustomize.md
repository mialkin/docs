# Kustomize

[↑ Kustomize](https://kustomize.io) is a tool that lets you customize raw, template-free YAML files for multiple purposes, leaving the original YAML untouched and usable as is.

## Installation

```bash
brew install kustomize
```

## Generating a `kustomization.yaml` file

It is recommended to generate the `kustomization.yaml` on your own and store it in Git.

Assuming your manifests are inside `apps/my-app`, you can generate a `kustomization.yaml` with:

```bash
cd apps/my-app
kustomize create --autodetect --recursive
```

## Commands

[↑ Commands](https://kubectl.docs.kubernetes.io/references/kustomize/cmd).
