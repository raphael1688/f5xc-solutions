.. meta::
   :description: F5 Distributed Cloud Customer Edge SiteCLI Reference
   :keywords: Distributed Cloud, SiteCLI, execcli, configure
   :category: Field-Sourced-Content
   :sub-category: how-to
   :author: Michael Coleman

.. _sitecli-reference:

===========================================================
F5 Distributed Cloud (XC) Site CLI Referece
===========================================================

.. contents:: Table of Contents
   :depth: 3
   :local:

Overview
========

The F5 Distributed Cloud Site CLI (also known as SiteCLI or vpmd) is an interactive command-line interface for managing and configuring F5 XC Customer Edge (CE) nodes. The CLI provides a menu-driven interface for initial configuration, network diagnosis, system health monitoring, and administrative tasks.

**Key Components:**

- **SiteCLI:** Main interactive shell with top-level commands for configuration and management
- **ExecCLI:** Sandboxed command execution framework providing safe access to diagnostic and administrative tools

Access
======

Connecting to SiteCLI
---------------------

The SiteCLI is accessed via SSH using the ``admin`` user account:

.. code-block:: bash

   ssh admin@<node-ip-address>

**Credentials:**

- Username: ``admin``
- Password: Provided during node provisioning

**Welcome Screen:**

Upon successful authentication, the following information is displayed:

- OS Version (e.g., rhel-9.2025.39)
- Memory (Total available)
- Storage (Disk information)
- CPU (Model and core count)
- Software Version (e.g., crt-20250613-3382)
- DNS Status (Connectivity to DNS servers)
- NTP Status (Time synchronization status)
- Uptime (System uptime)
- Registration Status (e.g., PROVISIONED)
- SLO IP (Site Local Outside IP)
- Public IP (External IP address)

Navigation
----------

The CLI uses an interactive prompt-based interface:

- **Prompt:** ``>>>``
- **Tab Completion:** Press Tab to see available commands
- **Help:** Use ``--help`` flag with commands for detailed information
- **Exit:** Type ``exit`` to leave the CLI

Part 1: SiteCLI Top-Level Commands
===================================

Overview
--------

The SiteCLI provides 13 top-level commands for node configuration, monitoring, and management:

1. configure
2. configure-generic-hardware
3. configure-http-proxy
4. configure-network
5. diagnosis
6. health
7. clear
8. factory-reset
9. reboot
10. show
11. upgrade

Command Reference
=================

configure
---------

**Purpose:** Initial configuration of the node

**Description:**
Used for the initial setup and configuration of the F5 XC Customer Edge node. This command guides administrators through the registration and setup process.

**Usage:**

.. code-block:: text

   >>> configure
   >>> configure --help

**Options:**

- ``--help`` - Display help information for the configure command

**Related Functions:**

- Node registration
- Site configuration
- Cluster setup
- Certificate configuration

configure-generic-hardware
--------------------------

**Purpose:** Configure Hardware that isn't certified by F5

**Description:**
Allows configuration of hardware that is not officially certified by F5 Distributed Cloud. This command enables the use of custom or generic hardware platforms.

**Usage:**

.. code-block:: text

   >>> configure-generic-hardware
   >>> configure-generic-hardware --help

**Options:**

- ``--help`` - Display help information

**Prompts:**

- "Select certified hardware:" - Interactive hardware selection
- "Confirm lte configuration?" - LTE configuration confirmation
- "Node is already configured" - Warning if node has existing configuration

**Notes:**

- Requires PCI device information
- May require LTE/cellular configuration
- Used for non-standard hardware deployments

configure-http-proxy
--------------------

**Purpose:** Initial configuration of the HTTP proxy

**Description:**
Configures HTTP/HTTPS proxy settings for the node to communicate with F5 XC services through a corporate proxy server.

**Usage:**

.. code-block:: text

   >>> configure-http-proxy
   >>> configure-http-proxy --help

**Options:**

- ``--help`` - Display help information

**Configuration Parameters:**

- Proxy server address and port
- Proxy authentication (if required)
- No-proxy exceptions
- TLS/SSL settings

**Related Settings:**

- Registration proxy configuration
- Internet proxy settings
- Proxy bypass rules

configure-network
-----------------

**Purpose:** Initial configuration of the network

**Description:**
Configures network interfaces, IP addressing, routing, and connectivity for the Customer Edge node.

**Usage:**

.. code-block:: text

   >>> configure-network
   >>> configure-network --help

**Options:**

- ``--help`` - Display help information

**Configuration Areas:**

- Interface configuration (ens18, ens19, etc.)
- IP address assignment (static/DHCP)
- Default gateway
- DNS servers
- VLAN configuration
- Bonding/LAG configuration
- WiFi configuration (if applicable)

**Prompts:**

- "What is your SSID:" - WiFi network name
- Network interface selection
- IP addressing parameters

diagnosis
---------

**Purpose:** Network diagnosis of the node

**Description:**
Provides network diagnostic tools to troubleshoot connectivity and performance issues on the Customer Edge node.

**Usage:**

.. code-block:: text

   >>> diagnosis
   >>> diagnosis --help

**Options:**

- ``--help`` - Display help information

**Diagnostic Tools:**

The diagnosis command provides access to various network diagnostic utilities:

**Ping Operations:**

- Ping external hosts
- Ping volterra.io services
- Test connectivity to registration endpoints

**Traceroute:**

- ``execcli tracepath ves.io`` - Trace route to F5 XC services
- Network path analysis

