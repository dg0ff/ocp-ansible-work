# Openshift Deployment Process
This folder defines an ansible role to deploy a managed OCP cluster from an ACM hub cluster.

Here is a current layout of this base role folder:
```
├── README.md
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


This role will create the necessary yaml files from templates and apply them within the hub cluster.  

