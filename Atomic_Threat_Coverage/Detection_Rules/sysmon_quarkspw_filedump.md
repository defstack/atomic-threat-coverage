| Title                | QuarksPwDump Dump File                                                                                                                                                 |
|:---------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description          | Detects a dump file written by QuarksPwDump password dumper                                                                                                                                           |
| ATT&amp;CK Tactic    | <ul></ul>  |
| ATT&amp;CK Technique | <ul></ul>                             |
| Data Needed          | <ul><li>[DN_0015_11_windows_sysmon_FileCreate](../Data_Needed/DN_0015_11_windows_sysmon_FileCreate.md)</li></ul>                                                         |
| Trigger              |  There is no Trigger for this technique yet.  |
| Severity Level       | critical                                                                                                                                                 |
| False Positives      | <ul><li>Unknown</li></ul>                                                                  |
| Development Status   | experimental                                                                                                                                                |
| References           | <ul><li>[https://jpcertcc.github.io/ToolAnalysisResultSheet/details/QuarksPWDump.htm](https://jpcertcc.github.io/ToolAnalysisResultSheet/details/QuarksPWDump.htm)</li></ul>                                                          |
| Author               | Florian Roth                                                                                                                                                |


## Detection Rules

### Sigma rule

```
title: QuarksPwDump Dump File
status: experimental
description: Detects a dump file written by QuarksPwDump password dumper
references:
    - https://jpcertcc.github.io/ToolAnalysisResultSheet/details/QuarksPWDump.htm
author: Florian Roth
date: 2018/02/10
level: critical
logsource:
    product: windows
    service: sysmon
detection:
    selection:
        # Sysmon: File Creation (ID 11)
        EventID: 11
        TargetFilename: '*\AppData\Local\Temp\SAM-*.dmp*'
    condition: selection
falsepositives:
    - Unknown


```





### Kibana query

```
(EventID:"11" AND TargetFilename.keyword:*\\\\AppData\\\\Local\\\\Temp\\\\SAM\\-*.dmp*)
```





### X-Pack Watcher

```
curl -s -XPUT -H \'Content-Type: application/json\' --data-binary @- localhost:9200/_xpack/watcher/watch/QuarksPwDump-Dump-File <<EOF\n{\n  "trigger": {\n    "schedule": {\n      "interval": "30m"\n    }\n  },\n  "input": {\n    "search": {\n      "request": {\n        "body": {\n          "size": 0,\n          "query": {\n            "query_string": {\n              "query": "(EventID:\\"11\\" AND TargetFilename.keyword:*\\\\\\\\AppData\\\\\\\\Local\\\\\\\\Temp\\\\\\\\SAM\\\\-*.dmp*)",\n              "analyze_wildcard": true\n            }\n          }\n        },\n        "indices": []\n      }\n    }\n  },\n  "condition": {\n    "compare": {\n      "ctx.payload.hits.total": {\n        "not_eq": 0\n      }\n    }\n  },\n  "actions": {\n    "send_email": {\n      "email": {\n        "to": null,\n        "subject": "Sigma Rule \'QuarksPwDump Dump File\'",\n        "body": "Hits:\\n{{#ctx.payload.hits.hits}}{{_source}}\\n================================================================================\\n{{/ctx.payload.hits.hits}}",\n        "attachments": {\n          "data.json": {\n            "data": {\n              "format": "json"\n            }\n          }\n        }\n      }\n    }\n  }\n}\nEOF\n
```





### Graylog

```
(EventID:"11" AND TargetFilename:"*\\\\AppData\\\\Local\\\\Temp\\\\SAM\\-*.dmp*")
```

