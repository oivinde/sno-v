# Single Node OpenShift with Virtualization HowTo for Lenovo P340 (Tiny)

HowTo is ment for setting up a quick demo for non-production use, and it is recommended use other ssh-keys than in this guide.

There is some configuration required for running virtual machines after using the Assisted Installer, namely defining persistant storage and for this use case, bridged networking for the virtual machines, and this is the main focus of this document.

## Machine config:
CPU&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: 8-core (16 vCPU) </br>
RAM&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: 64GB</br>
Disks&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: 2 x NVMe M.2 (One for install, one for persistent storage)</br>
NICs&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: Internal + external USB-C (Belkin F2CU040)</br>
Screen&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: Small portable (mini-HDMI), powered by USB from computer</br>
Keyboard&nbsp;: Small wireless keyboard

## Base install

New to OpenShift Assisted Installer? For visual howto, use this article: 

https://cloud.redhat.com/blog/visual-guide-to-single-node-openshift-deploy

Before following that guide, note that there are a couple of changes that needs to be made:

- Remember to enable checkbox for “OpenShift Virtualization”</br>
- We will use specific SSH-key</br>

**Cluster name** : ocpmini.somedomain.com</br>
**Node name** : ocp1.somedomain.com</br>
**IP**: 192.168.0.165/24</br>
**Public key**	: (Also save this to client as “ocpmini_rsa.pub)</br>

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCojCwqJNSNuHndGlavlcstGORyFbyBfvKBsc/p1FP9ycKJHX
entFqJ6IhCMpMRr3X7Y+TJSnAqYy6lGQ9cPo7A8zQeV68igQYGOGv09p5vm39M6Ps98n1W06qYgb0BokPMcsB5
KPWIWldrOiDg7x8SvMdFWhqg7A4epvn+xKqYHCvcP3oha4r725rUTkAx8Akf1XRLDJhzueiBsX3s7wyBzzZhoN
3uDFSpISCD00V1QB/P3DwpyQx5spLVK9yvAFK6zoVnnsQEBsjcZlAnwYWkFjKtc1fquny2R2qDF1iTdF80XPYX
aAO6to1ggCiIRf4pBub/+Lm9pbt4Qr+IVAwkXukFBm0vpaKkbKP/WnO+X/6Rax9KHZR/O20eceYwHyM8L/ZFBB
a3q0XjkpejEhVrH0O+3U6QEyfrlEVykj/9P+d1f3RXhRx0R+VHz8FW2sv3DUEPraR3Q79TP6Ww1/Br4O4BGAYJ
L5Qo/UycnLg0DImnTiKOG+gg2rksoKZmFfc=
```

Generate install ISO, and just follow the few steps. When node is up and running, take note of kubeadmin password and edit the /etc/hosts file locally on the client as described here. Remember to use same domain name as in installation.

/etc/hosts

```
192.168.0.165   api.ocpmini.somedomain.com
192.168.0.165   oauth-openshift.apps.ocpmini.somedomain.com
192.168.0.165   console-openshift-console.apps.ocpmini.somedomain.com
192.168.0.165   grafana-openshift-monitoring.apps.ocpmini.somedomain.com
192.168.0.165   thanos-querier-openshift-monitoring.apps.ocpmini.somedomain.com
192.168.0.165   prometheus-k8s-openshift-monitoring.apps.ocpmini.somedomain.com
192.168.0.165   alertmanager-main-openshift-monitoring.apps.ocpmini.somedomain.com
192.168.0.165   cdi-uploadproxy-openshift-cnv.apps.ocpmini.somedomain.com
192.168.0.165   ocp1
```

## Console access

Download and install command line tools from here https://console.redhat.com/openshift/downloads to get the “oc” command.

Save the follow SSH private key to a file (in Linux, remember chmod 600) named ocpmini_rsa:

```
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----

```

The first thing we need to figure out, is the device name for the USB-C network adapter that will be used for bridged networking for the virtual machines.

From client, logon to the node with:

```
ssh -i / *path to private key* /ocpmini_rsa core@192.168.0.165
```

In session, just do a *ip a | grep enp*, and in this case the adapter will show up with a device name similar to this: **enp0s20f0u4**

Since you are already logged on, let’s partition the disk for persistent storage right away:

Find the name by doing the following: *ls /dev | grep nvme*. This will show both local disks, and in my case the first disk with several partitions is 0, and the second with one volume is 1. Example: **nvme1**

To create a partition and filesystem on that drive: (make sure you are partitioning the correct device)

```
sudo fdisk /dev/nvme1n1
```

Select “n” for a new partition and just confirm default choices. After this is done, “w” to write changes and exit.

Next create file system with: (make sure you are formatting the correct device)

```
sudo mkfs.xfs /dev/nvme1n1p1 
```

Exit SSH session for now.

## Configure persistent storage

Logon to the WebUI *(or use the oc command if that suits you better)*. We are doing several steps here with copy/paste from yaml files.

**HostPathProvisioner**

In UI, select the installed Virtualization Operator under “Installed Operators”, and the tab to the right that says “HostPathProvisioner deployment. Create new, select yaml and paste the following:

```
apiVersion: hostpathprovisioner.kubevirt.io/v1beta1
kind: HostPathProvisioner
metadata:
 name: hostpath-provisioner
