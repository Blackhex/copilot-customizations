# Copilot customizations

Reusable instructions for GitHub Copilot, packaged as a plugin for supported clients.

Rule definitions live in [com.github.copilot/rules](com.github.copilot/rules/). Read the instruction files there for their current behavior and scope.

Rules guide the agent when attached to a request. Availability and behavior depend on client support, settings, and request context; instructions cannot add tool capabilities or guarantee enforcement.

## Install

In Copilot CLI:

```text
copilot plugin install Blackhex/copilot-customizations
```

In VS Code, install the plugin from its GitHub repository through the
Copilot plugin interface. The rules can be applied when a client supports
plugin rules and the plugin is installed and enabled; no custom agent
selection or skill invocation is required. File-matched instructions
may not apply to questions without an associated file.

To use an editable local checkout directly in VS Code, add its absolute
path to your **user** `settings.json` instead of installing a cached copy:

```json
"chat.pluginLocations": {
  "C:\\Projects\\copilot-customizations": true
}
```

VS Code watches plugin rule directories and the manifest in this
configuration, so changes in the checkout can be picked up without
reinstalling. Start a new chat to use updated instructions in a
conversation; an existing chat may retain its earlier context. For
Copilot CLI, use `copilot --plugin-dir C:\Projects\copilot-customizations`
to load the working tree for that invocation.

## Verify

```text
copilot plugin list
copilot instruction list
```

Confirm that `copilot-customizations` is installed. In a fresh
interactive Copilot CLI session, use `/env` to inspect the loaded
environment and `/instructions` to inspect available instruction
sources. Plugin installation alone does not prove the rules were attached
to a particular request. `copilot instruction list` may omit
plugin-contributed rules depending on client settings and file context.

To update the cached installation after a repository change, run
`copilot plugin install Blackhex/copilot-customizations` again.
