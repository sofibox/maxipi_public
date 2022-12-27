MaxiPI is a bash script used for managing API from various providers such as linode, Digitalocean, Microsoft Azure,
CloudFlare, AbuseIPDB and etc

For example, we can use this script to:

1) Manage DNS records through API,
2) Shutdown remote server through API,
3) Manage AbuseIPDB blocklist through API,
4) Manage Firewall API and many more


List of supported APIs:

1) linode (beta)
2) directadmin (under progress)
2) digitalocean (under development)
3) Microsoft Azure (under development)
4) cloudflare (under development)
5) abuseIPDB (under development)

When you first run the program maxipi <provider>, the script will check for existing token

In the case of linode provider, first it will read token key from maxipi.conf. If the file does not have token (or empty),
it will try to read it from ~/.config/linode-cli. If it did not find any of the above, it will prompt to manually put token key like below:



```
root@MAXIWORK:~/.config# maxipi linode --help
[maxipi->linode_api->input]: Warning, the variable LINODE_DNS_API_KEY for linode token has no value! Enter a new linode API token:
abcdefghioajkl()kls*************
[maxipi->linode_api]: Given token is: abcdefghioajkl()kls*********
[maxipi->linode_api->input]: Do you want to write the token into config file at /opt/maxipi/maxipi.conf? [default:Y] [Y/n]: y
[maxipi->status_handler->status]: [ Success ]

[maxipi->linode_api]: Testing the linode account using the new token ...

Success response:
{
  "company": "Sofibox Cloud",
  "email": "webmaster@sofibox.com",
  "first_name": "Arafat",
  "last_name": "Ali",
  "address_1": "****, ******",
  "address_2": "******",
  "city": "****",
  "state": "****",
  "zip": "****",
  "country": "********",
  "phone": "*************",
  "balance": *,
  "tax_id": "****************",
  "billing_source": "linode",
  "credit_card": {
    "last_four": "*****",
    "expiry": "*****"
  },
  "balance_uninvoiced": ********,
  "active_since": "*************",
  "capabilities": [
    "Linodes",
    "NodeBalancers",
    "Block Storage",
    "Object Storage",
    "Kubernetes",
    "Cloud Firewall",
    "Vlans",
    "LKE HA Control Planes",
    "Machine Images",
    "Managed Databases"
  ],
  "active_promotions": [],
  "euuid": "**************************"
}

[maxipi->status_handler->http]: [ Success ]

```
**Usage**

Only linode is supported at this moment

```
# -v | -V | --version | version | ver
root@MAXIWORK:~# maxipi -V
=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=
MaxiAPI is a bash script used for executing API from various providers
such as linode, Digitalocean, Microsoft Azure, CloudFlare and etc

1.1-alpha
Author: Arafat Ali | Email: arafat@sofibox.com | (C) 2019-2022
=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=
```

```
# -h | --help | help | /? | ?
root@MAXIWORK:~# maxipi -h
This is a readme file
```

```
# show-script-path | get-script-path | script-path
root@MAXIWORK:~# maxipi show-script-path
/opt/maxipi
```

```
# 1) How to get linode server status?
# similar actions: get-linode-status | get-status | get-server-status
root@MAXIWORK:~# maxipi linode get-linode-status --server-label sun.sofibox.com
[maxipi->linode_api]: Getting linode status for --linode-label sun.sofibox.com ...
[maxipi->linode_api]: Linode for --linode-label sun.sofibox.com is rebooting ...

or pass scripting to hide other outputs

root@MAXIWORK:~# maxipi linode get-linode-status --linode-label sun.sofibox.com --scripting
running

2) How about waiting for the status change with --until-linode-status <status> option
root@MAXIWORK:~# maxipi linode get-linode-status --server-label sun.sofibox.com --wait-until-linode-status offline
[maxipi->linode_api]: Getting linode status for --linode-label sun.sofibox.com ...
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is rebooting. Waiting for offline status ... 0/60
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is rebooting. Waiting for offline status ... 1/60
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is rebooting. Waiting for offline status ... 2/60
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is running. Waiting for offline status ... 3/60
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is running. Waiting for offline status ... 4/60
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is running. Waiting for offline status ... 5/60
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is running. Waiting for offline status ... 6/60
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is running. Waiting for offline status ... 7/60 
[maxipi->linode_api]: Success, the status of linode for --linode-label sun.sofibox.com is offline 

3) Default --max-call is 60 times (per second call). You can also pass --max-call, --max-retries, --timeout to extend the wait time
root@MAXIWORK:~# maxipi linode get-linode-status --server-label sun.sofibox.com --wait-until-linode-status offline --max-call 120
[maxipi->linode_api]: Getting linode status for --linode-label sun.sofibox.com ...
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is running. Waiting for offline status ... 0/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is running. Waiting for offline status ... 1/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is running. Waiting for offline status ... 2/120

```

