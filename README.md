
# CheckPoint MDS Gateways Report
This Ansible playbook will retrieve all your gateways information from the CheckPoint MDS and generate a report as a .csv file.

By default this report will give you:
1. device name
2. ipv4-address
3. type of device
4. gaia version
5. access-policy name
6. domain name

But you can easily modify the fields in the task "show_gateways_servers.yml".

## Requirements
```
* Ansible == 2.9.7
* Python == 3.6.8
* CentOs == 7
```

## Usage
```
ansible-playbook -i <MDS_INVENTORY> main.yml
```

## Tested on
* Full-Gaia: R80

## Important notes
The authentication for this playbook is just a placeholder, you must configure this section to your enterprise solution. My suggestion would be to use the [URI module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/uri_module.html). This playbook requires authentication, "global_login.yml" expects **"checkpoint_user"** and **"checkpoint_password"** to be defined.

This playbook is based on [URI module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/uri_module.html) and it's transforming the data from .json to .csv with the help of the jinja template "templates/csv.j2".

Last and more important, **"host_vars/mds-fqdn.yml"** is a place holder, this should be renamed to match your MDS fully qualified domain name and the offset variable is just a list to iterate over the amount of devices. CheckPoint has a limitation about the amount of devices you can retrieve using this API call, where 500 is the limit, so you can implement the logic like this: 

* MDS with 400 devices: *offset: ["0"]*
* MDS with 900 devices: *offset: ["0","500"]*
* MDS with 1250 devices: *offset: ["0","500","1000"]*

If you have more than one MDS in your organization, just create more files under **"host_vars/"** and the report will identify them all.


## Owner
[Giancarlo Mora](mailto:giank.ma@gmail.com)