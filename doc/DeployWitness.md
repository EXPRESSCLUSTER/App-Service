# Deploy Witness Server

## Index
- [Overview](#overview)
- [Deploy Witness Server](#deploy-witness-server)
- [Set Witness Server to Cluster Properties](#set-witness-server-to-cluster-properties)

## Overview
- The cluster nodes are running on availability zone 1 and 2.
- The witness server runs on a different subnet.
  ```
          +-------------------------------+
          | App Service                   |
          | clpwitnessd.azurewebsites.net |
          +---------------+---------------+
                          |
             +------------+------------+
             |                         |
  +----------+----------+   +----------+----------+
  | Virtual Machine     |   | Virtual Machine     |
  | Availability zone 1 |   | Availability zone 2 |
  | centos01            |   | centos02            |
  | 10.0.0.6/24         |   | 10.0.0.7/24         |
  +---------------------+   +---------------------+
  ```

## Deploy Witness Server
1. Click **App Services** icon and click **Create**.
1. Set the following parameters and create the container.
   - Basics
     - Resource group: your resource group
     - Name: your site name (E.g. clpwitnessd)
     - Publish: Docker Container
     - Operating System: Linux
     - Region: your region
   - Docker
     - Options: Single Container
     - Image source: Docker Hub
     - Access Type: Public
     - Image and Tag: epxresscluster/clpwitnessd
     - Startup Command: (NULL)
   - Monitoring
     - Enable Application Insight: No
   - Tags
     - Set your own tags.

## Set Witness Server to Cluster Properties
1. Start Cluster WebUI and change **Config** mode.
1. Open **Cluster Properties** and click **Interconnect** tab.
1. Click **Add** and select **Witness**.
1. Click **Properties** and set the following parameter.
   - Target Host: clpwitnessd.azurewebsites.net
   - Service Port: 8080
   - See also; 
     - Windows: https://www.manuals.nec.co.jp/contents/system/files/nec_manuals/node/539/W43_RG_EN/W_RG_02.html#interconnect-tab
     - Linux: https://www.manuals.nec.co.jp/contents/system/files/nec_manuals/node/540/L43_RG_EN/L_RG_02.html#interconnect-tab
1. Apply the configuration file.
1. Check the cluster status with clpstat command.
   ```
   [azureuser@centos01 ~]$ sudo clpstat
    ========================  CLUSTER STATUS  ===========================
     Cluster : cluster
     <server>
      *centos01 ........: Online
         lankhb1        : Normal           Kernel Mode LAN Heartbeat
         witnesshb1     : Normal           Witness Heartbeat
         httpnp1        : Normal           http resolution
       centos02 ........: Online
         lankhb1        : Normal           Kernel Mode LAN Heartbeat
         witnesshb1     : Normal           Witness Heartbeat
         httpnp1        : Normal           http resolution
     <group>
       failover ........: Online
       (snip)   
   ```
