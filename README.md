# Project-13-INTRODUCING-DYNAMIC-ASSIGNMENT-INTO-OUR-STRUCTURE


## Table of Contents.
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Introducing Dynamic Assignment Into Our structure](#introducing-dynamic-assignment-into-our-structure)


## Introduction
In this project, we will introduce dynamic assignments by using 'include' module.

Now you may be wondering, what is the difference between static and dynamic assignments?

Well, from the previous project, you can already tell that static assignments use the import Ansible module. The module that enables dynamic assignments is included.

Hence,

```
import = Static
include = Dynamic
```

When the import module is used, all statements are pre-processed at the time playbooks are parsed. Meaning, when you execute the site.yml playbook, Ansible will process all the playbooks referenced during the time it is parsing the statements. This also means that, during actual execution, if any statement changes, such statements will not be considered. Hence, it is static.

On the other hand, when the 'include' module is used, all statements are processed only during the execution of the playbook. Meaning, after the statements are parsed, any changes to the statements encountered during execution will be used.

Take note that in most cases it is recommended to use static assignments for playbooks because it is more reliable. With dynamic ones, it is hard to debug playbook problems due to their dynamic nature. However, you can use dynamic assignments for environment-specific variables as we will be introducing in this project.

## Prerequisites
- Infrastructure: AWS.
- Web Servers: Linux Red Hat.
- Database Server: Ubuntu 20.04 + MySQL.
- Storage Server: Red Hat 8 + NFS.
- Load Balancer: Ubuntu 20.04 (Nginx).
- Jenkins Server.
- Configure Ansible to perform dynamic assignments.
- Source Code: Download from <a href="https://github.com/manny-uncharted/ansible-config-mgt.git">here</a>.


## Introducing Dynamic Assignment Into Our structure

- In our GitHub repository start by creating a new branch called 'dynamic-assignments'.

```
git checkout -b dynamic-assignments
```

Results:

![create new branch](img/create-new-branch.png)

- Create a new directory called 'dynamic-assignments' in the 'ansible-config-mgt' directory.

```bash
mkdir dynamic-assignments
```

Results:

![create new directory](img/create-new-directory.png)

- Inside this folder, create a new file and name it 'env-vars.yml'. Then we will instruct 'site.yml' to include this playbook later. For now, we would keep building the structure.

Results:

![create new file](img/create-new-file.png)

- Our GitHub should have the following structure by now.


```
├── dynamic-assignments
│   └── env-vars.yml
├── inventory
│   └── dev
    └── stage
    └── uat
    └── prod
└── playbooks
    └── site.yml
└── roles (optional folder)
    └──...(optional subfolders & files)
└── static-assignments
    └── common.yml
```
<b>Note:</b> It is recommended to have all the codes managed and tracked in GitHub. This will help you to keep track of all the changes you make to your code.

Since we will be using Ansible to configure multiple environments, and each of these environments will have certain unique attributes such as server names, IP addresses, etc., we will need to create a separate inventory file for each environment. This will help us to easily manage our environments.

- Create a folder called 'env-vars' and inside it create new '.yml' files for each environment. This is where we would set our environment-specific variables.
Our folder structure should look like this:

```
├── dynamic-assignments
│   └── env-vars.yml
├── env-vars
    └── dev.yml
    └── stage.yml
    └── uat.yml
    └── prod.yml
├── inventory
    └── dev
    └── stage
    └── uat
    └── prod
├── playbooks
    └── site.yml
└── static-assignments
    └── common.yml
    └── webservers.yml
```

Results:

![create new folder](img/create-env-vars-folder.png)

- Now in 'env-vars.yml' file add the following instruction

```
---
- name: collate variables from env specific file, if it exists
  hosts: all
  tasks:
    - name: looping through list of available files
      include_vars: "{{ item }}"
      with_first_found:
        - files:
            - dev.yml
            - stage.yml
            - prod.yml
            - uat.yml
          paths:
            - "{{ playbook_dir }}/../env-vars"
      tags:
        - always
```

Results:

![add instruction](img/add-instruction.png)

Note: Notice 3 things to notice here:

We used include_vars syntax instead of include, this is because Ansible developers decided to separate different features of the module. From Ansible version 2.8, the include module is deprecated and variants of include_* must be used. These are:
- include_role
- include_tasks
- include_vars

In the same version, variants of import also introduces, such as:

- import_role
- import_tasks

We made use of special variables { playbook_dir } and { inventory_file }. { playbook_dir } will help Ansible to determine the location of the running playbook, and from there navigate to other paths on the filesystem. { inventory_file } on the other hand will dynamically resolve to the name of the inventory file being used, then append .yml so that it picks up the required file within the env-vars folder.

We are including the variables using a loop. with_first_found implies that, looping through the list of files, the first one found is used. This is good so that we can always set default values in case an environment-specific env file does not exist.

