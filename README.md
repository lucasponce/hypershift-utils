# hypershift-debug
Debug tooling for hypershift environment

# Requeriments

- `oc` CLI
- `ocm` CLI
- `hypershift` CLI
- `aws` CLI
- `jq`

# Preparation

Define a hc-sc-$ENVIRONMENT-credentials file with the URL of the ServiceCluster, user and password under your $HOME.

Supported $ENVIRONMENTS are `stage`, `int` or `local` for development purposes.

Please, ask the OCM team to get a password for Service Cluster / Managed Cluster debugging.  

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
=== Checking local prerequisites
=== [local] Service Cluster https://api.hs-sc-cbmcd0f3e.jlf6.i1.devshift.org:6443
Login successful.

You have access to 123 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "default".
Management Cluster     : hs-mc-cdducjuis
Hypershift Namespace   : ocm-lponce-205cfu7g1t8kh30q0vn57dolcn0betkc
Hypershift Cluster     : lponce-local-03

=== [local] Management Cluster https://api.hs-mc-cdducjuis.lq7x.i1.devshift.org:6443
Login successful.

You have access to 113 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "default".

=== Versions
Service Cluster OCP version        : 4.10.24
Management Cluster OCP version     : 4.11.9
ACM version                        : TODO
HyperShift operator image          : quay.io:443/acm-d/hypershift-rhel8-operator@sha256:834ec01781d767aafdd7e07f4e001fb6ef8141712a2f3a7480f5a224e781d71d
HostedCluster target version       : 4.12.0-rc.0

=== hostedcluster
NAME              VERSION       KUBECONFIG                         PROGRESS    AVAILABLE   PROGRESSING   MESSAGE
lponce-local-03   4.12.0-rc.0   lponce-local-03-admin-kubeconfig   Completed   True        False         The hosted control plane is available

=== nodepools
NAME                      CLUSTER           DESIRED NODES   CURRENT NODES   AUTOSCALING   AUTOREPAIR   VERSION       UPDATINGVERSION   UPDATINGCONFIG   MESSAGE
lponce-local-03-workers   lponce-local-03   2               2               False         True         4.12.0-rc.0                                      

=== machines.cluster.x-k8s.io
NAME                                       CLUSTER                            NODENAME                                     PROVIDERID                              PHASE     AGE   VERSION
lponce-local-03-workers-58f476db9c-9llx5   205cfu7g1t8kh30q0vn57dolcn0betkc   ip-10-1-139-177.us-west-2.compute.internal   aws:///us-west-2a/i-033a17619c063dbd7   Running   13h   4.12.0-rc.0
lponce-local-03-workers-58f476db9c-xlq4c   205cfu7g1t8kh30q0vn57dolcn0betkc   ip-10-1-138-252.us-west-2.compute.internal   aws:///us-west-2a/i-0349f3f7f4717474c   Running   13h   4.12.0-rc.0

=== awsmachines
NAME                            CLUSTER                            STATE     READY   INSTANCEID                              MACHINE
lponce-local-03-workers-mnhrk   205cfu7g1t8kh30q0vn57dolcn0betkc   running   true    aws:///us-west-2a/i-0349f3f7f4717474c   lponce-local-03-workers-58f476db9c-xlq4c
lponce-local-03-workers-rhb26   205cfu7g1t8kh30q0vn57dolcn0betkc   running   true    aws:///us-west-2a/i-033a17619c063dbd7   lponce-local-03-workers-58f476db9c-9llx5

=== machinedeployment
NAME                      CLUSTER                            REPLICAS   READY   UPDATED   UNAVAILABLE   PHASE     AGE   VERSION
lponce-local-03-workers   205cfu7g1t8kh30q0vn57dolcn0betkc   2          2       2         0             Running   13h   4.12.0-rc.0

=== machineset
NAME                                 CLUSTER                            REPLICAS   READY   AVAILABLE   AGE   VERSION
lponce-local-03-workers-58f476db9c   205cfu7g1t8kh30q0vn57dolcn0betkc   2          2       2           13h   4.12.0-rc.0

=== hypershift operator logs

=== capi-provider logs

=== control-plane-operator

=== events

=== console-logs
2022-11-23T09:34:39+01:00	INFO	Successfully retrieved console logs

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
