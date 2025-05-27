# Nydus Snapshotter extension

## Installation

See [Installing Extensions](https://github.com/siderolabs/extensions#installing-extensions).

## Configuration

### Registry Mirrors

By default, snapshotter configuration sets `remote.mirrors_config.dir` to the directory `/etc/cri/conf.d/hosts`.

The `/etc/cri/conf.d/hosts` file is where Talos writes its CRI-compatible mirror configuration. Fortunately,
the snapshotter supports overriding nydusd mirrors given you configure it with the same directory that
containerd uses. This makes mirror configuration transparent.

### Snapshotter or Nydusd

Configuraton is broken into two parts: snapshotter and nydusd configuration. You can modify the
Talos machine configuraton to overwrite or append the respective configuration files as needed:

 - `/usr/local/etc/containerd-nydus-grpc/snapshotter-config.toml`
 - `/usr/local/etc/containerd-nydus-grpc/nydusd-config.json`

For example:
```
machine:
  files:
    - content: |
        [log]
        level = "debug"
      path: /usr/local/etc/containerd-nydus-grpc/snapshotter-config.toml
      op: append
    - content: |
        {
          "fs_prefetch": {
            "enable": false,
            "threads_count": 8,
            "merging_size": 1048576,
            "prefetch_all": true
          }
        }
      path: /usr/local/etc/containerd-nydus-grpc/nydusd-config.json
      op: overwrite
```
