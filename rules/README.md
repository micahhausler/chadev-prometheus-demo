# Alerting Overview

This overview is to help you create meaningful alerts that are not overactive,
and conform to some common conventions.

## Rule Overview

First off, read through the [prometheus alerting rules](https://prometheus.io/docs/alerting/rules/)
page. Look over other rules in this directory to get a feel for the format. The
basic rule structure looks like this:

```
ALERT <alert name>
  IF <expression>
  [ FOR <duration> ]
  [ LABELS <label set> ]
  [ ANNOTATIONS <label set> ]
```

## Conventions

Alerts should contain the following labels:

* `serverity` - See the [Severity](#Severity) section below
* `service` - The corresponding service (if the alert is service related)
* `instance` - The corresponding instance (if the alert is instance related)

Alerts should also contain the following annotations

* `summary` - A short summary in English of why the alert fired

Rules should be straigtforward and easy to understand, however if you have a
complex query that requires explanation, please put a comment line above the
rule explaining what you are doing. Comment lines begin with `#`.

## Severity

Most alerts to humans should be `error` and above to aviod over-alerting and
pager-fatigue. This includes alerts to PagerDuty, emails, and individual Slack
emssages.

Alerts with severity from `info` to `warning` (inclusive) should primarily be made
to automated events, and on occassion to a Slack channel.

| Severity    | Description                              | Example                        |
|-------------|------------------------------------------|--------------------------------|
| `emergency` | System is unusable                       |                                |
| `critical`  | Critical condition                       | Failure in primary application |
| `error`     | Non-terminating error condition          | Out of disk space              |
| `warning`   | Error will probably occur without action | Low disk space                 |
| `notice`    | Unusual event                            | High incidence of HTTP 301/302 |
| `info`      | Normal operational event                 | System scaling operation       |
| `debug`     | Development debugging message            | Extra output for debugging     |

## Validation

To validate rule-files, use
[`promtool`](https://github.com/prometheus/prometheus#building-from-source).
You can install it with homebrew (`brew install prometheus`) or if you have `go`
installed:

```
GO15VENDOREXPERIMENT=1 go get github.com/prometheus/prometheus/cmd/...
```

Once installed, you can run it against your rule files.

```
$ promtool check-rules ./jenkins_alert.rule
Checking jenkins_alerts.rule
  SUCCESS: 3 rules found
```

