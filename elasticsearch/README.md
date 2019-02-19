# IBM Cloud Databases for Elasticsearch Migration Guide

This script will allow you to migrate your Compose for Elasticsearch database to IBM Cloud Databases for Elasticsearch. Make sure that the same resources that you've given your original database is the same as the target database. The script will mount your IBM Cloud Object Storage / S3 bucket to both databases and takes snapshots of your Compose for Elasticsearch database and places those into a secure bucket. Then, your IBM Cloud Databases for Elasticsearch will read and restore those snapshots from that IBM Cloud Object Storage / S3 bucket. 

The assumption is that you're writing to your Compose database while you're taking snapshots and restoring to the Databases for Elasticsearch database. Incremental restores can only work if the number of shards of each index on both instances match. Don't try to reindex and change the number of shards of any indices once you've started taking snapshots. We're taking four snapshots in this example script. Each snapshot will take a shorter amount of time than the previous one due to the snapshot backing up the latest writes to your Compose database before restoring them in the Databases for Elasticsearch deployment.

## Bring your own object storage

The script will work with an S3 compatible object storage bucket. IBM Cloud Object Storage or S3 will work with these examples. Therefore, you can choose the storage solution that works with your use case.

## Variables used in the migration script

#### Compose Credentials

- `compose_username` - tthe username combination used to authenticate against the Compose deployment
- `compose_password` - the password combination used to authenticate against the Compose deployment
- `compose_endpoint` - the hostname used to talk to the Compose deployment
- `compose_port` - the port used to talk to the Compose deployment

#### Databases for Elasticsearch Credentials

- `icd_username` - the hostname used to talk to the Databases for Elasticsearch deployment
- `icd_password` - the password combination used to authenticate against the Databases for Elasticsearch deployment
- `icd_endpoint` - the hostname used to talk to the Databases for Elasticsearch deployment
- `icd_port` - the port used to talk to the Databases for Elasticsearch deployment
- `export CURL_CA_BUNDLE` - path to a file containing the SSL client certificate used to connect to your Databases for Elasticsearch

#### IBM Cloud Object Storage / S3 Credentials

- `storage_service_endpoint` - the IBM Cloud Object Storage/S3 endpoint hostname
- `bucket_name` - the name of the IBM Cloud Object Storage/S3 bucket you're going to use
- `access_key` - the IBM Cloud Object Storage/S3 bucket HMAC access key
- `secret_key` - the IBM Cloud Object Storage/S3 bucket HMAC secret key
- `path_to_snapshot_folder` - the base path inside the bucket where we want to write all snapshot data. Make sure there's no leading slash (e.g: folder1/folder2, not /folder1/folder2)

## Running the script

Once you've downloaded and added your credentials to the script, you'll need to make it executable in the terminal. 

For macOS or Linux:

```shell
chmod a+x elasticsearch_migration.sh
```

Then run the script in your terminal, e.g.:

```shell
./elasticsearch_migrate.sh
```