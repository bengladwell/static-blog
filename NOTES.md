# Notes

## Run bengladwell.com locally

```sh
$ docker run --rm --name ghost-local -e NODE_ENV=development -e database__connection__filename='/var/lib/ghost/content/data/ghost.db' -p 2368:2368 -v $PWD/data/:/var/lib/ghost/content/data/ -v $PWD/content/images/:/var/lib/ghost/content/images/ ghost:5
```

* login at `http://localhost:2368/ghost`
* create/update post

## Generate static site

```sh
$ rm -fr static/
$ node_modules/.bin/gssg --url https://www.bengladwell.com
```

## Sync locally generated files to S3

```sh
$ aws s3 sync --delete static/ s3://bengladwell.com
```

## Update all objects to have public read access

* in AWS S3 console, select all objects in the bucket
* Actions -> Make public using ACL

## Create cloudformation stack

* shouldn't be necessary; once deployed, this stack should work indefinitely

```sh
$ aws cloudformation create-stack --stack-name static-blog  --template-body file://cloudformation/static-blog.yaml --parameters ParameterKey=BucketName,ParameterValue=bengladwell.com ParameterKey=DomainName,ParameterValue=bengladwell.com ParameterKey=HostedZoneId,ParameterValue=Z04446873P9XH28194IOF
```


### Flush local DNS after updating /etc/hosts

```sh
$ dscacheutil -flushcache
```
