# Openshift Deployment Process
This folder defines an ansible role to deploy a managed OCP cluster from an ACM hub cluster.

Here is a current layout of this base role folder:


```
├── README.md
├── inventory
│   └── inventory.yml
├── playbook.yml
└── roles
    └── managed-clusters-role
        ├── defaults
        │   └── main
        │       ├── vsphere-secret-vars.yml
        │       └── vsphere-vars.yml
        ├── handlers
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── tasks
        │   └── main.yml
        └── templates
            ├── cluster-install-config-secret.yaml.j2
            ├── full-deploy.j2
            ├── klusterletaddonconfig.yaml.j2
            ├── managedcluster.yaml.j2
            ├── namespace.yaml.j2
            ├── pull-secret.yaml.j2
            ├── ssh-key.yaml.j2
            ├── vs-ca-certs.yaml.j2
            ├── vs-certs.yaml.j2
            ├── vs-clusterdeployment.yaml.j2
            ├── vs-creds.yaml.j2
            ├── vs-install-config.yaml.j2
            └── vs-worker-machinepool.yaml.j2

```

The operations contained in tasks/main.yml will run through the steps of creating the necessary dirs and files needed for the managed cluster and will then apply those accordingly.

To run this role: `ansible-playbook playbook.yml -i inventory/ --ask-vault-pass`

In summary, this playbook will generate the necessary yaml files it needs for ACM to create and deploy a spoke cluster in vsphere.  It does this by reading the vars files in defaults, populating the jinja templates with those vars, creating the yamls, and then combining all of these yamls in one 'master' yaml file called 'full-deploy.yaml'.  

Once this 'full-deploy.yaml' file is created, a user can log into the ACM hub cluster cli, and `oc apply` the full-deploy.yaml and the cluster will begin the creation process.  
