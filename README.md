# fluentd custom

- timezone: Asia/Tokyo
- plugin: s3, slack

## run
```
docker run --env-file=./.env.docker --rm -it --name fluentd_s3_notice fluentd_s3_notice:latest
```

## tag

### use s3 save
tag
```
s3.<path>.nginx.error
```

s3 path
```
s3://<ENV:S3_BUCKET>/<TAG:path>/Y/m/d/Y-m-d-H_(index).${tag}.ext
```

### use slack post
tag
```
notice.<channel>.<app name, env...other>
```

slack
```
endpoint: <ENV:SLACK_WEBHOOK_URL>
channel: <TAG:channel>
message: "[timestamp] env:<TAG:app name> ```<message>```"
```

## other custom??
ex.) add chatwork plugin and copy
```
  <match notice.**>
    @type copy
    <store>
      @type relabel
      @label @slack
    </store>
+   <store>
+     @type relabel
+     @label @chatwork
+   </store>
  </match>
```


