# 26 - High Availability Cluster (Pacemaker / Corosync)

## Install (on all nodes)
```bash
dnf install pcs pacemaker fence-agents-all
systemctl enable --now pcsd
echo "StrongPass" | passwd --stdin hacluster
firewall-cmd --permanent --add-service=high-availability && firewall-cmd --reload
```

## Create cluster (from one node)
```bash
pcs host auth node1 node2 -u hacluster
pcs cluster setup mycluster node1 node2
pcs cluster start --all
pcs cluster enable --all
```

## Status
```bash
pcs status
pcs cluster status
pcs status nodes
pcs config
corosync-cfgtool -s
```

## Resources
```bash
pcs resource create vip ocf:heartbeat:IPaddr2 ip=192.168.1.100 cidr_netmask=24
pcs resource create web ocf:heartbeat:apache configfile=/etc/httpd/conf/httpd.conf
pcs resource group add webgroup vip web
pcs resource move web node2
pcs resource clear web
pcs resource cleanup web
pcs resource disable web ; pcs resource enable web
pcs resource delete web
```

## Constraints
```bash
pcs constraint colocation add web with vip INFINITY
pcs constraint order vip then web
pcs constraint location web prefers node1=100
pcs constraint list
```

## Fencing (STONITH)
```bash
pcs stonith list
pcs stonith create fence1 fence_ipmilan ip=10.0.0.5 username=admin password=x pcmk_host_list=node1
pcs property set stonith-enabled=true
pcs stonith fence node2
```

## Node maintenance
```bash
pcs node standby node1
pcs node unstandby node1
pcs cluster stop node1
pcs property set maintenance-mode=true
```

## Quorum
```bash
pcs quorum status
corosync-quorumtool
```