spec:
 imagePullPolicy: IfNotPresent
 pathConfig:
   path: /var/srv/storage
   useNamingPrefix: false
```

Next in the admin menu is “Storage” and “StorageClasses”. Create new storage class and paste the following code:

```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
 name: hostpath-provisioner
 annotations:
   storageclass.kubernetes.io/is-default-class: "true"
provisioner: kubevirt.io/hostpath-provisioner
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsumer
```

Then, under “Compute” - “MachineConfig” create the following: *(mandates that partitioning and formatting has already been done. Remember to edit with correct device name for disk)*

```
apiVersion: machineconfiguration.openshift.io/v1
kind: MachineConfig
metadata:
 name: 50-cnv-local-storage
 labels:
   machineconfiguration.openshift.io/role: master
spec:
 config:
   ignition:
     version: 3.1.0
   systemd:
     units:
       - contents: |
           [Unit]
           Description=Create mountpoint /var/srv/storage
           Before=kubelet.service
 
           [Service]
           ExecStart=/bin/mkdir -p /var/srv/storage
 
           [Install]
           WantedBy=var-srv-storage.mount
         enabled: true
         name: create-mountpoint-var-srv-storage.service
       - name: var-srv-storage.mount
         enabled: true
         contents: |
           [Unit]
           Before=local-fs.target
           [Mount]
           What=/dev/nvme1n1p1
           Where=/var/srv/storage
           Type=xfs
           [Install]
           WantedBy=local-fs.target
       - contents: |
           [Unit]
           Description=Set SELinux chcon for hostpath provisioner
           Before=kubelet.service
 
           [Service]
           ExecStart=/usr/bin/chcon -Rt container_file_t /var/srv/storage
 
           [Install]
           WantedBy=multi-user.target
         enabled: true
         name: hostpath-provisioner.service
```
 



## Configure bridged networking for virtual machines


Now log on the cluster with the oc command from you client.

```
oc login -u kubeadmin -p <thepasswordfrominstaller> https://api.ocpmini.somedomain.com:6443
```

Create a file (br1-bridge.yaml) with the following content on your client (remember to use the correct USB-C NIC device name). This is the node Network Configuration Policy.

```
apiVersion: nmstate.io/v1beta1
kind: NodeNetworkConfigurationPolicy
metadata:
 name: br1-enp0s20f0u4-policy
spec:
 nodeSelector:
   node-role.kubernetes.io/worker: ""
 desiredState:
   interfaces:
     - name: br1
       description: Linux bridge with enp0s20f0u4
       type: linux-bridge
       state: up
       ipv4:
         dhcp: true
         enabled: true
       bridge:
         options:
           stp:
             enabled: false
         port:
           - name: enp0s20f0u4
```


Apply the policy with the oc command like this:

```
oc apply -f br1-bridge.yaml
```

Last thing we need to do is create a Network Attachment Definition under Network in the WebUI. Paste the following yaml:

```
apiVersion: "k8s.cni.cncf.io/v1"
kind: NetworkAttachmentDefinition
metadata:
 name: a-bridge-network 
spec:
 config: '{
   "cniVersion": "0.3.1",
   "name": "a-bridge-network",
   "type": "cnv-bridge",
   "bridge": "br1" 
 }'
```

Now the post config of OCP is done, and we can move on to creating templates and virtual machines to test if we have been successful.

Create template and virtual machine

Decide if you want to use ISO-files or qcow2 as base. Select the desired template in the WebUI under “Workloads” - “Virtualization” - “Templates”. Click “Add source”. You can upload directly or use valid URLs. If upload starts and progress to a progressbar, persistent storage config has been successful. 

For RHEL, if you use qcow2 image, you can use cloud-init for “unattend” install. With ISO, remember to add OS-disk if you select to customize in the wizard. And we are going to customize the image to replace the default nic with a bridged nic. Simply add a new nic, select the bridged option as named earlier, and delete the default one.

Your machine should now reach out to the bridged network to get an IP. 

