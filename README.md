# ansible_examples

ANSIBLE tool to check if a local dbaadm script is identically on all servers 

**Path : ROOT_PROJECT_DIR**

**Ansible_palybook: check_one.yaml**. Playng this playbook make no change on distant server ni on local.

Hosts are defined in directory `inventort/hosts.yaml` and are splitted in four perimeters: `noprod`, `prod` and `all`.

Feel free to add new hosts if heeded.

Inventory hosts file is an YAML file, and the ssh key for ansible is added in `vars` section og the file.

**NOTE :** Check the host list for its completude.

The golden source is in directory files/bin/… which reproduce the directory structure on distant server. 

The playbook compute the checksum of the local file and compare it with the checksum of distant file. If there is any difference or the distant file doesn't exist an error is printed.


**Exemple 1:**
Check the file bin/test.sh for the perimeter prod_exacc
```bash
    $> cd $ROOT_PROJECT_DIR
    $> ansible-playbook -i inventory check_one.yaml -e "file_to_check=bin/test.sh" -e "perimeter=prod"
    ...
    ok: [host1] => {
    "msg": "Compare files local 4e38fdf98365725d2e5569f79e7671d3cf75cb5e vs remote 4e38fdf98365725d2e5569f79e7671d3cf75cb5e"
    }
    ok: [host2] => {
        "msg": "Compare files local 4e38fdf98365725d2e5569f79e7671d3cf75cb5e vs remote 4e38fdf98365725d2e5569f79e7671d3cf75cb5e"
    }

    PLAY RECAP *************************************************************************************************************************************************************************************************************************************************
    host1 : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    host2 : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    host3 : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    host4 : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    ....
```
  

**Exemple 2 :**

A simple script `check_many.sh` (6 lignes) check all files in  `bin` on perimeter prod
The logfile of the script is /tmp/check_many.log
```bash
    $> cd $ROOT_PROJECT_DIR
    $> ./check_many.sh
    ….
```

# To go further

**Copy a file on a distant perimeter**
```bash
    $> cd $ROOT_PROJECT_DIR    
    $> ansible -i inventory -m copy -a "src=files/bin/test.sh dest=/home/my_user/bin/test.sh mode=0755 backup=yes" prod
    ...
```
The command  copy the file `test.sh` from source to the perimeter `prod` making a backup backup on a distant server if the file exists. The rights will be  0755. <br> *Nothing is done if the local source file is identically with distant file.*

**List the inventory contents**
```bash
    $> cd $ROOT_PROJECT_DIR
    $> ansible-inventory --inventory inventory  --graph [prod|nopord]
    @all:
    |--@prod:
    |  |--host1
    |  |--host2
    |--@noprod:
    |  |--host3
    |  |--host4
    ...
```
