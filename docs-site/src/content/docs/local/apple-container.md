---
title: Apple Container DNS Setup
description: Manual steps to configure DNS for Apple Container runtime on macOS.
---

When using Apple Container as your container runtime, Scion agents need to reach the Hub server. This requires a DNS rule that maps `host.containers.internal` to the loopback address.

Apple Container's DNS rules persist across sessions but the underlying PF (packet filter) rules do not survive macOS reboots. You need to re-run this command after each reboot:

```bash
sudo container system dns create host.containers.internal --localhost 203.0.113.1
```

## Why sudo is required

The DNS setup modifies macOS PF (packet filter) rules, which require root access. This is an Apple Container limitation — there is no rootless alternative for PF rules.

## Automating after reboot

To avoid running this manually after each reboot, you can add it to a launchd plist or run it as part of your startup scripts.

## Verification

To check if the DNS rule is configured:

```bash
container system dns list
```

You should see `host.containers.internal` in the output.