```
1) How to reboot into linode rescue mode?
root@MAXIWORK:~# maxipi linode boot-into-rescue --linode-label sun.sofibox.com
[maxipi->linode_api]: Getting linode ID for --linode-label sun.sofibox.com ...
[maxipi->linode_api]: Notice, Linode will boot into rescue mode without mounting any disks or volumes
[maxipi->linode_api]: Putting linode sun.sofibox.com into rescue mode ...

Success response:
{}

[maxipi->status_handler->http]: Success, the linode has been set to reboot into rescue mode
All data: boot-into-rescue

2) Boot linode into rescue mode with mounting options:
[maxipi->status_handler->http]: Success, the linode has been set to reboot into rescue mode
All data: boot-into-rescue
root@MAXIWORK:~# maxipi linode boot-into-rescue --linode-label sun.sofibox.com --dev-sda disk:Boot_Disk --dev-sdb disk:OS_Disk
[maxipi->linode_api]: Getting linode ID for --linode-label sun.sofibox.com ...

[maxipi->linode_api]: Volume and disk information
[maxipi->linode_api]: ==========================
[maxipi->linode_api]: disk_volume_letter: a
[maxipi->linode_api]: label_disk_vol: disk:Boot_Disk
[maxipi->linode_api]: label_type: disk
[maxipi->linode_api]: label_name: Boot_Disk
[maxipi->linode_api]: disk_id: 81886026
[maxipi->linode_api]: volume_id: null
[maxipi->linode_api]: ==========================


[maxipi->linode_api]: Volume and disk information
[maxipi->linode_api]: ==========================
[maxipi->linode_api]: disk_volume_letter: b
[maxipi->linode_api]: label_disk_vol: disk:OS_Disk
[maxipi->linode_api]: label_type: disk
[maxipi->linode_api]: label_name: OS_Disk
[maxipi->linode_api]: disk_id: 81886029
[maxipi->linode_api]: volume_id: null
[maxipi->linode_api]: ==========================

[maxipi->linode_api]: Notice, Linode will boot into rescue mode and mount the following options:
{
  "devices": {
    "sda": {
      "disk_id": 81886026,
      "volume_id": null
    },
    "sdb": {
      "disk_id": 81886029,
      "volume_id": null
    }
  }
}
[maxipi->linode_api]: Putting linode sun.sofibox.com into rescue mode ...

Success response:
{}

[maxipi->status_handler->http]: Success, the linode has been set to reboot into rescue mode
All data: boot-into-rescue

3) You can also pass --wait-until-linode-status online option to make sure that the script wait until the rescue mode is started

4) How to start linode into a normal server operating system using config file?
   a) using maxipi linode start-linode
   
root@MAXIWORK:~# maxipi linode start-linode --linode-label sun.sofibox.com --linode-config-label OS_Config
[maxipi->linode_api]: Getting linode ID for --linode-label sun.sofibox.com ...
[maxipi->linode_api]: Getting required linode config ID for --linode-label OS_Config ...
[maxipi->linode_api]: Booting linode sun.sofibox.com ( 24805756 ) with the option --linode-config-label OS_Config ...
[maxipi->status_handler->http]: Error(400), there was something wrong when trying to boot from offline
Response details:
{
  "errors": [
    {
      "reason": "Linode 24805756 already booted."
    }
  ]
}

Note the error occur because maxipi linode start-linode can only be executed when linode is offline. So better use 

    b) maxipi linode reboot-linode

[maxipi->linode_api]: Getting linode ID for --linode-label sun.sofibox.com ...
[maxipi->linode_api]: Getting required linode config ID for --linode-label OS_Config ...
[maxipi->linode_api]: Rebooting linode sun.sofibox.com ( 24805756 ) with the option --linode-config-label OS_Config ...

Success response:
{}

[maxipi->status_handler->http]: Success, the linode has been set to boot from offline
All data: reboot-linode

and remember that you can still wait for the reboot status with the following option: --wait-until-linode-status online

5) How to shutdown linode?

root@MAXIWORK:~# maxipi linode shutdown-linode --linode-label sun.sofibox.com --wait-until-linode-status offline
[maxipi->linode_api]: Getting linode ID for --linode-label sun.sofibox.com ...
[maxipi->linode_api]: Shutting down linode sun.sofibox.com ( 24805756 ) ...

Success response:
{}

[maxipi->status_handler->http]: Success, the linode has been set to shutdown
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is . Waiting for offline status ... 0/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 1/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 2/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 3/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 4/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 5/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 6/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 7/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 8/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 9/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 10/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 11/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 12/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 13/120
[maxipi->linode_api]: Success, the status of linode for --linode-label sun.sofibox.com is offline

```

