# Openshift Deployment Process
This folder contains the needed ansible files to deploy Openshift to the vmware environment using the IPI installer method.  

Here is a current layout of this base ansible folder:
```
├── README.md
├── ansible.cfg
├── files
│   ├── README.md
│   ├── pull-secret ((pull secret used for install-config)
│   ├── username.pub (ssh key of the user deploying the Openshift cluster)
│   └── vcenter ((password used with the vcenter account specified in the install-config to deploy the vms for openshift)
├── install-ipi.yaml ((Deployment playbook for cluster deployment; example usage: ansible-playbook -i inventory/ install-ipi.yaml --extra-vars="@dev.yaml")
├── inventory
│   ├── group_vars
│   └── hosts
├── templates
│   └── install-config.j2 ((Template ansible provides openshift-installer to input the variables and deploy from)
└── vars.yaml (Variables file for the dev environment;  to be used with the '--extra-vars=@<variablefilename> flag')

```

Changes for the Openshift install will be made to the following vars files only:
`vars.yaml`

The vars file is used to input their variables into the template/install-config.j2 which in turn creates the install-config.yaml to be used by the Openshift-installer.
These live in the base ansible folder where the install-ipi.yaml playbook resides. 


### Installer Playbook

The `install-ipi.yaml` playbook is the glue that holds all the pieces together and the one which will run to deploy the cluster.  
Example usage:
`ansible-playbook -i inventory/ install-ipi.yaml --extra-vars="@vars.yaml" --ask-vault-pass`

This playbook will deploy the openshift vms to vcenter, starting with the bootstrap vm followed by the masters, power them up, and then after some time, the workers will deploy.  Once the cluster is up, the bootstrap will destroy, and the playbook will continue to the next tasks of installing an argo operator by pulling the login token from the cluster, logging in, and deploying an argo operator using local operator config files.




