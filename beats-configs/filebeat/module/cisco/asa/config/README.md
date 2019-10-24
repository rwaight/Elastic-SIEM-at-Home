See [this comment](https://github.com/elastic/beats/issues/6872#issuecomment-516577472) in beats [GitHub issue 6872](https://github.com/elastic/beats/issues/6872) for reference.

The updated configuration file was created to resolve the `ERROR [syslog] syslog/input.go:132 can't parse event as syslog rfc3164` message for the Cisco module

### Quote from the comment:
If it helps, while troubleshooting an `ERROR   [syslog]        syslog/input.go:132     can't parse event as syslog rfc3164` error for the `cisco` module; I found that using the workaround mentioned in [this discussion comment](https://discuss.elastic.co/t/filebeat-syslog-parse-error/189643/9) allows Filebeat to parse Cisco ASA logs.  

For clarification, the `module/cisco/asa/config/input.yml` file is modified from this:
```
{{ if eq .input "syslog" }}

type: syslog
protocol.udp:
  host: "{{.syslog_host}}:{{.syslog_port}}"
```

To this:
```
{{ if eq .input "syslog" }}

type: udp
host: "{{.syslog_host}}:{{.syslog_port}}"
```
