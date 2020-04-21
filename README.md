# Powershell to InfluxLine Converter
This is a PowerShell module to convert PowerShell outputs for Telegraf agent to the InfluxLine-Protocol. 
The Telegraf agents reads the converted outputs and send this to an InfluxDB.

_This is a fork of the Powershell module [PowerShell-Influx](https://github.com/markwragg/PowerShell-Influx) from markwragg._


# Installation
Import the module by running:
```
Import-Module PSInfluxLineConverter -Scope CurrentUser
```

# Usage
For example:
##Converts the output of "Get-Process" to the InfluxLine-Protocol
```
Import-Module PSInfluxLineConverter
Get-Process | ConvertTo-Metric -Measure win_processes -MetricProperty CPU,PagedmemorySize,Handles -TagProperty Id,ProcessName | ConvertTo-InfluxLineString
```
**Informations about the example**
* Measurement = win_process
* Metric/Fields = CPU, PagedmemorySize, Handles
* Tags = Id, ProcessName


# Cmdlets
A full list of implemented cmdlets is provided below for your reference. Use `Get-Help <cmdlet name>` with these to learn more about their usage.

Cmdlet                       | Description
-----------------------------| --------------------------------------------------------------------
ConvertTo-Metric             | Converts the specified properties of any object to a metric object, which can then be easily transmitted to Influx by piping to one of the `Write-` cmdlets.
ConvertTo-InfluxLineString   | Convert metrics to the Influx line protocol format, output as strings.
