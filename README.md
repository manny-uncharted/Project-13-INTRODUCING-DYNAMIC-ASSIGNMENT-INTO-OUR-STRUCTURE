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

![create new branch](img/create-new-branch)