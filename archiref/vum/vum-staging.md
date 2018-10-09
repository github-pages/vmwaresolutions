---

copyright:

  years:  2016, 2018

lastupdated: "2018-10-03"

---

#	Staging and remediation

Patches and extensions can be optionally staged before remediation to ensure that they are downloaded from VUM to the vSphere ESXi host without applying the patches or extensions immediately. During remediation, VUM applies the patches, extensions, and upgrades to the inventory objects. Staging patches and extensions accelerates the remediation process because the patches and extensions are already available locally on the hosts.

If during the remediation process any host fails to enter maintenance mode, VUM will report an error and the remediation process stops and fails. The vSphere ESXi hosts that have already been remediated remain at the updated level.

For the remediation process to work smoothly, it is advisable to disable some of the cluster features such as DPM, HA Admission Control, Fault Tolerance  as well as disconnecting any removable devices from the virtual machines so that DRS should not face any issues while migrating VMs to other hosts.
Also, while running the remediation wizard, you can Generate Reports to verify that there are no inconsistent settings present at host/VM level that can cause the remediation process to fail.

1.	Using the vSphere Web Client, select **Home** > **Hosts and Clusters**.
2.	Right-click a datacenter, cluster, or a host, and click **Stage Patches**.
3.	On the Baseline Selection page of the Stage wizard, select the patch and extension baselines to stage.
4.	Select the hosts where patches and extensions will be applied and click **Next**. If you selected to stage patches and extensions to a single host, it is selected by default.
5.	Optionally, deselect the patches and extensions to exclude from the stage operation. To search within the list of patches and extensions, enter text in the text box in the Filter box. Click **Next**.
6.	Review the Ready to Complete page and click **Finish**.

The number of the staged patches and extensions for the specific host is displayed in the Patches and Extensions columns in the bottom pane of the Update Manager tab. After a remediation is successfully completed, all staged patches and extensions, whether installed or not during the remediation, will be deleted from the host.
Remediation is the process in which VUM applies patches, extensions, and upgrades to vSphere ESXi hosts, virtual machines, or virtual appliances after a scan is complete and makes the selected objects compliant. You can remediate single hosts, virtual machines, or virtual appliances or at a folder, cluster, or a data center level. VUM supports remediation for the following inventory objects by using either manual remediation or scheduled remediation:
*	ESXi hosts for patch, extension, and upgrade remediation.
*	Powered on, suspended, or powered off virtual machines and templates for VMware Tools and virtual machine hardware upgrade.
*	Powered on virtual appliances that are created with VMware Studio 2.0 and later, for virtual appliance upgrade.

If the update requires it, hosts are put into maintenance mode before remediation. The VCSA migrates the virtual machines to other hosts within VCS instance before the host is put in maintenance mode.

  Important note for hosts in a vSAN Cluster
  Be aware of the following behavior for hosts that are part of a vSAN cluster:
  *	The host remediation process might take an extensive amount of time to complete.
  *	By design, only one host from a VSAN cluster can be in a maintenance mode at any time.
  *	VUM remediates hosts that are part of a VSAN cluster sequentially even if you set the option to remediate the hosts in parallel.
  *	Any virtual machine on the host that uses a VM storage policy with a setting for "Number of failures to tolerate=0", the host might experience unusual delays when entering maintenance mode. The delay occurs because vSAN must migrate the virtual machine data from one disk to another in the vSAN datastore cluster and this can take many hours. You can work around this by setting the "Number of failures to tolerate=1" for the VM storage policy, which results in creating two copies of the virtual machine files in the vSAN datastore.
  *	Any virtual machine on the host that uses a VM storage policy with a setting for "Number of failures to tolerate=1", then the VM will become non-redundant when the host enters maintenance mode. If this is not acceptable, see [virtual machine vSAN redundancy](vum-vsan-redundancy.html).

