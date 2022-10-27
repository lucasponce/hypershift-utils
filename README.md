# hypershift-debug
Debug tooling for hypershift environment

# Requeriments

- `oc` CLI
- `ocm` CLI
- `hypershift` CLI
- `aws` CLI

# Preparation

Define a hc-sc-$ENVIRONMENT-credentials file with the URL of the ServiceCluster, user and password under your $HOME.

Example:

```
$ pwd
/home/lponce
$ cat hc-sc-stage-credentials
https://api.hs-sc-ccb0thlad.bgnu.s1.devshift.org:6443 fleet-manager-debug <YOUR_SECRET_PASSWORD>
```

Put the `hypershift-debug` script on your PATH (i.e. `$HOME/.local/bin`).

# Usage

```
$ hypershift-debug stage logs-demo /tmp
```

Expected output:

```
=== [stage] Service Cluster https://api.hs-sc-ccb0thlad.bgnu.s1.devshift.org:6443
Login successful.

You have access to 149 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "default".
Hypershift Namespace   : ocm-staging-1vk560949mtdg7ls93fjc6pd8k7an736
Hypershift Cluster     : logs-demo

=== [stage] Management Cluster https://api.hs-mc-cd8ni7ios.u1fc.s1.devshift.org:6443
Login successful.

You have access to 122 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "default".

machines.cluster.x-k8s.io
NAME                                 CLUSTER                            NODENAME   PROVIDERID                              PHASE         AGE   VERSION
logs-demo-workers-6d66c7b887-jq7wc   1vk560949mtdg7ls93fjc6pd8k7an736              aws:///us-west-2a/i-0570b6cc16f20ebbb   Provisioned   14m   4.12.0-0.nightly-2022-10-27-135134
logs-demo-workers-6d66c7b887-zqdpz   1vk560949mtdg7ls93fjc6pd8k7an736              aws:///us-west-2a/i-0f22c6267f207c18f   Provisioned   12m   4.12.0-0.nightly-2022-10-27-135134

awsmachines
NAME                      CLUSTER                            STATE     READY   INSTANCEID                              MACHINE
logs-demo-workers-6qggz   1vk560949mtdg7ls93fjc6pd8k7an736   running   true    aws:///us-west-2a/i-0f22c6267f207c18f   logs-demo-workers-6d66c7b887-zqdpz
logs-demo-workers-fjkmd   1vk560949mtdg7ls93fjc6pd8k7an736   running   true    aws:///us-west-2a/i-0570b6cc16f20ebbb   logs-demo-workers-6d66c7b887-jq7wc

machinedeployment
NAME                CLUSTER                            REPLICAS   READY   UPDATED   UNAVAILABLE   PHASE       AGE   VERSION
logs-demo-workers   1vk560949mtdg7ls93fjc6pd8k7an736   2                  2         2             ScalingUp   62m   4.12.0-0.nightly-2022-10-27-135134

machineset
NAME                           CLUSTER                            REPLICAS   READY   AVAILABLE   AGE   VERSION
logs-demo-workers-6d66c7b887   1vk560949mtdg7ls93fjc6pd8k7an736   2                              62m   4.12.0-0.nightly-2022-10-27-135134

hypershift operator logs

capi-provider logs
Found 3 pods, using pod/capi-provider-659dbcb9dd-84plf

events

console-logs
2022-10-27T19:09:42+02:00	INFO	Successfully retrieved console logs
```

Logs stored in:

```
$ ls -l /tmp/hs-stage-logs-demo-logs/
total 3968
-rw-rw-r-- 1 lponce lponce  410643 oct 27 19:09 capi-provider-logs.log
-rw-rw-r-- 1 lponce lponce  545391 oct 27 19:09 events.log
-rw-rw-r-- 1 lponce lponce 2965849 oct 27 19:09 hypershift-operator-logs.log
-rw-r--r-- 1 lponce lponce   65535 oct 27 19:09 logs-demo-workers-6qggz.log
-rw-r--r-- 1 lponce lponce   65535 oct 27 19:09 logs-demo-workers-fjkmd.log
```