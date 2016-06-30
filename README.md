The timer runs every 15 minutes by default; edit
`journal-monitor.timer` before installing to change this.

Install by specifying the recipients of the monitoring emails, a
priority level (`emerg`, `alert`, `crit`, `err`, `warning`, `notice`,
`info`, or `debug`), and the systemd units to monitor. Multiple
recipients need single quotes around the comma-separated list:
```
sudo ./install 'recipient1@email.org, recipient2@email.org' warning unit1 unit2
```

Service messages written to stderr are logged to the systemd journal
at the `info` level by default. To log messages at a different
priority level, prefix the log messages with `<n>` where `n` is the
numeric code for a log level (`0` for `emerg`, `7` for `debug`).

Then start the timer:
```
sudo systemctl start journal-monitor.timer
```

You can check that the timer loaded properly:
```
systemctl list-timers
```

If you want the timer to be enabled at bootup:
```
sudo systemctl enable journal-monitor.timer
```

You can launch a single run of the journal monitor manually:
```
sudo systemctl start journal-monitor
```

The `test` directory contains a simple service (`fail.service`) that
does nothing but write to stderr, which can be used for testing the
journal monitor. The test service logs to the journal at the `warning`
(`4`) level.