**DNS Testing:**

- Domain resolution tests
- ``domain_curl_result`` - Test HTTPS connectivity to domains

**Connectivity Tests:**

- ``nodes_connectivity`` - Test connectivity between cluster nodes
- Host ping responses
- Network reachability tests

**Other Diagnostic Commands:**

- ``execcli netstat -a`` - Network statistics and connections
- ``execcli lsof`` - List open files and network connections
- ``calls lsof command on node`` - File descriptor information
- ``calls ping command on node`` - ICMP connectivity tests

health
------

**Purpose:** Status of the node

**Description:**
Displays comprehensive health and status information about the Customer Edge node, including system resources, services, and operational metrics.

**Usage:**

.. code-block:: text

   >>> health

**Displayed Information:**

**System Information:**

- OS version and kernel information
- CPU usage and load average
- Memory utilization
- Disk space and I/O statistics
- Network interface statistics
- System uptime

**Service Status:**

- VPM (VP Manager) service status
- Kubernetes components (kubelet, etc.)
- Docker/container runtime status
- Networking services
- Registration status

**Operational Metrics:**

- ``Status of services`` - Individual service health
- ``Status of the node`` - Overall node health
- Component readiness
- Registration and connectivity status

**Resource Utilization:**

- ``Memory Information`` - RAM usage details
- CPU metrics
- Storage utilization
- Network throughput

**Output Categories:**

- System metrics
- Service states
- Connectivity status
- Resource utilization
- Operational status

clear
-----

**Purpose:** Clear the terminal screen

**Description:**
Clears the CLI screen and resets the display.

**Usage:**

.. code-block:: text

   >>> clear

**Notes:**

- Simple command to clean up the display
- Does not affect system state or configuration

factory-reset
-------------

**Purpose:** Factory reset the node

**Description:**
Performs a factory reset of the Customer Edge node, removing all configuration and returning it to its initial state.

**Usage:**

.. code-block:: text

   >>> factory-reset
   >>> factory-reset --help

**Options:**

- ``--help`` - Display help information

**Prompts:**

- "Are you sure?" - Confirmation prompt before proceeding

**Actions Performed:**

- Remove site registration
- Delete local configuration
- Reset networking settings
- Clear certificates and keys
- Remove workload configuration
- Restore default settings

**Warnings:**

- ⚠️ This operation is **IRREVERSIBLE**
- This command is only supported on Bare Metal installs, all other systems will be unrecoverable.
- All node configuration will be lost
- Node will need to be re-registered
- Workloads will be removed

**Use Cases:**

- Decommissioning a node
- Preparing for re-deployment
- Recovering from misconfiguration
- Node reuse in different environment

reboot
------

**Purpose:** Reboot the node

**Description:**
Performs a system reboot of the Customer Edge node.

**Usage:**

.. code-block:: text

   >>> reboot
   >>> reboot --help

**Options:**

- ``--help`` - Display help information

**Notes:**

- Graceful system restart
- Services will be stopped cleanly
- Node will go offline during reboot
- Typically takes 2-5 minutes to complete

Advanced SiteCLI Functions
==========================

Debug Information Collection
-----------------------------

**Command:** ``collect-debug-info``

**Purpose:**
Collects comprehensive diagnostic information for troubleshooting and support purposes.

**Collected Data:**

- System logs (journalctl output)
- Service status
- Configuration files
- Network diagnostics
- Resource utilization
- Kernel logs
- Container logs

**Usage Context:**

- Troubleshooting issues
- Support ticket submission
- Pre-upgrade diagnostics
- Performance analysis

Kubelet Parameters
------------------

**Command:** ``kubelet-get-params``

**Purpose:**
Retrieves Kubernetes kubelet configuration and parameters.

**Information Provided:**

- Kubelet version
- Configuration parameters
- Resource allocations
- Node labels and taints
- Feature gates
- Runtime configuration

Part 2: ExecCLI Commands
=========================

Overview
--------

ExecCLI provides safe, sandboxed access to diagnostic and administrative commands for F5 XC Customer Edge nodes. All commands are accessed through the SiteCLI shell interface.

**Access Method:**

.. code-block:: bash

   # SSH to admin account
   ssh admin@<node-ip>

   # Run execcli commands
   # execcli <command> [arguments]

**Example Usage:**

.. code-block:: text

   execcli journalctl -u vpm -f

Complete ExecCLI Command Reference
===================================

Memory and Resource Commands
-----------------------------

check-mem
~~~~~~~~~

**Purpose:** Return memory statistics

**Description:**
Executes ``free`` and ``vmstat`` commands to display memory usage and virtual memory statistics.

**Usage:**

.. code-block:: bash

   execcli check-mem

**Output Includes:**

- Total, used, free memory
- Swap usage
- Buffer and cache information
- Virtual memory statistics

top
~~~

**Purpose:** Monitor real-time resource usage

**Description:**
Top command to check CPU, memory, and process resource usage in real-time.

**Usage:**

.. code-block:: bash

   execcli top

**Interactive Keys:**

- ``q`` - Quit
- ``M`` - Sort by memory
- ``P`` - Sort by CPU
- ``k`` - Kill process
- ``1`` - Toggle CPU cores

Time Synchronization
--------------------

chronyc-sources
~~~~~~~~~~~~~~~

**Purpose:** Check if NTP is synced

**Description:**
Displays chrony time synchronization sources and sync status.

**Usage:**

.. code-block:: bash

   execcli chronyc-sources

