# kube-marketplace

A Claude Code plugin marketplace for Kubernetes development workflows. Provides plugins for committing changes, creating pull requests, and reviewing Kubernetes APIs -- all following Kubernetes project conventions.

## Plugins

| Plugin | Description | Command |
|--------|-------------|---------|
| **commit-from-changes** | Stage and commit changes with Kubernetes-style commit messages | `/commit-from-changes:commit-from-changes` |
| **create-pr-from-changes** | Create a GitHub PR from the current branch following the repo's PR template | `/create-pr-from-changes:create-pr-from-changes` |
| **kube-api-review** | Review Kubernetes API types against API conventions | `/kube-api-review:kube-api-review` |

See [PLUGINS.md](PLUGINS.md) for detailed documentation on each plugin and its commands.

## Installation

```bash
# Add this marketplace
/plugin marketplace add kannon92/kube-marketplace-template

# Install a plugin
/plugin install commit-from-changes@kube-marketplace-template
/plugin install create-pr-from-changes@kube-marketplace-template
/plugin install kube-api-review@kube-marketplace-template
```

## Development

### Create a new plugin

```bash
make new-plugin NAME=my-plugin
```

### Lint plugins

```bash
make lint
```

### Update documentation

```bash
make update
```

### All make targets

| Command | Description |
|---------|-------------|
| `make help` | Show all available commands |
| `make lint` | Run plugin linter |
| `make lint-pull` | Pull latest linter image |
| `make update` | Update docs and website data |
| `make update-from-template` | Pull template updates |
| `make new-plugin NAME=foo` | Create a new plugin |

## Contributing

1. Run `make lint` before committing
2. Follow the existing plugin structure
3. Update documentation when adding plugins

## License

MIT
