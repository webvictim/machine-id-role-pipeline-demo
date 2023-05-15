## machine-id-role-pipeline-demo

An example of applying a directory of roles stored as YAML files to a Teleport cluster via Machine ID and Github Actions.

### Prerequisites

- A working Teleport cluster running Teleport 11+

### Setup

- Fork this repo
- Update [.github/workflows/role-pipeline.yaml](.github/workflows/role-pipeline.yaml) and change:
  - The proxy address of your Teleport cluster
  - The Teleport cluster version
- Apply a token to the Teleport cluster, whitelisting the forked Github repo name:

```console
$ cat <<EOF > token.yaml
kind: token
version: v2
metadata:
  name: github-token
  expires: "2100-01-01T00:00:00Z"
spec:
  roles: [Bot]
  join_method: github
  bot_name: github-demo
  github:
    allow:
      # update this
      - repository: webvictim/machine-id-role-pipeline-demo
EOF

$ tctl create -f token.yaml
```

That's pretty much it. Commits to the [`roles`](roles) directory in the repo will then automatically be applied to the cluster.
