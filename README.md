# OpenShift etcd Backup
Simple OpenShift Cronjob that creates a etcd snapshot and writes it to a Persistent Volume. 

## Requirements
```
etcd running on static pods on masters
python >= 2.7
openshift >= 0.6
PyYAML >= 3.11
```

## Installation
Tested on OpenShift 3.11

```
ansible-playbook playbook.yml \
-e openshift_etcd_backup_job_name="cronjob-etcd-backup" \
-e openshift_etcd_backup_schedule="0 6,18 * * * * "
-e openshift_etcd_backup_job_service_account="etcd-backup"
-e openshift_etcd_backup_namespace="etcd-backup"
-e openshift_etcd_backup_image="registry.redhat.io/rhel7/etcd"
-e openshift_etcd_backup_image_tag="3.2.2"
-e openshift_etcd_backup_storage_size="10G"
-e openshift_etcd_backup_deadline="3600"

oc adm policy add-scc-to-user privileged system:serviceaccount:${openshift_etcd_backup_namespace}:etcd-backup
```

| Parameter  | Description | Defaults |
| ------------- | ------------- | ------------- |
| openshift_etcd_backup_job_name | Name of the Scheduled Job to Create | cronjob-etcd-backup |
| openshift_etcd_backup_schedule | Cron Schedule to Execute the Job | 0 6,18 * * * |
| openshift_etcd_backup_job_service_account | Name of the Service Account To Execute the Job As | etcd-backup |
| openshift_etcd_backup_namespace | Name of the Namespace where to deploy the Scheduled Job | etcd-backup |
| openshift_etcd_backup_image | Image to use for the container | registry.redhat.io/rhel7/etcd |
| openshift_etcd_backup_image_tag | Image Tag to use for the container. Needs to be at least 3.11  | 3.2.2 |
| openshift_etcd_backup_storage_size | Size of the Persistent Volume  | 10G |
| openshift_etcd_backup_deadline | Duration of the job runtime  | 3600 |
| openshift_etcd_backup_retention_time | Retention time in days  | 2 |
