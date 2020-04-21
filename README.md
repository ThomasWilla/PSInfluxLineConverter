# Powershell to InfluxLine Converter
This is a PowerShell module to convert PowerShell outputs for Telegraf agent to the InfluxLine-Protocol. 
The Telegraf agents reads the converted outputs and send this to an InfluxDB.

This is a fork of the Powershell module [PowerShell-Influx](https://github.com/markwragg/PowerShell-Influx) from markwragg.

# Purpose

This module was written to allow metrics from different sources to be written to InfluxDB, for presentation via one or more Grafana Dashboards. By utilising this module, InfluxDB and Grafana you can create and populate interactive dashboards like these (note: sensitive data has been redacted from these screenshots):

<p align="center">
<img src="http://wragg.io/content/images/2018/02/Grafana-Example-2.png" height=200>  <img src="http://wragg.io/content/images/2018/02/Grafana-TFS-Build-Dashboard.png" height=200>
</p>


# Installation

Import the module by running:
```
Import-Module PSInfluxLineConverter -Scope CurrentUser
```

# Usage

There are 3 methods for writing to Influx currently implemented:

1. `Write-Influx` which uses the Influx REST API to submit metrics via TCP.
2. `Write-InfluxUDP` which transmits metrics to an Influx UDP listener.
3. `Write-StatsD` which transmits metrics via UDP to a StatsD listener (the StatsD listener can be enabled via the Influx Telegraf component -- see the Statsd Server section in the telegraf.conf).

For example:

```
#REST API
Write-Influx -Measure Server -Tags @{Hostname=$env:COMPUTERNAME} -Metrics @{Memory=50;CPU=10} -Database Web -Server http://myinflux.local:8086 -Verbose

#REST API with authentication
Write-Influx -Measure Server -Tags @{Hostname=$env:COMPUTERNAME} -Metrics @{Memory=50;CPU=10} -Database Web -Server http://myinflux.local:8086 -Credential (Get-Credential) -Verbose
 
#Influx UDP
Write-InfluxUDP -Measure Server -Tags @{Hostname=$env:COMPUTERNAME} -Metrics @{Memory=50;CPU=10} -Database Web -IP 1.2.3.4 -Port 8089 -Verbose
 
#StatsD UDP
Write-Statsd "Server.CPU,Hostname=$($env:COMPUTERNAME):10|g" -IP 1.2.3.4 -Port 8125 -Verbose
```

# Cmdlets

A full list of implemented cmdlets is provided below for your reference. Use `Get-Help <cmdlet name>` with these to learn more about their usage.

Cmdlet                       | Description
-----------------------------| --------------------------------------------------------------------
ConvertTo-Metric             | Converts the specified properties of any object to a metric object, which can then be easily transmitted to Influx by piping to one of the `Write-` cmdlets.
ConvertTo-InfluxLineString   | Convert metrics to the Influx line protocol format, output as strings.