```
1) How to easily create a disk in linode?
Note that disk creation is allowed when linode is running except for disk deletion but you can pass the option --shutdown-linode-first before creating disk for peace of mind

root@MAXIWORK:~# maxipi linode create-disk --shutdown-linode-first --linode-label sun.sofibox.com --linode-disk-label Hacking_Disk --linode-disk-filetype raw --linode-disk-size 10MB
[maxipi->linode_api]: Getting linode ID for --linode-label sun.sofibox.com ...
[maxipi->linode_api]: Shutting down linode sun.sofibox.com ( 24805756 ) ...

Success response:
{}

[maxipi->status_handler->http]: Success, the linode has been set to shutdown
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is . Waiting for offline status ... 0/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 1/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 2/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 3/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 4/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 5/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 6/120
[maxipi->linode_api]: Notice, the status of linode for --linode-label sun.sofibox.com is shutting_down. Waiting for offline status ... 7/120
[maxipi->linode_api]: Success, the status of linode for --linode-label sun.sofibox.com is offline
[maxipi->linode_api]: Checking for existing entry of --linode-disk-label Hacking_Disk ...
[maxipi->linode_api]: Creating a new linode disk with --linode-disk-label Hacking_Disk ...
[maxipi->linode_api]: json_query_data: {
  "label": "Hacking_Disk",
  "filesystem": "raw",
  "size": 10
}

Success response:
{
  "id": 81887355,
  "status": "not ready",
  "label": "Hacking_Disk",
  "created": "2022-12-18T20:01:00",
  "updated": "2022-12-18T20:01:00",
  "filesystem": "raw",
  "size": 10
}

[maxipi->status_handler->http]: Success, the linode disk has been set to be created
All data: create-disk

- Note by default you are not allowed to create the same disk label but if you want you can pass the option --allow-duplicate-entries

- You can also pass the option --wait-until-disk-status ready so, that the script will only end if the disk status is ready

2) How do you delete the linode disk?

To delete the linode disk, you must pass the option --shutdown-linode-first because you cannot delete a disk if linode is running

root@MAXIWORK:~# maxipi linode delete-disk --linode-label sun.sofibox.com --linode-disk-label Hacking_Disk
[maxipi->linode_api]: Getting required linode ID for --linode-label sun.sofibox.com ...
[maxipi->linode_api]: Deleting linode disk(s) based on --linode-disk-label Hacking_Disk ...
[maxipi->linode_api]: Notice, total linode disk ID(s) found is 1
[maxipi->linode_api]: Deleting linode disk 1 / 1 with disk label Hacking_Disk and disk ID 81887355 ...
[maxipi->linode_api]: Success, the linode disk label Hacking_Disk (81887355) has been deleted!

3) Does it support multiple disks deletion?

Yes, definitely but with a prompt! (Warning, if you pass the option --scripting, you will not have prompt)

root@MAXIWORK:~# maxipi linode delete-disk --linode-label sun.sofibox.com --linode-disk-label Hacking_Disk
[maxipi->linode_api]: Getting required linode ID for --linode-label sun.sofibox.com ...
[maxipi->linode_api]: Deleting linode disk(s) based on --linode-disk-label Hacking_Disk ...
[maxipi->linode_api]: Notice, total linode disk ID(s) found is 5
[maxipi->linode_api->input]: Warning found 5 records when searching the disk label Hacking_Disk. Do you want to delete all of the disks? [default:n] [Y/n]: y
[maxipi->linode_api]: Deleting linode disk 1 / 5 with disk label Hacking_Disk and disk ID 81887710 ...
[maxipi->linode_api]: Success, the linode disk label Hacking_Disk (81887710) has been deleted!
[maxipi->linode_api]: Deleting linode disk 2 / 5 with disk label Hacking_Disk and disk ID 81887739 ...
[maxipi->linode_api]: Success, the linode disk label Hacking_Disk (81887739) has been deleted!
[maxipi->linode_api]: Deleting linode disk 3 / 5 with disk label Hacking_Disk and disk ID 81887764 ...
[maxipi->linode_api]: Success, the linode disk label Hacking_Disk (81887764) has been deleted!
[maxipi->linode_api]: Deleting linode disk 4 / 5 with disk label Hacking_Disk and disk ID 81887793 ...
[maxipi->linode_api]: Success, the linode disk label Hacking_Disk (81887793) has been deleted!
[maxipi->linode_api]: Deleting linode disk 5 / 5 with disk label Hacking_Disk and disk ID 81887806 ...
[maxipi->linode_api]: Success, the linode disk label Hacking_Disk (81887806) has been deleted!
All data: delete-disk

- Or you can pass the option --first-record to only delete the first linode disk ID or --last-record to delete the last (or latest linode disk ID)
- You can also pass the option --check-busy-status before creating disk so you don't have any stucked processes at the background that prevent you from deleting or creating disk
- Note that there is a difference between delete-disk and delete-disks (with a plural). When you use delete-disks action, you don't have to pass the option --linode-disk-label, it will delete all disks but with a prompt!

4) How to update disk information after created? 
linode only allow to resize the disk and change disk system file type. I will include this in the next version

```

