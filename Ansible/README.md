# Openshift Deployment Process
This folder contains the needed ansible files to deploy Openshift to the vmware environment using the IPI installer method.  

Here is a current layout of this base ansible folder:
```
├── ansible.cfg
├── dev.yaml (Variables file for the dev environment;  to be used with the '--extra-vars=@<variablefilename> flag')
├── files
    |-  .pub ssh key of the user deploying the Openshift cluster
│   ├── pull-secret.txt (pull secret used for install-config)
│   ├── README.md
│   └── vcenter (password used with the vcenter account specified in the install-config to deploy the vms for openshift)
├── install-ipi.yaml (Deployment playbook for cluster deployment; example usage: ansible-playbook -i inventory/ install-ipi.yaml --extra-vars="@dev.yaml")
├── inventory
│   ├── group_vars (copies of variable files kept here)
│   └── hosts
├── README.md
├── templates
    └── install-config.j2 (Template ansible provides openshift-installer to input the variables and deploy from)
```

Changes for the Openshift install will be made to the following vars files only:
`dev.yaml`
(File can be called whatever you'd like)

The vars file is used to input their variables into the template/install-config.j2 which in turn creates the install-config.yaml to be used by the Openshift-installer.
These live in the base ansible folder where the install-ipi.yaml playbook resides. 


### Installer Playbook

The `install-ipi.yaml` playbook is the glue that holds all the pieces together and the one which will run to deploy the cluster.  
Example usage:
`ansible-playbook -i inventory/ install-ipi.yaml --extra-vars="@dev.yaml" --ask-vault-pass`

This playbook will deploy the openshift vms to vcenter, starting with the bootstrap vm followed by the masters, power them up, and then after some time, the workers will deploy.  Once the cluster is up, the bootstrap will destroy, and the playbook will continue by grabbing the infrastructureName of the newly deployed cluster, inputting that value into the overlay/machineset files, git commit and push the files to git, deploy the argo instance, and finally deploying all the necessary operators, including the machinesets.



