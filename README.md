# Minio

## Deployment

```
docker service create \
	-p 9000:9000 \
        --name minio \
	--mount type=volume,source=minio-data,destination=/data \
	--mount type=volume,source=minio-config,destination=/root/.minio \
	--constraint 'node.labels.type == minio' \
	minio/minio server /data
```

## Usage examples

### Minio Client

For simplicity you could use a container, for different use cases see [the docs](https://docs.minio.io/docs/minio-client-quickstart-guide)

```bash
docker run -it --entrypoint=/bin/sh minio/mc
```

#### Configure it to access your minio server

The syntax is:

```bash
mc config host add <ALIAS> <YOUR-S3-ENDPOINT> <YOUR-ACCESS-KEY> <YOUR-SECRET-KEY> <API-SIGNATURE>
```

Example:

```bash
mc config host add mysuperhost http://10.0.0.3:9000 BKIKJAA5BMMU2RHO6IBB V7f1CwQqAcwo80UEIJEjc5gVQUSSx5ohQ9GSrr12 S3v4
```

#### List files in a bucket

```
mc ls mysuperhost/foldername/anotherfoldername
[2016-03-22 19:47:48 PDT]     0B my-bucketname/
[2016-03-22 22:01:07 PDT]     0B mytestbucket/
[2016-03-22 20:04:39 PDT]     0B mybucketname/
[2016-01-28 17:23:11 PST]     0B newbucket/
[2016-03-20 09:08:36 PDT]     0B s3git-test/
```

### s3cmd client

Note: same as minio client but another implementation (it is on yum, you know).

#### Install the client

```
yum install s3cmd
```


#### Configuration example

```
[default]
access_key = BKIKJAA5BMMU2RHO6IBB 
host_base = 10.0.0.3:9000
host_bucket = 10.0.0.3:9000
human_readable_sizes = True
secret_key = 7sCAIzhlxySqZDLL7YD3uJD7u
send_chunk = 65536
server_side_encryption = False
signature_v2 = False
use_https = False
```
###

Example:

```
export AWS_SECRET_ACCESS_KEY=/7sCAIzhlxySqZDLL7YD3uJD7
export AWS_ACCESS_KEY=I5AWJSQHMN7NECRN68CM
mkdir /mnt/mybucket
./goofys --endpoint http://10.0.0.3:9000 mybucket /mnt/mybucket
```

### PHP Client Example

TODO: example here, we have to use composer first.