**Output Shows:**

- NTP server status
- Stratum levels
- Sync state
- Time offset

Container Management - CRI-O/containerd
---------------------------------------

crictl-images
~~~~~~~~~~~~~

**Purpose:** Check CRI-O/containerd images

**Description:**
Lists all container images managed by CRI-O/containerd runtime.

**Usage:**

.. code-block:: bash

   execcli crictl-images

**Output Fields:**

- Image ID
- Repository
- Tag
- Size

crictl-inspect
~~~~~~~~~~~~~~

**Purpose:** Inspect CRI-O/containerd container

**Description:**
Display detailed container information in JSON format.

**Usage:**

.. code-block:: bash

   execcli crictl-inspect <container-id>

**Output Includes:**

- Container configuration
- State information
- Network settings
- Mount points

crictl-logs
~~~~~~~~~~~

**Purpose:** Check CRI-O container logs

**Description:**
Retrieve and display container logs.

**Usage:**

.. code-block:: bash

   execcli crictl-logs <container-id>

crictl-ps
~~~~~~~~~

**Purpose:** List CRI-O containers

**Description:**
Lists running containers using CRI-O/containerd runtime.

**Usage:**

.. code-block:: bash

   execcli crictl-ps

crictl-ps-a
~~~~~~~~~~~

**Purpose:** List all CRI-O containers

**Description:**
Lists all containers (running and stopped) with ``-a`` option.

**Usage:**

.. code-block:: bash

   execcli crictl-ps-a

Container Management - Docker
-----------------------------

docker-images
~~~~~~~~~~~~~

**Purpose:** Check Docker images

**Description:**
Lists all Docker images on the system.

**Usage:**

.. code-block:: bash

   execcli docker-images

**Output Fields:**

- Repository
- Tag
- Image ID
- Created
- Size

docker-inspect
~~~~~~~~~~~~~~

**Purpose:** Inspect Docker container

**Description:**
Display detailed container information in JSON format.

**Usage:**

.. code-block:: bash

   execcli docker-inspect <container-id>

docker-logs
~~~~~~~~~~~

**Purpose:** Check Docker container logs

**Description:**
View container logs from Docker runtime.

**Usage:**

.. code-block:: bash

   execcli docker-logs <container-id>

docker-prune
~~~~~~~~~~~~

**Purpose:** Prune Docker unused objects

**Description:**
Remove all unused Docker objects (containers, networks, images, volumes).

**Usage:**

.. code-block:: bash

   execcli docker-prune

**Warning:** This removes ALL unused Docker objects. Use with caution.

docker-ps
~~~~~~~~~

**Purpose:** List Docker containers

**Description:**
Lists currently running Docker containers.

**Usage:**

.. code-block:: bash

   execcli docker-ps

docker-ps-a
~~~~~~~~~~~

**Purpose:** List all Docker containers

**Description:**
Lists all containers (running and stopped) with ``-a`` option.

**Usage:**

.. code-block:: bash

   execcli docker-ps-a

Network Diagnostics - HTTP/HTTPS
---------------------------------

curl-host
~~~~~~~~~

**Purpose:** Curl on host OS

**Description:**
Execute curl command from the host operating system context.

**Usage:**

.. code-block:: bash

   execcli curl-host <url> [options]

**Examples:**

.. code-block:: bash

   execcli curl-host https://register.ves.volterra.io
   execcli curl-host -I https://cloud.f5.com
   execcli curl-host --proxy http://proxy:8080 https://ves.io

curl-vega
~~~~~~~~~

**Purpose:** Curl in Kubernetes cluster

**Description:**
Execute curl from within the Kubernetes cluster/pod context.

**Usage:**

.. code-block:: bash

   execcli curl-vega <url> [options]

**Use Case:** Testing connectivity from within the K8s cluster network

Network Diagnostics - Basic
----------------------------

ping
~~~~

**Purpose:** Test ICMP connectivity

**Description:**
Calls ping command on node to test network reachability.

**Usage:**

.. code-block:: bash

   execcli ping <hostname-or-ip>

**Examples:**

.. code-block:: bash

   execcli ping ves.io
   execcli ping 8.8.8.8
   execcli ping register.ves.volterra.io

netstat
~~~~~~~

**Purpose:** Network statistics and connections

**Description:**
Netstat command on node to display network connections, routing tables, and interface statistics.

**Usage:**

.. code-block:: bash

   execcli netstat [options]

**Common Options:**

- ``-tulnp`` - TCP/UDP listening ports with PIDs
- ``-rn`` - Routing table
- ``-s`` - Protocol statistics
- ``-i`` - Interface statistics

lsof
~~~~

**Purpose:** List open files and network connections

**Description:**
Calls lsof command on node to list open files and sockets.

**Usage:**

.. code-block:: bash

   execcli lsof [options]

**Examples:**

.. code-block:: bash

   execcli lsof -i TCP:22      # SSH connections
   execcli lsof -i :443        # HTTPS connections
   execcli lsof -u root        # Files opened by root

Network Configuration
---------------------

ip
~~

**Purpose:** IP address and network management

**Description:**
Calls ip command for network configuration and status.

**Usage:**

.. code-block:: bash

   execcli ip <subcommand> [options]

**Common Subcommands:**

.. code-block:: bash

   execcli ip addr show        # IP addresses
   execcli ip route show       # Routing table
   execcli ip link show        # Network interfaces
   execcli ip neigh show       # ARP/ND cache

