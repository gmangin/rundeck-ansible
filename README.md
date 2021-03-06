### RUNDECK PROCESS

# Install
## PLAYBOOK
  * ansible-playbook playbooks/rundeck.yml -l your_host
  * Example: `ansible-playbook playbooks/rundeck.yml`

## ROLE
The role rundeck will install and setup rundeck

# Add or Delete PROJECT

  * if you want to add a project, you have to update `rundeck_jobs` variable. Update it on `defaults/main.yml`
  * Example1: You want to add project TOTO with no jobs

``` yaml
rundeck_jobs:
  TOTO: []
```

  * Example2: You want to add project TOTO with job tata (see Add or Delete JOBS)

``` yaml
rundeck_jobs:
  TOTO:
    - tata
```

  * Example2: You want to delete project TOTO, you need to delete on rundeck web and on `defaults/main.yml` (so ansible won't push the project again on rundeck web)

``` yaml
rundeck_jobs:
  TUTU: []
  TOTO:
    - tata
```
``` yaml
rundeck_jobs:
  TUTU: []
```

# Add or Delete JOBS

> Note: you can't update or override existing jobs, you have to delete the previous one first.

  * if you want to add a job on project TOTO:
    * 1. you have to update `rundeck_jobs` variable. Update it on `defaults/main.yml`
	* 2. you have to add your jobs on `rundeck_jobs_path` (you can find this variable on `defaults/main.yml`)
	  * you can create your job from rundeck web and click `Download Job definition in YAML`, then move it to `rundeck_jobs_path`
	  * or you can cp an existing job and update the part you need to update.
  * Example1: You want to add tata on project TOTO

``` yaml
rundeck_jobs:
  TOTO:
    - tata
```

  * Example2: You want to delete tata on project TOTO, you need to delete on rundeck web, on `defaults/main.yml` (so ansible won't push the project again on rundeck web) and on `{{ rundeck_jobs_path }} /<name of your job>.yaml`

```yaml
rundeck_jobs:
  TOTO:
    - tata
    - tutu
```
```yaml
rundeck_jobs:
  TOTO:
    - tutu
```

# Add or Delete KEY STORAGE
  * if you want to add a key storage, you have to update `rundeck_storage` variable. Update it on `defaults/main.yml`
  * Example: You want to add key storage TOTO with the pass TACACS and password tutu
```yaml
tutu_var: "{{ tutu_var_vault }}" # add tutu on vault file, please read the README of the project for more information

rundeck_storage:
  - name: TOTO
    content: "{{ tutu_var }}"
    path: TACACS/
```
  * if you want to delete, just clean `rundeck_storage` variable and delete your key password on rundeck web
```
rundeck_storage: []
```

# Add or Delete ACL PROJECT
  * Be careful, if you update acl, be aware that rundeck will reboot, you need to check that no job are on process or are schedule !!!!!!!!!
  * if you want to add an acl for a project, you have to update `rundeck_acl_projects` variable. Update it on `defaults/main.yml`
  * Example: You want to add group ldap customer-success to the projects TOTO and TATA:

```yaml
rundeck_acl_projects:
  customer-success:
    - TOTO
    - TATA
```