```
1) How to create linode config profile? Creating config profile using maxipi is easier than using linode-cli

- You are only required to give a 1) --linode--label <label>, and one disk or volume mounting option using the following format: 

--dev-sdN <type>:<disk/volume label>

root@MAXIWORK:/home/maxi32# maxipi linode create-config --linode-label sun.sofibox.com --linode-config-label "Hacking_Config" --dev-sda disk:Boot_Disk
[maxipi->linode_api]: Getting required linode ID for --linode-label sun.sofibox.com ...
[maxipi->linode_api]: Checking for existing entry of --linode-config-label Hacking_Config ...

[maxipi->linode_api]: Volume and disk information
[maxipi->linode_api]: ==========================
[maxipi->linode_api]: disk_volume_letter_count: 1
[maxipi->linode_api]: disk_volume_letter: a
[maxipi->linode_api]: label_disk_vol: disk:Boot_Disk
[maxipi->linode_api]: label_type: disk
[maxipi->linode_api]: label_name: Boot_Disk
[maxipi->linode_api]: disk_id: 81890530
[maxipi->linode_api]: volume_id: null
[maxipi->linode_api]: ==========================

[maxipi->linode_api]: Notice, Linode config will be created and mount with the following options:
{
  "label": "Hacking_Config",
  "kernel": "linode/latest-64bit",
  "run_level": "default",
  "virt_mode": "paravirt",
  "root_device": "/dev/sda",
  "helpers": {
    "updatedb_disabled": false,
    "distro": false,
    "modules_dep": false,
    "devtmpfs_automount": false,
    "network": false
  },
  "devices": {
    "sda": {
      "disk_id": 81890530,
      "volume_id": null
    }
  }
}
[maxipi->linode_api]: Creating a new linode config with --linode-disk-label  ...

Success response:
{
  "id": 43528779,
  "label": "Hacking_Config",
  "helpers": {
    "updatedb_disabled": false,
    "distro": false,
    "modules_dep": false,
    "network": false,
    "devtmpfs_automount": false
  },
  "kernel": "linode/latest-64bit",
  "comments": "",
  "memory_limit": 0,
  "created": "2022-12-19T05:39:43",
  "updated": "2022-12-19T05:39:43",
  "root_device": "/dev/sda",
  "devices": {
    "sda": {
      "disk_id": 81890530,
      "volume_id": null
    },
    "sdb": null,
    "sdc": null,
    "sdd": null,
    "sde": null,
    "sdf": null,
    "sdg": null,
    "sdh": null
  },
  "initrd": null,
  "run_level": "default",
  "virt_mode": "paravirt",
  "interfaces": []
}

[maxipi->status_handler->http]: Success, the linode config profile for --linode-config-label Hacking_Config has been created
All data: create-config

- Some can also pass other settings to change their default values. For example:

maxipi linode create-config --linode-label sun.sofibox.com \
--linode-config-label Boot_config --linode-config-comment "The installer boot boot configuration" --linode-config-virtual-mode "paravirt" \
--linode-config-kernel "linode/direct-disk" --linode-config-runlevel "default" --linode-config-memory-limit "4096" --linode-config-rootdev "/dev/sda" \
--linode-config-enable-distro-helper --linode-config-disable-update_db --linode-config-dep-helper --linode-config-automount-devtmpfs --linode-config-autoconf-network \
--dev-sda "disk:Boot_Disk" --dev-sdb "disk:OS_Disk"

- Remember to pass --allow-duplicate-entries option if you want to create duplicate config profile with the same label name


2) How to remove config profile created?

This is similar like removing disk, there is a difference between remove-config and remove-configs (with plural). The one with plural will remove all configs with a prompt
You can pass --first-record or --last-record to only remove the first or the last config profile

root@MAXIWORK:~# maxipi linode remove-config --linode-label sun.sofibox.com --linode-config-label "Hacking_Config" --first-record
[maxipi->linode_api]: Getting required linode ID for --linode-label sun.sofibox.com ...
[maxipi->linode_api]: Deleting linode config(s) based on --linode-config-label Hacking_Config ...
[maxipi->linode_api]: Notice, total linode config ID(s) found is 1
[maxipi->linode_api]: Deleting linode config 1 / 1 with config label Hacking_Config and config ID 43528779 ...
[maxipi->linode_api]: Success, the linode config label Hacking_Config (43528779) has been deleted!
All data: remove-config

```

