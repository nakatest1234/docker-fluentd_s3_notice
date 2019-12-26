# fluentd custom

## build env
| key | val |
| --- | --- |
| FLUENT_TZ | Asia/Tokyo |
| FLUENT_PLUGINS | fluent-plugin-s3:1.1.11 fluent-plugin-slack:0.6.7 fluent-plugin-record-reformer:0.9.1 |

## run
```
docker run --env-file=./.env.docker --rm -it --name fluentd_s3_notice fluentd_s3_notice:latest
```

## env
| key | val |
| --- | --- |
| LOG_LEVEL | fluentd log level. default: `info` |
| S3_AWS_KEY_ID | use s3: **required** |
| S3_AWS_SEC_KEY | use s3: **required** |
| S3_BUCKET | use s3: **required** |
| S3_REGION | default: `ap-northeast-1` |
| S3_STORE_AS | default: `text` |
| S3_INDEX_FORMAT | default: `%02d` |
| S3_TIME_SLICE_FORMAT | default: `%Y/%m/%d/%Y%m%d-%H` |
| S3_OBJECT_KEY_FORMAT | default: `%{path}/%{time_slice}_%{index}.${tag}.%{file_extension}` |
| S3_TOP | default: `default` |
|        | path=s3://`S3_BUCKET`/`S3_TOP`/`S3_OBJECT_KEY_FORMAT` |
| S3_TIMEKEY | default: `60m` |
| S3_TIMEKEY_WAIT | default: `1m` |
| S3_CHUNK_LIMIT_SIZE | default: `64m` |
| SLACK_WEBHOOK_URL | use slack: **required** |
| SLACK_FLUSH_INTERVAL | default: `10s` |
| SLACK_USERNAME | default: `fluentd` |
| SLACK_ICON | default: `:heavy_exclamation_mark:` |

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

need key
```
message
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


