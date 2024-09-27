# S3 Sync

Generic of existing tooling to obtain archive history of public records S3 buckets.

## Installation

* Clone this repository.
* Add the bin directory of this repository to your path

## Usage

### monitor-s3-object-versions

```monitor-s3-object-versions```

* Reads ```monitor.buckets``` for a list of buckets
* Creates a ```versions``` directory if it does not exist
* Initializes the ```versions``` directory as a git repo
* For each bucket in monitor.buckets
  * Performs an aws s3api list-object-versions to ```versions/$bucket```
  * Checks if ```versions/$bucket``` has changed and adds the bucket to sync list
* Commits the list-objects files
* Creates ```sync.buckets``` with the list of buckets that changed
* Zero exit code when there are changes.  Non-zero when there are not (or failure)