```
1) How to get domain ID?
root@MAXIWORK:/home/maxi32# maxipi linode domain-get-id --domain-name maxibi.com
[maxipi->linode_api]: OK, the given --domain-name maxibi.com is a valid FQDN
[maxipi->linode_api]: Getting domain ID for --domain-name maxibi.com ...
[maxipi->linode_api]: Domain ID for --domain-name maxibi.com is 1932201

- or with scripting option:
root@MAXIWORK:/home/maxi32# maxipi linode domain-get-id --domain-name maxibi.com --scripting
1932201

- Regardless of using --scripting option or not, the script will exit with success code 0 else you will get error code 1

2) How do you create a domain?

- This is super easy. You can use the following command:

root@MAXIWORK:/home/maxi32# maxipi linode create-domain --domain arafatx.com --domain-type master --domain-email webmaster@arafatx.com --domain-ttl 30 --domain-status active --rebuild
[maxipi->linode_api]: OK, the given --domain-name arafatx.com is a valid FQDN
[maxipi->linode_api]: OK, the given --domain-type master is a valid
[maxipi->linode_api]: OK, the given --domain-email webmaster@arafatx.com is a valid
[maxipi->linode_api]: OK, the given --domain-ttl 30 is a valid
[maxipi->linode_api]: OK, the given --domain-status active is a valid
[maxipi->linode_api]: Notice, the option --rebuild is enabled. The existing domain arafatx.com will be deleted (if found) before creating a new one
[maxipi->linode_api]: Getting a required domain ID for --domain-name arafatx.com ...
[maxipi->linode_api]: Found existing domain name arafatx.com. Deleting domain arafatx.com ...
[maxipi->linode_api]: Creating a domain name arafatx.com ...

Success response:
{
  "id": 1933642,
  "type": "master",
  "domain": "arafatx.com",
  "tags": [],
  "group": "",
  "status": "active",
  "errors": "",
  "description": "",
  "soa_email": "webmaster@arafatx.com",
  "retry_sec": 0,
  "master_ips": [],
  "axfr_ips": [],
  "expire_sec": 0,
  "refresh_sec": 0,
  "ttl_sec": 30,
  "created": "2022-12-19T05:54:45",
  "updated": "2022-12-19T05:54:45"
}

- The option --rebuild means it will force creating the domain by removing existing domain first and create the new one. If you want to disable the domain after being created use the option --domain-status disabled

- You can also provide default values when creating a domain with the option --default so that you can ignore other options:

root@MAXIWORK:/home/maxi32# maxipi linode create-domain --domain thearafatx.com --default
[maxipi->linode_api]: Notice, the option --default is set. The default values will be set for the following options:
--domain-type=master
--domain-email=webmaster@
--domain-ttl=30
--domain-status=active
[maxipi->linode_api]: OK, the given --domain-name thearafatx.com is a valid FQDN
[maxipi->linode_api]: OK, the given --domain-type master is a valid
[maxipi->linode_api]: OK, the given --domain-email webmaster@thearafatx.com is a valid
[maxipi->linode_api]: OK, the given --domain-ttl 30 is a valid
[maxipi->linode_api]: OK, the given --domain-status active is a valid
[maxipi->linode_api]: Creating a domain name thearafatx.com ...

Success response:
{
  "id": 1933643,
  "type": "master",
  "domain": "thearafatx.com",
  "tags": [],
  "group": "",
  "status": "active",
  "errors": "",
  "description": "",
  "soa_email": "webmaster@thearafatx.com",
  "retry_sec": 0,
  "master_ips": [],
  "axfr_ips": [],
  "expire_sec": 0,
  "refresh_sec": 0,
  "ttl_sec": 30,
  "created": "2022-12-19T05:57:29",
  "updated": "2022-12-19T05:57:29"
}

[maxipi->status_handler->http]: Success, the domain name thearafatx.com has been created
All data: create-domain

```



