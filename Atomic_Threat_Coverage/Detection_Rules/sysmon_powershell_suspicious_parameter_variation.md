| Title                | Suspicious PowerShell Parameter Substring                                                                                                                                                 |
|:---------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description          | Detects suspicious PowerShell invocation with a parameter substring                                                                                                                                           |
| ATT&amp;CK Tactic    | <ul><li>[TA0002: Execution](https://attack.mitre.org/tactics/TA0002)</li></ul>  |
| ATT&amp;CK Technique | <ul><li>[T1086: PowerShell](https://attack.mitre.org/techniques/T1086)</li></ul>                             |
| Data Needed          | <ul><li>[DN_0003_1_windows_sysmon_process_creation](../Data_Needed/DN_0003_1_windows_sysmon_process_creation.md)</li></ul>                                                         |
| Trigger              | <ul><li>[T1086: PowerShell](../Triggers/T1086.md)</li></ul>  |
| Severity Level       | high                                                                                                                                                 |
| False Positives      | <ul><li>Penetration tests</li></ul>                                                                  |
| Development Status   | experimental                                                                                                                                                |
| References           | <ul><li>[http://www.danielbohannon.com/blog-1/2017/3/12/powershell-execution-argument-obfuscation-how-it-can-make-detection-easier](http://www.danielbohannon.com/blog-1/2017/3/12/powershell-execution-argument-obfuscation-how-it-can-make-detection-easier)</li></ul>                                                          |
| Author               | Florian Roth (rule), Daniel Bohannon (idea), Roberto Rodriguez (Fix)                                                                                                                                                |


## Detection Rules

### Sigma rule

```
title: Suspicious PowerShell Parameter Substring
status: experimental
description: Detects suspicious PowerShell invocation with a parameter substring
references:
    - http://www.danielbohannon.com/blog-1/2017/3/12/powershell-execution-argument-obfuscation-how-it-can-make-detection-easier
tags:
    - attack.execution
    - attack.t1086
author: Florian Roth (rule), Daniel Bohannon (idea), Roberto Rodriguez (Fix)
logsource:
    product: windows
    service: sysmon
detection:
    selection:
        Image:
            - '*\Powershell.exe'
        EventID: 1
        CommandLine:
            - ' -windowstyle h '
            - ' -windowstyl h'
            - ' -windowsty h'
            - ' -windowst h'
            - ' -windows h'
            - ' -windo h'
            - ' -wind h'
            - ' -win h'
            - ' -wi h'
            - ' -win h '
            - ' -win hi '
            - ' -win hid '
            - ' -win hidd '
            - ' -win hidde '
            - ' -NoPr '
            - ' -NoPro '
            - ' -NoProf '
            - ' -NoProfi '
            - ' -NoProfil ' 
            - ' -nonin '
            - ' -nonint '
            - ' -noninte '
            - ' -noninter '
            - ' -nonintera '
            - ' -noninterac '
            - ' -noninteract '
            - ' -noninteracti '
            - ' -noninteractiv '
            - ' -ec '
            - ' -encodedComman '
            - ' -encodedComma '
            - ' -encodedComm '
            - ' -encodedCom '
            - ' -encodedCo '
            - ' -encodedC '
            - ' -encoded '
            - ' -encode '
            - ' -encod '
            - ' -enco '
            - ' -en '
    condition: selection
falsepositives:
    - Penetration tests
level: high

```





### Kibana query

```
(Image.keyword:(*\\\\Powershell.exe) AND EventID:"1" AND CommandLine:("\\ \\-windowstyle\\ h\\ " "\\ \\-windowstyl\\ h" "\\ \\-windowsty\\ h" "\\ \\-windowst\\ h" "\\ \\-windows\\ h" "\\ \\-windo\\ h" "\\ \\-wind\\ h" "\\ \\-win\\ h" "\\ \\-wi\\ h" "\\ \\-win\\ h\\ " "\\ \\-win\\ hi\\ " "\\ \\-win\\ hid\\ " "\\ \\-win\\ hidd\\ " "\\ \\-win\\ hidde\\ " "\\ \\-NoPr\\ " "\\ \\-NoPro\\ " "\\ \\-NoProf\\ " "\\ \\-NoProfi\\ " "\\ \\-NoProfil\\ " "\\ \\-nonin\\ " "\\ \\-nonint\\ " "\\ \\-noninte\\ " "\\ \\-noninter\\ " "\\ \\-nonintera\\ " "\\ \\-noninterac\\ " "\\ \\-noninteract\\ " "\\ \\-noninteracti\\ " "\\ \\-noninteractiv\\ " "\\ \\-ec\\ " "\\ \\-encodedComman\\ " "\\ \\-encodedComma\\ " "\\ \\-encodedComm\\ " "\\ \\-encodedCom\\ " "\\ \\-encodedCo\\ " "\\ \\-encodedC\\ " "\\ \\-encoded\\ " "\\ \\-encode\\ " "\\ \\-encod\\ " "\\ \\-enco\\ " "\\ \\-en\\ "))
```





### X-Pack Watcher

```
curl -s -XPUT -H \'Content-Type: application/json\' --data-binary @- localhost:9200/_xpack/watcher/watch/Suspicious-PowerShell-Parameter-Substring <<EOF\n{\n  "trigger": {\n    "schedule": {\n      "interval": "30m"\n    }\n  },\n  "input": {\n    "search": {\n      "request": {\n        "body": {\n          "size": 0,\n          "query": {\n            "query_string": {\n              "query": "(Image.keyword:(*\\\\\\\\Powershell.exe) AND EventID:\\"1\\" AND CommandLine:(\\"\\\\ \\\\-windowstyle\\\\ h\\\\ \\" \\"\\\\ \\\\-windowstyl\\\\ h\\" \\"\\\\ \\\\-windowsty\\\\ h\\" \\"\\\\ \\\\-windowst\\\\ h\\" \\"\\\\ \\\\-windows\\\\ h\\" \\"\\\\ \\\\-windo\\\\ h\\" \\"\\\\ \\\\-wind\\\\ h\\" \\"\\\\ \\\\-win\\\\ h\\" \\"\\\\ \\\\-wi\\\\ h\\" \\"\\\\ \\\\-win\\\\ h\\\\ \\" \\"\\\\ \\\\-win\\\\ hi\\\\ \\" \\"\\\\ \\\\-win\\\\ hid\\\\ \\" \\"\\\\ \\\\-win\\\\ hidd\\\\ \\" \\"\\\\ \\\\-win\\\\ hidde\\\\ \\" \\"\\\\ \\\\-NoPr\\\\ \\" \\"\\\\ \\\\-NoPro\\\\ \\" \\"\\\\ \\\\-NoProf\\\\ \\" \\"\\\\ \\\\-NoProfi\\\\ \\" \\"\\\\ \\\\-NoProfil\\\\ \\" \\"\\\\ \\\\-nonin\\\\ \\" \\"\\\\ \\\\-nonint\\\\ \\" \\"\\\\ \\\\-noninte\\\\ \\" \\"\\\\ \\\\-noninter\\\\ \\" \\"\\\\ \\\\-nonintera\\\\ \\" \\"\\\\ \\\\-noninterac\\\\ \\" \\"\\\\ \\\\-noninteract\\\\ \\" \\"\\\\ \\\\-noninteracti\\\\ \\" \\"\\\\ \\\\-noninteractiv\\\\ \\" \\"\\\\ \\\\-ec\\\\ \\" \\"\\\\ \\\\-encodedComman\\\\ \\" \\"\\\\ \\\\-encodedComma\\\\ \\" \\"\\\\ \\\\-encodedComm\\\\ \\" \\"\\\\ \\\\-encodedCom\\\\ \\" \\"\\\\ \\\\-encodedCo\\\\ \\" \\"\\\\ \\\\-encodedC\\\\ \\" \\"\\\\ \\\\-encoded\\\\ \\" \\"\\\\ \\\\-encode\\\\ \\" \\"\\\\ \\\\-encod\\\\ \\" \\"\\\\ \\\\-enco\\\\ \\" \\"\\\\ \\\\-en\\\\ \\"))",\n              "analyze_wildcard": true\n            }\n          }\n        },\n        "indices": []\n      }\n    }\n  },\n  "condition": {\n    "compare": {\n      "ctx.payload.hits.total": {\n        "not_eq": 0\n      }\n    }\n  },\n  "actions": {\n    "send_email": {\n      "email": {\n        "to": null,\n        "subject": "Sigma Rule \'Suspicious PowerShell Parameter Substring\'",\n        "body": "Hits:\\n{{#ctx.payload.hits.hits}}{{_source}}\\n================================================================================\\n{{/ctx.payload.hits.hits}}",\n        "attachments": {\n          "data.json": {\n            "data": {\n              "format": "json"\n            }\n          }\n        }\n      }\n    }\n  }\n}\nEOF\n
```





### Graylog

```
(Image:("*\\\\Powershell.exe") AND EventID:"1" AND CommandLine:(" \\-windowstyle h " " \\-windowstyl h" " \\-windowsty h" " \\-windowst h" " \\-windows h" " \\-windo h" " \\-wind h" " \\-win h" " \\-wi h" " \\-win h " " \\-win hi " " \\-win hid " " \\-win hidd " " \\-win hidde " " \\-NoPr " " \\-NoPro " " \\-NoProf " " \\-NoProfi " " \\-NoProfil " " \\-nonin " " \\-nonint " " \\-noninte " " \\-noninter " " \\-nonintera " " \\-noninterac " " \\-noninteract " " \\-noninteracti " " \\-noninteractiv " " \\-ec " " \\-encodedComman " " \\-encodedComma " " \\-encodedComm " " \\-encodedCom " " \\-encodedCo " " \\-encodedC " " \\-encoded " " \\-encode " " \\-encod " " \\-enco " " \\-en "))
```