ip-link-set
~~~~~~~~~~~

**Purpose:** Bring network interface up or down

**Description:**
Configure network interface state and parameters.

**Usage:**

.. code-block:: bash

   execcli ip-link-set <interface> <up|down>
   execcli ip-link-set <interface> mtu <value>

**Examples:**

.. code-block:: bash

   execcli ip-link-set ens18 up
   execcli ip-link-set ens18 down
   execcli ip-link-set ens18 mtu 9000

ip-link-show
~~~~~~~~~~~~

**Purpose:** See link-layer information

**Description:**
Display link-layer information of all available network devices.

**Usage:**

.. code-block:: bash

   execcli ip-link-show
   execcli ip-link-show <interface>

nmcli
~~~~~

**Purpose:** Configure NetworkManager profile

**Description:**
NetworkManager command-line interface for network configuration.

**Usage:**

.. code-block:: bash

   execcli nmcli [command]

**Common Commands:**

.. code-block:: bash

   execcli nmcli connection show
   execcli nmcli device status
   execcli nmcli general hostname

BGP Routing Commands
--------------------

show-ip-bgp
~~~~~~~~~~~

**Purpose:** Show BGP with more detail

**Description:**
Display detailed BGP routing table information.

**Usage:**

.. code-block:: bash

   execcli show-ip-bgp

show-ip-bgp-summary
~~~~~~~~~~~~~~~~~~~

**Purpose:** Show BGP summary

**Description:**
Display BGP neighbor summary and status overview.

**Usage:**

.. code-block:: bash

   execcli show-ip-bgp-summary

**Output Includes:**

- Neighbor count
- AS numbers
- State/PfxRcd
- Uptime

show-ip-bgp-neighbors
~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Show BGP neighbor info

**Description:**
Display detailed information about BGP neighbors.

**Usage:**

.. code-block:: bash

   execcli show-ip-bgp-neighbors [neighbor-ip]

show-ip-bgp-neighbors-advertised-route
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Show advertised routes to BGP neighbors

**Description:**
Display routes being advertised to BGP neighbors.

**Usage:**

.. code-block:: bash

   execcli show-ip-bgp-neighbors-advertised-route [neighbor-ip]

IPsec VPN Commands
------------------

ipsec-status
~~~~~~~~~~~~

**Purpose:** Show IPsec status

**Description:**
Display IPsec tunnel status and statistics.

**Usage:**

.. code-block:: bash

   execcli ipsec-status

ipsec-statusall
~~~~~~~~~~~~~~~

**Purpose:** Show IPsec status for all targets

**Description:**
Display comprehensive IPsec status for all configured tunnels.

**Usage:**

.. code-block:: bash

   execcli ipsec-statusall

**Output Includes:**

- Tunnel state (up/down)
- Security associations
- Traffic statistics
- Peer information

Argo (Data Plane) Commands
---------------------------

dropstats
~~~~~~~~~

**Purpose:** Dump Argo drop statistics

**Description:**
Display packet drop statistics from the Argo data plane.

**Usage:**

.. code-block:: bash

   execcli dropstats

dropstats-non-zero
~~~~~~~~~~~~~~~~~~

**Purpose:** Dump Argo drop statistics (skip zero counters)

**Description:**
Display only non-zero packet drop statistics.

**Usage:**

.. code-block:: bash

   execcli dropstats-non-zero

**Use Case:** Quickly identify which drop counters have activity

flow-l
~~~~~~

**Purpose:** Dump Argo flow info

**Description:**
Display flow table information from Argo data plane.

**Usage:**

.. code-block:: bash

   execcli flow-l

flow-l-match
~~~~~~~~~~~~

**Purpose:** Dump Argo flow info with IP or IP:port pair

**Description:**
Display flow information matching specific IP address or IP:port combination.

**Usage:**

.. code-block:: bash

   execcli flow-l-match <ip>
   execcli flow-l-match <ip:port>

**Examples:**

.. code-block:: bash

   execcli flow-l-match 192.168.1.100
   execcli flow-l-match 192.168.1.100:443

mpls
~~~~

**Purpose:** Invoke Argo MPLS command

**Description:**
Display MPLS label information from Argo data plane.

**Usage:**

.. code-block:: bash

   execcli mpls

nh
~~

**Purpose:** Invoke Argo next-hop command

**Description:**
Display next-hop information from Argo data plane.

**Usage:**

.. code-block:: bash

   execcli nh

rt
~~

**Purpose:** Invoke Argo routing table command

**Description:**
Display routing table from Argo data plane.

**Usage:**

.. code-block:: bash

   execcli rt

vif
~~~

**Purpose:** Invoke Argo VIF command

**Description:**
Display virtual interface information from Argo data plane.

**Usage:**

.. code-block:: bash

   execcli vif

**Output Includes:**

- VIF ID
- Interface name
- Type
- Status
- Statistics

Packet Capture Commands
------------------------

vifdump
~~~~~~~

**Purpose:** Capture packets on specified VIF

**Description:**
Capture network packets on a specific virtual interface.

**Usage:**

.. code-block:: bash

   execcli vifdump <vif-id>

**Output:** Creates .pcap file in /tmp

vifdump-d
~~~~~~~~~

**Purpose:** Capture dropped packets

**Description:**
Capture dropped packets on specified VIF ID or all VIFs.

**Usage:**

.. code-block:: bash

   execcli vifdump-d <vif-id>
   execcli vifdump-d all

**Use Case:** Troubleshooting packet drops

