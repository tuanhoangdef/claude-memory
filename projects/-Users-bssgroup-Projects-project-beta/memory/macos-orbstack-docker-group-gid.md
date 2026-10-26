---
name: macos-orbstack-docker-group-gid
description: On this macOS box Docker is OrbStack, so DOCKER_GROUP_GID must be 0, not the Linux value 984.
metadata:
  type: project
---

`.env`'s `DOCKER_GROUP_GID` must be **0** on this machine, not 984.

Docker here is OrbStack (socket `~/.orbstack/run/docker.sock`, symlinked from
`/var/run/docker.sock`). Inside a container the bind-mounted socket is `uid=0 gid=0 mode=660`,
so the app container's non-root user needs supplementary group **0** to reach it. There is no
`docker` group; 984 was the Linux host's docker gid and grants nothing here.

**Why:** the app container's PM2-managed OpenSandbox server talks to the daemon over that
socket. With the wrong gid it cannot, the slot reaper holds every slot, and no task ever
dispatches — a failure that looks like a scheduler bug, not a permissions one.

**How to apply:** verified by `docker run -u 501:20 --group-add 0 -v /var/run/docker.sock:... `
being able to write the socket while the same run without `--group-add 0` cannot. Note
`DOCKER_UID`/`DOCKER_GID` do NOT need changing from 1000: OrbStack maps every container uid, so
files land owned by the host user regardless. Backup of the Linux original is `.env.bak-linux`.