To remediate hosts and clusters, follow these steps:
1.	Use the vSphere Web Client, select **Home** > **Hosts and Clusters**.
2.	From the inventory object navigator, select a data center, a cluster, or a host, click the **Update Manager** tab and click **Remediate**. If you selected a container object, all hosts under the selected object are remediated. The Remediate wizard opens.
3.	Select **Patch Baselines** or **Extension Baselines** depending on what type of update you want to perform on the host. On the Select baseline page of the Remediate wizard, select the **baseline group** and **baselines** to apply.
4.	Select the target hosts that you want to remediate and click **Next**. If you have chosen to remediate a single host and not a container object, the host is selected by default.
5.	Optionally, on the **Patches and Extensions** page, deselect specific patches or extensions to exclude them from the remediation process, and click **Next**.
6.	Optionally, on the **Advanced options** page, select the option to schedule the remediation to run later and specify a unique name and an optional description for the task. The time that you set for the scheduled task is the time on the VCSA. Optionally, select the option to ignore warnings about unsupported devices on the host, or no longer supported VMFS datastore to continue with the remediation. Click **Next**.
7.	On the Host **Remediation Options** page, from the **VM Power state** drop-down menu, you can select the change in the power state of the virtual machines and virtual appliances that are running on the hosts to be remediated. A host cannot enter maintenance mode until virtual machines on the host are powered off, suspended, or migrated with vMotion to other hosts in a DRS cluster. Some updates require that a host enters maintenance mode before remediation. Virtual machines and appliances cannot run when a host is in maintenance mode. To reduce the host remediation downtime at the expense of virtual machine availability, you can choose to shut down or suspend virtual machines and virtual appliances before remediation. In a DRS cluster, if you do not power off the virtual machines, the remediation takes longer but the virtual machines are available during the entire remediation process because they are migrated with vMotion to other hosts. The selections are as follows:

  - **Power Off virtual machines** - Power off all virtual machines and virtual appliances before remediation.
  - **Suspend virtual machines** - Suspend all running virtual machines and virtual appliances before remediation.
  - **Do Not Change VM Power State** - Leave virtual machines and virtual appliances in their current power state.

8.	Optionally, select **Disable any removable media devices connected to the virtual machine on the host**. VUM does not remediate hosts on which virtual machines have connected CD, DVD, or diskette drives. In cluster environments, connected media devices might prevent vMotion if the destination host does not have an identical device or mounted ISO image, which in turn prevents the source host from entering maintenance mode. After remediation, VUM reconnects the removable media devices if they are still available.
9.	Optionally, select **Retry entering maintenance mode in case of failure**, specify the number of retries, and specify the time to wait between retries. VUM waits for the retry delay period and retries putting the host into maintenance mode as many times as you indicate in Number of retries field.
10.	There is no requirement in a VCS instance to select the check box under ESXi Patch Settings to enable Update Manager to patch powered on PXE booted ESXi hosts.
11.	Click **Next**.
12.	If you remediate hosts in a cluster, edit the cluster remediation options. The **Cluster remediation options** page is available only when you remediate clusters. The following options can be selected:

*	**Disable Distributed Power Management (DPM)** if it is enabled for any of the selected clusters – VUM does not remediate clusters with active DPM. DPM monitors the resource use of the running virtual machines in the cluster. If sufficient excess capacity exists, DPM recommends moving virtual machines to other hosts in the cluster and placing the original host into standby mode to conserve power. Putting hosts into standby mode might interrupt remediation.
*	**Disable High Availability admission control** if it is enabled for any of the selected clusters - VUM does not remediate clusters with active HA admission control. Admission control is a policy that is used by VMware HA to ensure failover capacity within a cluster. If HA admission control is enabled during remediation, the virtual machines within a cluster might not migrate with vMotion.
*	**Disable Fault Tolerance (FT)** if it is enabled. This affects all fault tolerant virtual machines in the selected clusters - If FT is turned on for any of the virtual machines on a host, VUM does not remediate that host. For FT to be enabled, the hosts on which the Primary and Secondary virtual machines run must be of the same version and must have the same patches installed. If you apply different patches to these hosts, FT cannot be reenabled.
*	**Enable parallel remediation for the hosts in the selected clusters** - Remediate hosts in clusters in a parallel manner. If the setting is not selected, VUM remediates the hosts in a cluster sequentially. You can select one of the following options for parallel remediation:
    - You can let VUM continuously evaluate the maximum number of hosts it can remediate concurrently without disrupting DRS settings.
    - You can specify a limit of the number of concurrently remediated hosts in each cluster you remediate. Note, VUM remediates concurrently only the hosts on which virtual machines are powered off or suspended. You can choose to power off or suspend virtual machines from the VM Power State menu in the Maintenance Mode Options pane on the Host Remediation Options page. By design only one host from a vSAN cluster can be in a maintenance mode at any time. VUM remediates hosts that are part of a vSAN cluster sequentially even if you select the option to remediate them in parallel.
*	**Migrate powered off and suspended virtual machines to other hosts in the cluster**, if a host must enter maintenance mode. Update Manager migrates the suspended and powered off virtual machines from hosts that must enter maintenance mode to other hosts in the cluster. You can choose to power off or suspend virtual machines before remediation in the **Maintenance Mode Settings** pane.
13.	On the Ready to complete page, you can optionally click **Pre-check Remediation** to generate a cluster remediation options report and click **OK**. A Cluster Remediation Options Report dialog box opens. You can export this report or copy the entries for your own record and click **Next**.
14.	Review the Ready to Complete page and click **Finish**.

### Related links

* [VMware HCX on IBM Cloud Solution](https://www.ibm.com/cloud/garage/files/HCX_Architecture_Design.pdf)
* [VMware Solutions on IBM Cloud Digital Technical Engagement](https://ibm-dte.mybluemix.net/ibm-vmware) (Demos)