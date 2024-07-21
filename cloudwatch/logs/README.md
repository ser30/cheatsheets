## CloudWatch Logs Insights

- https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax-examples.html

Loglines:

```
172.31.37.134 - - [07/Jul/2020 13:18:34] "GET / HTTP/1.1" 200 -
172.31.37.134 - - [07/Jul/2020 13:18:34] "GET /status HTTP/1.1" 200 -
```

Show all logs:

```
fields @message
```

Show the 25 most recent log entries:

```
fields @timestamp, @message | sort @timestamp desc | limit 25
```

Show all logs and include parsed fields:

```
fields @message, @log, @logStream, @ingestionTime, @timestamp
```

Only show logs containing `/status`:

```
fields @message | filter @message like '/status'
```

View eks audit logs for delete verbs:

```
fields @timestamp, @message, @logStream, @log
| filter objectRef.namespace = 'dev' and objectRef.resource like /service.*/ and verb = 'delete'
| sort @timestamp desc
| limit 20
```

Select the logstream and filter on a string content:

```
fields @timestamp, @message, @logStream
| sort @timestamp desc 
| filter @logStream = 'cb2a300000000000000000003b3' 
| filter @message like 'msg='
```

Select the logstream and filter out string content:

```
fields @timestamp, @message, @logStream | sort @timestamp desc 
| filter @logStream = 'cb2a300000000000000000003b3' 
| filter @message not like "Something I dont want to see"
```

Filter out multiple strings:

```
fields @timestamp, @message, @logStream | sort @timestamp desc 
| filter @logStream = 'cb2a300000000000000000003b3'  
  and not (
    @message like "Something I dont want to see" or
    @message like "also dont want to see this" or
    @message like "or even this"
  )
```