vifdump-file-cp
~~~~~~~~~~~~~~~

**Purpose:** Copy vifdump files from Argo container

**Description:**
Executes: ``docker cp $(argo):/tmp/. /tmp/vifdump/``

**Usage:**

.. code-block:: bash

   execcli vifdump-file-cp

**Result:** Copies packet capture files to host /tmp/vifdump/

vifdump-file-rm
~~~~~~~~~~~~~~~

**Purpose:** Remove Argo /tmp/*.pcap files

**Description:**
Removes packet capture files from Argo container /tmp directory.

**Usage:**

.. code-block:: bash

   execcli vifdump-file-rm

vifdump-stop
~~~~~~~~~~~~

**Purpose:** Stop vifdump command

**Description:**
Stop vifdump if previous run abnormally ended.

**Usage:**

.. code-block:: bash

   execcli vifdump-stop

**Use Case:** Clean up hung packet capture processes

Envoy Proxy Commands
--------------------

envoy-clusters
~~~~~~~~~~~~~~

**Purpose:** Show installed Envoy clusters

**Description:**
Display Envoy cluster configuration and statistics.

**Usage:**

.. code-block:: bash

   execcli envoy-clusters

**Output Includes:**

- Cluster names
- Endpoints
- Health status
- Statistics

envoy-config-dump
~~~~~~~~~~~~~~~~~

**Purpose:** Show Envoy config-dump

**Description:**
Dump complete Envoy proxy configuration in JSON format.

**Usage:**

.. code-block:: bash

   execcli envoy-config-dump

**Output:** Full Envoy configuration (can be large)

envoy-hc-config-dump
~~~~~~~~~~~~~~~~~~~~

**Purpose:** Show Envoy healthcheck config-dump

**Description:**
Display Envoy health check configuration.

**Usage:**

.. code-block:: bash

   execcli envoy-hc-config-dump

envoy-listeners
~~~~~~~~~~~~~~~

**Purpose:** Show installed Envoy listeners

**Description:**
Display Envoy listener configuration and statistics.

**Usage:**

.. code-block:: bash

   execcli envoy-listeners

**Output Includes:**

- Listener addresses
- Port numbers
- Filter chains
- Statistics

Etcd Commands
-------------

etcdctl-cluster-member-status
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Check etcd member cluster status

**Description:**
From etcd pod on this node, check etcd member cluster status using: ``etcdctl -w table member status --cluster``

**Usage:**

.. code-block:: bash

   execcli etcdctl-cluster-member-status

**Output Includes:**

- Member IDs
- Leader status
- Raft term
- Index
- DB size

Vega Control Plane Commands
----------------------------

vegactl-configuration-list
~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** List Vega configurations

**Description:**
Display Vega control plane configuration list.

**Usage:**

.. code-block:: bash

   execcli vegactl-configuration-list

vegactl-introspect-dump-table
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Dump Vega internal tables

**Description:**
Display Vega introspection table data.

**Usage:**

.. code-block:: bash

   execcli vegactl-introspect-dump-table <table-name>

vegactl-introspect-get
~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Get Vega introspection data

**Description:**
Retrieve specific Vega introspection information.

**Usage:**

.. code-block:: bash

   execcli vegactl-introspect-get <object>

vegactl-introspect-list-tracebuffers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** List Vega trace buffers

**Description:**
Display available Vega trace buffers for debugging.

**Usage:**

.. code-block:: bash

   execcli vegactl-introspect-list-tracebuffers

vegactl-introspect-show-election
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Check Vega cluster primary election status

**Description:**
Display Vega control plane leader election information.

**Usage:**

.. code-block:: bash

   execcli vegactl-introspect-show-election

**Output Shows:**

- Current leader
- Election term
- Voter status

vegactl-introspect-show-tracebuffer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Show Vega trace buffer

**Description:**
Display contents of specific Vega trace buffer.

**Usage:**

.. code-block:: bash

   execcli vegactl-introspect-show-tracebuffer <buffer-name>

System Logging
--------------

journalctl
~~~~~~~~~~

**Purpose:** Check system logs

**Description:**
Query systemd journal logs for system services and events.

**Usage:**

.. code-block:: bash

   execcli journalctl [options]

**Common Options:**

- ``-u <unit>`` - Logs for specific service
- ``-f`` - Follow logs (tail -f style)
- ``-n <num>`` - Show last N lines
- ``-xe`` - Recent logs with details
- ``--since "1 hour ago"`` - Time-based filtering

**Examples:**

.. code-block:: bash

   execcli journalctl -u vpm -f
   execcli journalctl -u kubelet -n 100
   execcli journalctl --since "1 hour ago"
   execcli journalctl -p err

File Operations
---------------

files
~~~~~

**Purpose:** Perform file operations on node

**Description:**
File operations command - saving to file output is allowed but only under /tmp directory.

**Usage:**

.. code-block:: bash

   execcli files <command>

**Restriction:** Output can only be saved to /tmp directory for security

Kubelet Configuration
---------------------

kubelet-add-param
~~~~~~~~~~~~~~~~~

**Purpose:** Add custom parameter to Kubelet

**Description:**
Add a custom configuration parameter to the Kubelet service.

**Usage:**

.. code-block:: bash

   execcli kubelet-add-param <parameter> <value>

**Warning:** Only add parameters recommended by F5 XC support

kubelet-get-params
~~~~~~~~~~~~~~~~~~

**Purpose:** Get custom parameters from Stage

**Description:**
Retrieve current Kubelet custom parameters and configuration.

**Usage:**

.. code-block:: bash

   execcli kubelet-get-params

**Output Includes:**

- Custom parameters
- Kubelet version
- Feature gates
- Resource reservations

kubelet-remove-param
~~~~~~~~~~~~~~~~~~~~

**Purpose:** Remove custom parameters from file

**Description:**
Remove a custom configuration parameter from Kubelet.

**Usage:**

.. code-block:: bash

   execcli kubelet-remove-param <parameter>

System Service Management
-------------------------

systemctl-status-vpm
~~~~~~~~~~~~~~~~~~~~

**Purpose:** Check VPM service status

**Description:**
Display VPM (VP Manager) service status.

**Usage:**

.. code-block:: bash

   execcli systemctl-status-vpm

systemctl-restart-vpm
~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Restart VPM service

**Description:**
Restart the VPM service.

**Usage:**

.. code-block:: bash

   execcli systemctl-restart-vpm

**Warning:** This will cause brief service interruption

systemctl-status-kubelet
~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Check Kubelet service status

**Description:**
Display Kubelet service status.

**Usage:**

.. code-block:: bash

   execcli systemctl-status-kubelet

systemctl-restart-kubelet
~~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Restart Kubelet service

**Description:**
Restart the Kubelet service.

**Usage:**

.. code-block:: bash

   execcli systemctl-restart-kubelet

**Warning:** May cause pod disruptions

systemctl-status-docker
~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Check Docker service status

**Description:**
Display Docker daemon service status.

**Usage:**

.. code-block:: bash

   execcli systemctl-status-docker

systemctl-restart-docker
~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Restart Docker service

**Description:**
Restart the Docker daemon.

**Usage:**

.. code-block:: bash

   execcli systemctl-restart-docker

**Warning:** Will restart all Docker containers

systemctl-status-crio
~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Check CRI-O service status

**Description:**
Display CRI-O container runtime service status.

**Usage:**

.. code-block:: bash

   execcli systemctl-status-crio

systemctl-restart-crio
~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Restart CRI-O service

**Description:**
Restart the CRI-O container runtime.

**Usage:**

.. code-block:: bash

   execcli systemctl-restart-crio

**Warning:** Will restart all CRI-O containers

systemctl-status-iscsid
~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Check iSCSI daemon service status

**Description:**
Display iSCSI initiator daemon status.

**Usage:**

.. code-block:: bash

   execcli systemctl-status-iscsid

systemctl-restart-iscsid
~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Restart iSCSI daemon service

**Description:**
Restart the iSCSI initiator daemon.

**Usage:**

.. code-block:: bash

   execcli systemctl-restart-iscsid

**Use Case:** After iSCSI configuration changes

systemctl-status-multipathd
~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Check multipathd service status

**Description:**
Display device-mapper multipath daemon status.

**Usage:**

.. code-block:: bash

   execcli systemctl-status-multipathd

systemctl-restart-multipathd
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Restart multipathd service

**Description:**
Restart the multipath daemon for storage path management.

**Usage:**

.. code-block:: bash

   execcli systemctl-restart-multipathd

System Configuration
--------------------

sysctl
~~~~~~

**Purpose:** Kernel parameter management

**Description:**
Calls sysctl command on node to read or modify kernel parameters.

**Usage:**

.. code-block:: bash

   execcli sysctl <parameter>
   execcli sysctl -a

**Examples:**

.. code-block:: bash

   execcli sysctl net.ipv4.ip_forward
   execcli sysctl kernel.hostname
   execcli sysctl -a | grep net

load-sysctl-conf
~~~~~~~~~~~~~~~~

**Purpose:** Load sysctl.conf

**Description:**
Reload sysctl configuration from /etc/sysctl.conf.

**Usage:**

.. code-block:: bash

   execcli load-sysctl-conf

**When to Use:** After editing sysctl.conf

System Package Management
-------------------------

rpm-ostree
~~~~~~~~~~

**Purpose:** OS package management

**Description:**
Calls rpm-ostree command on node for OS package and deployment management.

**Usage:**

.. code-block:: bash

   execcli rpm-ostree <command>

**Common Commands:**

.. code-block:: bash

   execcli rpm-ostree status        # Show deployments
   execcli rpm-ostree upgrade       # Upgrade OS
   execcli rpm-ostree rollback      # Roll back

Advanced Configuration - Use with Caution
------------------------------------------

edit-app-env-file
~~~~~~~~~~~~~~~~~

**Purpose:** Edit /etc/vpm/app_env.yaml

**Description:**
Edit local app environment configuration file.

**Usage:**

.. code-block:: bash

   execcli edit-app-env-file

**WARNING:** Please do not use this unless F5 XC support requested

edit-azure-client-id-secret
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Update Azure client ID and secret

**Description:**
Update Azure client ID and secret for this CE when it has expired.

**Usage:**

.. code-block:: bash

   execcli edit-azure-client-id-secret

**Use Case:** Azure authentication credential renewal

edit-certified-hardware
~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Edit certified hardware config

**Description:**
Modify certified hardware configuration.

**Usage:**

.. code-block:: bash

   execcli edit-certified-hardware

**WARNING:** Please do not use this unless F5 XC support requested

edit-etc-hosts
~~~~~~~~~~~~~~

**Purpose:** Edit /etc/hosts file

**Description:**
Modify the system hosts file for name resolution.

**Usage:**

.. code-block:: bash

   execcli edit-etc-hosts

**Use Case:** Adding custom hostname mappings

edit-sysctl-conf
~~~~~~~~~~~~~~~~

**Purpose:** Edit sysctl.conf

**Description:**
Modify kernel parameter configuration file.

**Usage:**

.. code-block:: bash

   execcli edit-sysctl-conf

**Note:** Use ``load-sysctl-conf`` after editing to apply changes

edit-udev-10-nic-name
~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Edit NIC name udev rules

**Description:**
Edit /etc/udev/rules.d/10-nic-names.rules to update network interface names.

**Usage:**

.. code-block:: bash

   execcli edit-udev-10-nic-name

**Use Case:** Persistent network interface naming

Root Access Management
----------------------

get-auxilary-root-access-to-node
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Create auxiliary root user

**Description:**
Creates a root user named ``xuser`` for emergency access.

**Usage:**

.. code-block:: bash

   execcli get-auxilary-root-access-to-node

**Security Note:** Only use when instructed by F5 XC support

remove-auxilary-root-access-to-node
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Purpose:** Delete auxiliary root user

**Description:**
Deletes the root user ``xuser`` when no longer needed.

**Usage:**

.. code-block:: bash

   execcli remove-auxilary-root-access-to-node

**Best Practice:** Remove auxiliary access after troubleshooting is complete

Usage Examples and Workflows
=============================

Complete Network Troubleshooting
---------------------------------

.. code-block:: bash

   # Step 1: Check interface status
   execcli ip-link-show
   execcli ip addr show

   # Step 2: Test basic connectivity
   execcli ping ves.io
   execcli ping 8.8.8.8

   # Step 3: Check routing
   execcli ip route show

   # Step 4: Check active connections
   execcli netstat -tulnp

   # Step 5: Test external connectivity
   execcli curl-host https://register.ves.volterra.io

VPM Service Troubleshooting
----------------------------

.. code-block:: bash

   # Check VPM service status
   execcli systemctl-status-vpm

   # View recent VPM logs
   execcli journalctl -u vpm -n 100

   # Follow VPM logs in real-time
   execcli journalctl -u vpm -f

   # Check VPM container
   execcli docker-ps | grep vpm
   execcli docker-logs vpm

   # Restart VPM if needed
   execcli systemctl-restart-vpm

BGP Routing Diagnostics
------------------------

.. code-block:: bash

   # BGP overview
   execcli show-ip-bgp-summary

   # Detailed BGP table
   execcli show-ip-bgp

   # BGP neighbor info
   execcli show-ip-bgp-neighbors

   # Advertised routes
   execcli show-ip-bgp-neighbors-advertised-route

IPsec Tunnel Troubleshooting
-----------------------------

.. code-block:: bash

   # Check all IPsec tunnels
   execcli ipsec-statusall

   # Check specific tunnel
   execcli ipsec-status

   # Check connectivity to peer
   execcli ping <peer-ip>

   # View IPsec logs
   execcli journalctl -u strongswan

Data Plane (Argo) Troubleshooting
----------------------------------

.. code-block:: bash

   # Check VIFs (virtual interfaces)
   execcli vif

   # Check flows
   execcli flow-l

   # Find specific flow
   execcli flow-l-match 192.168.1.100

   # Check drop statistics
   execcli dropstats-non-zero

   # Check routing table
   execcli rt

   # Check next-hops
   execcli nh

   # Capture packets on VIF
   execcli vifdump <vif-id>

   # Stop capture
   execcli vifdump-stop

   # Copy capture files
   execcli vifdump-file-cp

Envoy Proxy Debugging
----------------------

.. code-block:: bash

   # Check Envoy listeners
   execcli envoy-listeners

   # Check Envoy clusters
   execcli envoy-clusters

   # Full config dump
   execcli envoy-config-dump

   # Health check config
   execcli envoy-hc-config-dump

Performance Analysis
--------------------

.. code-block:: bash

   # System resources
   execcli top
   execcli check-mem

   # Network statistics
   execcli netstat -s
   execcli ip -s link

   # Container resource usage
   execcli docker-ps
   execcli docker-inspect <container-id>

Control Plane (Vega) Debugging
-------------------------------

.. code-block:: bash

   # Check leader election
   execcli vegactl-introspect-show-election

   # List configurations
   execcli vegactl-configuration-list

   # Check trace buffers
   execcli vegactl-introspect-list-tracebuffers

   # Show specific trace
   execcli vegactl-introspect-show-tracebuffer <name>

   # Dump internal table
   execcli vegactl-introspect-dump-table <table-name>

Etcd Cluster Health Check
--------------------------

.. code-block:: bash

   # Check etcd cluster status
   execcli etcdctl-cluster-member-status

   # View etcd logs
   execcli journalctl -u etcd

Time Sync Verification
----------------------

.. code-block:: bash

   # Check NTP sync
   execcli chronyc-sources

   # View time sync logs
   execcli journalctl -u chronyd

System Information
==================

Registration Status
-------------------

The node can be in various registration states:

**PROVISIONED:**
Node is fully registered and operational with F5 XC services.

**WAITING_FOR_CONFIG:**
Node is registered but waiting for configuration from the controller.

**Other States:**
- Not Registered
- Registration Pending
- Configuration Error

Network Configuration
---------------------

**Site Local Inside (SLI):**
Internal network interface for node-to-node communication.

**Site Local Outside (SLO):**
External network interface for internet/WAN connectivity.

**Management Interface:**
Dedicated management network (optional).

Software Components
-------------------

**VPM (VP Manager):**
Core service managing node operations and F5 XC integration.

**Container Runtime:**
Docker or containerd for workload execution.

**Kubernetes:**
K8s components for container orchestration.

**Networking:**
- Calico/Flannel for pod networking
- iptables/nftables for firewall rules
- CoreDNS for service discovery

**Argo:**
High-performance data plane for packet forwarding.

**Vega:**
Control plane for configuration and orchestration.

**Envoy:**
Layer 7 proxy for application services.

Logs and Troubleshooting
=========================

Log Locations
-------------

**System Logs:**

.. code-block:: bash

   # VPM service logs
   sudo journalctl -u vpm.service

   # Kubelet logs
   sudo journalctl -u kubelet.service

   # System logs
   sudo journalctl -xe

**Configuration Files:**

- ``/etc/vpm/`` - VPM configuration
- ``/etc/default/vpm`` - VPM environment variables
- ``/etc/kubernetes/`` - Kubernetes configuration
- ``/etc/systemd/system/`` - Service definitions

Common Issues
-------------

**Registration Failures:**

- Check proxy configuration
- Verify network connectivity
- Confirm registration token validity
- Review ``/etc/vpm/config.yaml``

**Service Failures:**

- Use ``health`` command to identify failed services
- Check service logs with journalctl
- Verify resource availability
- Review system errors in logs

**Network Issues:**

- Use ``diagnosis`` command for network tests
- Check interface configuration
- Verify routing tables
- Test DNS resolution
- Confirm firewall rules

Security Considerations
=======================

Access Control
--------------

- Admin password should be changed after initial setup
- SSH key-based authentication recommended
- Regular password rotation policy
- Audit access logs regularly

ExecCLI Security
----------------

- All execcli commands are input-sanitized
- File operations restricted to /tmp directory
- Auxiliary root access should be removed after use
- Configuration edits should only be done when requested by F5 XC support
- Service restarts may cause temporary disruptions

Certificate Management
----------------------

- Certificates are managed automatically by VPM
- Certificate renewal handled by F5 XC
- Monitor certificate expiry dates
- Verify certificate trust chain

Best Practices
==============

Configuration Management
------------------------

1. **Initial Setup:**
   - Complete ``configure`` wizard fully
   - Document all configuration choices
   - Verify connectivity before proceeding

2. **Network Configuration:**
   - Plan IP addressing carefully
   - Document gateway and DNS settings
   - Test connectivity at each step
   - Configure redundant network paths where possible

3. **Monitoring:**
   - Regularly check ``health`` status
   - Monitor resource utilization
   - Review logs for errors
   - Set up alerts for critical issues

4. **Maintenance:**
   - Apply software updates during maintenance windows
   - Test upgrades in non-production first
   - Maintain configuration backups
   - Document all changes

5. **Troubleshooting:**
   - Use ``diagnosis`` for network issues
   - Collect debug information before contacting support
   - Check service status with ``health``
   - Review recent logs for clues

ExecCLI Best Practices
----------------------

1. **Always Check Status Before Restarting Services**

   .. code-block:: bash

      execcli systemctl-status-vpm
      # Review status before:
      execcli systemctl-restart-vpm

2. **Use Non-Zero Drop Stats for Efficiency**

   .. code-block:: bash

      # Faster than full dropstats
      execcli dropstats-non-zero

3. **Follow Logs for Real-Time Monitoring**

   .. code-block:: bash

      execcli journalctl -u vpm -f

4. **Stop Packet Captures When Done**

   .. code-block:: bash

      execcli vifdump <vif-id>
      # ... analysis ...
      execcli vifdump-stop
      execcli vifdump-file-cp
      execcli vifdump-file-rm

5. **Use Specific BGP Commands for Targeted Info**

   .. code-block:: bash

      # Summary first
      execcli show-ip-bgp-summary
      # Then details if needed
      execcli show-ip-bgp-neighbors

Appendix
========

Getting Help
------------

**SiteCLI Help:**

.. code-block:: text

   >>> <command> --help

**ExecCLI Help:**

.. code-block:: bash

   execcli --help
   execcli <command> --help

Related Documentation
---------------------

- F5 Distributed Cloud Documentation: https://docs.cloud.f5.com/
- Customer Edge Deployment Guides
- Networking Architecture Documents
- API Reference Documentation

Glossary
========

**CE (Customer Edge):**
F5 Distributed Cloud node deployed in customer environment.

**RE (Regional Edge):**
F5-managed edge location providing global connectivity.

**SiteCLI:**
Site Command Line Interface for node management.

**ExecCLI:**
Sandboxed command execution framework for diagnostics.

**VPM (VP Manager):**
Core management service on Customer Edge nodes.

**vk8s (Virtual Kubernetes):**
F5 XC's distributed Kubernetes implementation.

**SLI (Site Local Inside):**
Internal-facing network interface.

**SLO (Site Local Outside):**
External-facing network interface.

Notes
=====

- This documentation is based on SiteCLI version crt-20250613-3382
- Command availability may vary by software version
- Some advanced features may require specific licenses or configurations
- Always refer to official F5 documentation for the most current information
- ExecCLI commands are input-sanitized and run in a sandboxed environment

Contact and Support
===================

For additional assistance:

- F5 Distributed Cloud Support Portal
- Technical Account Manager (TAM)
- Community Forums
- Official Documentation Site

---