```
The codes have been updated with many changes and the output above might have changed. Use --help to show information about an action

You can always trigger the help within an action. Example to understand how to create a domain:


`maxipi linode create-domain --help`

This will show the following help specific for this action




root@MAXIWORK:/home/maxi32# maxipi linode create-domain --help
Action name: create-domain
Required option(s):
--domain-name <domain_name>
--domain-type <domain_type>
--domain-master-ips <1 or more ip_addresses separated by a space> (REQUIRED only if --domain-type is slave)
--domain-email <domain_email> (REQUIRED only if --domain-type is master)
Purpose: This action is used to create / add a domain in linode
Notes:
The option --domain-email is required by linode if the --domain-type is set to master. However, if you don't use this option it will have a default email value with the following syntax: webmaster@<domain_name>
You cannot create a domain name with the same label name if it is already exist in linode. If a domain is already exist, you can use an option --rebuild or --force to recreate the domain from scratch
You will not be able to create some other domains that are reserved in linode domain management (eg: microsoft.com, apple.com)
Example 1 (create a master domain by just specifying a --domain-name option):
maxipi linode create-domain --domain-name sofibox.com
Example 2 (create a master domain by specifying the full options):
maxipi linode create-domain --domain-name sofibox.com --domain-type master --domain-email webmaster@sofibox.com --domain-ttl 30 --domain-status active
Example 3: (use the option --rebuild to recreate the same domain name if it already exist)
maxipi linode create-domain --domain-name sofibox.com --rebuild
Example 4: (you can disable a new domain after being created with the following commands)
maxipi linode create-domain --domain-name sofibox.com --domain-status disabled
Example 5: (create a slave domain with 1 domain master IP)
maxipi linode create-domain --domain-name sofibox.com --domain-type slave --domain-master-ips 1.2.3.4
Example 6: (create a slave domain with multiple domain master IPs)
maxipi linode create-domain --domain-name sofibox.com --domain-type slave --domain-master-ips "1.2.3.4 1.2.3.4 ::1"
```
