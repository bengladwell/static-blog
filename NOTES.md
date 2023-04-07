# Notes

## Run bengladwell.com locally

```sh
$ docker run --rm --name ghost-local -e NODE_ENV=development -e database__connection__filename='/var/lib/ghost/content/data/ghost.db' -p 3001:2368 -v $PWD/data/:/var/lib/ghost/content/data/ -v $PWD/content/images/:/var/lib/ghost/content/images/ ghost:5
```

## Generate static site

```sh
$ node_modules/.bin/gssg --url https://bengladwell.com
```

## Create cloudformation stack

```sh
$ aws cloudformation create-stack --stack-name static-blog  --template-body file://cloudformation/static-blog.yaml --parameters ParameterKey=BucketName,ParameterValue=bengladwell.com ParameterKey=DomainName,ParameterValue=bengladwell.com ParameterKey=HostedZoneId,ParameterValue=Z04446873P9XH28194IOF
```


## Sync locally generated files to S3

```sh
$ aws s3 sync --delete static/ s3://bengladwell.com
```
