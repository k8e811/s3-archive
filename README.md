# S3 Sync

Generic of existing tooling to obtain archive history of public records S3 buckets.

## Installation

* Clone this repository.
* Add the bin directory of this repository to your path
* Install the AWS cli.  We recommend using asdf.

## Other Setup

* Keep version history from contaminating git repo
  * `cd versions; git init .`
* Per bucket snapshots
  * Create the directory zfs-s3
  * For each bucket provide a symlink to the directory (preferably mount
    root) of the zfs filesystem for that bucket's snapshots
* Bucket snapshots to a different filesystem
  * Move/Remove the zfs-s3 directory and create a symlink to the the directory
    (preferably mount root) of the zfs filesystem to hold all the bucket snapshots

## Usage

### monitor-s3-object-versions

`monitor-s3-object-versions`

* Reads `monitor.buckets` for a list of buckets
* Creates a `versions` directory if it does not exist
* Initializes the ```versions``` directory as a git repo
* For each bucket in monitor.buckets
  * Performs an aws s3api list-object-versions to ```versions/$bucket```
  * Checks if ```versions/$bucket``` has changed and adds the bucket to sync list
* Commits the list-objects files
* Creates ```sync.buckets``` with the list of buckets that changed
* Zero exit code when there are changes.  Non-zero when there are not (or failure)

### sync-s3-zfs

```sync-s3-zfs```

* Reads ```${SYNC_S3_BUCKETS:-sync.buckets}``` for a list of buckets
* For each bucket
  * Creates a zfs-s3/.logs directory
  * Creates a zfs-s3/$bucketname directory for each bucket
  * Fails if the directory is NOT zfs
  * Syncs using ```aws s3 sync ...```
  * Creates a snapshot indicating start time and bucket

### sync-s3-minio

```sync-s3-minio```

Not implemented

* Thin wrapper over
  [mc mirror](https://min.io/docs/minio/linux/reference/minio-mc/mc-mirror.html)
  probably using '--watch'
