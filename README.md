# siembol-config

To get started with Siembol, you'll need a couple of things.  Among them, you'll need a repo like this one to store the configuration for Siembol over time.  

You can either use one repository for the whole shebang, or you can set up separate repositories for each service and even use separate repositories to differentiate betweenthe store and the release configs.

The repository will need at least one commit to master so that there's a ref for `jgit` to grab.

# Authentication Configuration

Currently, you need to specify an authentication type, even if you don't configure it.  For example:

```
config-editor-auth.type=oauth2
```

# Service Configuration

At a minimum, you will need to configure at least one service for the config-editor to work.  The key lines to configure with your github

```
config-editor.services.<service_to_configure>.config-store.git-user-name=<github_username>
config-editor.services.<service_to_configure>.config-store.github-url=<https://github.com or on-premise Github URL>
config-editor.services.<service_to_configure>.config-store.store-repository-name=<store-repository-name>
config-editor.services.<service_to_configure>.config-store.release-repository-name=<release-repository-name>
```

Meanwhile, for config-editor to work with git, you'll need to create a personal access token. 

* https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token

But don't change this line:

```
config-editor.services.<service_to_configure>.config-store.git-password=${GIT_PSWD}
```

Instead, create a secret with `kubectl`:

```
kubectl create secret generic config-editor-rest-secrets --from-literal=git=<your_Github_PAT>
```

You can leave the rest of the lines in the `application.properties` file at their defaults or change them as you see fit:

```
config-editor.services.<service_to_configure>.config-store.store-repository-path=/rules/<service_to_configure>-rules-store
config-editor.services.<service_to_configure>.config-store.release-repository-path=/rules/<service_to_configure>-rules-release
config-editor.services.<service_to_configure>.config-store.store-directory=rules
config-editor.services.<service_to_configure>.config-store.release-directory=release
config-editor.services.<service_to_configure>.config-store.test-case-directory=testcases
config-editor.services.<service_to_configure>.ui-config-file-name=/opt/config-editor-rest/ui-config/<service_to_configure>-layout-config.json
```

