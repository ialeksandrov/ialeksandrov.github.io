---
layout: post
title:  "How to backup and restore Hashicorp Vault snapshots"
date:   2023-10-11 22:21:14 +0300
categories: jekyll update
---
### Backup / Snapshot
To create snapshot/backup of your Hashicorp Vault and you are using raft as a backend you can use the command
```shell
$ vault operator raft snapshot save backup.snap
```
This will take the snapshot using a consistent mode that forwards the request to the cluster leader, and the leader will verify it is still in power before taking the snapshot.

Store these snapshots in a secure place away from the Vault clusters. For example you can use S3 bucket.

NOTE: Backup process can be automated with a short shell script using this command plus a logic for snapshots retention.
### Restore
Bring the Vault cluster back online following the circumstances that required you to restore from the backup. You will need to reinitialise the Vault cluster and log in with the new root token generated during its reinitialisation. Note that these will be temporary- the original unseal keys will be needed following restore.

Copy the Vault Raft Snapshot file onto a Vault cluster member and run the below command, replacing the filename with that of your snapshot file. Note, the ```-force``` option is required here since the Auto-unseal or Shamir keys will not be consistent with the snapshot data as you will be restoring a snapshot from a different cluster. Snapshots can be stored S3 bucket. To copy a snapshot from S3 execute:

```shell
$ aws s3 cp s3://<bucket_name>/backup.snapshot . 
$ vault operator raft snapshot restore -force backup.snapshot
```

Once you have restored the Raft snapshot you will need to unseal your Vault cluster again using the following command:
```shell
$ vault operator unseal [unseal_key]
```
### Restore specifics:

If you need to restore a snapshot to a fresh vault instance you need to configure it similarly.  This means even the sealing method should be as same as the vault instance from which we are taking the snapshot.

To configure it correctly you need to know if your main vault from which you are taking the snapshot using the AWS KMS auto-unseal method. If you are using this method the following lines should be added to the configuration:
```
seal "awskms" {
  region     = "<aws region>"
  kms_key_id = "<existing kms key id>"
}
```

NOTE: The snapshot contains Data Encryption Key, which is encrypted by the aws KMS. If your vault running on minikube doesn’t have access to that KMS, the newly restored vault will not unseal.

This is designed to protect the snapshot.

If you are not feeling comfortable restoring with CLI there is also a GUI Version from your fresh Vault instance.
When you authenticate you will see a drop-down button “Status” → “Raft Storage” → “Snapshots” → “Restore”
After this, there will be an option to provide a snapshot file.
After choosing the snapshot file tick the checkbox “Force restore” and click the “Restore” button.
This is the same as the CLI sequence from the above section.