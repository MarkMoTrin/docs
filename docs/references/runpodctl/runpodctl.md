---
title: "runpodctl"
---

You can use RunPod's CLI [runpodctl](https://github.com/runpod/runpodctl) to manage Pods.

The runpodctl is a tool for managing your GPU pods on RunPod.
All Pods come with `runpodctl` installed with a Pod-scoped API key, which makes managing your Pods easier through the command line.

Choose one of the following methods to install the RunPod CLI.

### MacOs

**ARM**

```bash
wget --quiet --show-progress https://github.com/runpod/runpodctl/releases/download/v1.14.3/runpodctl-darwin-arm64 -O runpodctl && chmod +x runpodctl && sudo mv runpodctl /usr/local/bin/runpodctl
```

**AMD**

```bash
wget --quiet --show-progress https://github.com/runpod/runpodctl/releases/download/v1.14.3/runpodctl-darwin-amd64 -O runpodctl && chmod +x runpodctl && sudo mv runpodctl /usr/local/bin/runpodctl
```

### Linux

```bash
wget --quiet --show-progress https://github.com/Run-Pod/runpodctl/releases/download/v1.14.3/runpodctl-linux-amd -O runpodctl && chmod +x runpodctl && sudo cp runpodctl /usr/bin/runpodctl
```

### Windows (powershell)

```bash
wget https://github.com/runpod/runpodctl/releases/download/v1.14.3/runpodctl-win-amd -O runpodctl.exe
```

### Google Collab

```bash
!wget --quiet --show-progress https://github.com/Run-Pod/runpodctl/releases/download/v1.14.3/runpodctl-linux-amd -O runpodctl
!chmod +x runpodctl
!cp runpodctl /usr/bin/runpodctl
```

### Jupyter notebook

```bash
!wget --quiet --show-progress https://github.com/Run-Pod/runpodctl/releases/download/v1.14.3/runpodctl-linux-amd -O runpodctl
!chmod +x runpodctl
!cp runpodctl /usr/bin/runpodctl
```

## runpodctl

CLI for runpod.io

### Synopsis

CLI tool to manage your pods for runpod.io

### Options

```
-h, --help   help for runpodctl
```

### SEE ALSO

- [runpodctl config](runpodctl_config.md) - CLI Config
- [runpodctl create](runpodctl_create.md) - create a resource
- [runpodctl get](runpodctl_get.md) - get resource
- [runpodctl project](runpodctl_project.md) - Manage RunPod projects
- [runpodctl receive](runpodctl_receive.md) - receive file(s), or folder
- [runpodctl remove](runpodctl_remove.md) - remove a resource
- [runpodctl send](runpodctl_send.md) - send file(s), or folder
- [runpodctl ssh](runpodctl_ssh.md) - SSH keys and commands
- [runpodctl start](runpodctl_start.md) - start a resource
- [runpodctl stop](runpodctl_stop.md) - stop a resource
- [runpodctl update](runpodctl_update.md) - update runpodctl
- [runpodctl version](runpodctl_version.md) - runpodctl version
