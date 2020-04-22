# Powershell to InfluxLine Converter
This is a lightweight PowerShell module to convert PowerShell outputs for Telegraf agent to the InfluxLine-Protocol. 
Then the Telegraf agents reads the converted outputs and send this to an InfluxDB.

_This is a fork of the PowerShell module [PowerShell-Influx](https://github.com/markwragg/PowerShell-Influx) from markwragg._
_All unused code are removed to minimize the overhead and optimize the performance._


# Installation
Import the module by running:
```
Import-Module PSInfluxLineConverter -Scope CurrentUser
```

# Usage
**Example: Converts the output of "Get-Process" to the InfluxLine-Protocol**
```
# Content of PowerShell script "input.process.ps1"
Import-Module PSInfluxLineConverter
Get-Process | ConvertTo-Metric -Measure win_process -MetricProperty CPU,PagedmemorySize,Handles -TagProperty Id,ProcessName | ConvertTo-InfluxLineString
```

```
# Content of Telegraf configuration "inputs.exec.process.conf"
[[inputs.exec]]
commands = ['powershell -NoProfile -File "C:\Program Files\Telegraf\scripts\input.process.ps1"']
data_format = "influx"
timeout = "1m"
```


**Informations about the example**
* Measurement = win_process
* Tags = Id, ProcessName
* Metric/Fields = CPU, PagedmemorySize, Handles



# Cmdlets
A full list of implemented cmdlets is provided below for your reference. Use `Get-Help <cmdlet name>` with these to learn more about their usage.

Cmdlet                       | Description
-----------------------------| --------------------------------------------------------------------
ConvertTo-Metric             | Converts the specified properties of any object to a metric object, which can then be easily transmitted to Influx.
ConvertTo-InfluxLineString   | Convert metrics to the InfluxLine protocol format, output as strings.



## License
Copyright 2020 Robin Hermann
PSInfluxLineConverter Copyright (C) 2020 Robin Hermann
This program comes with ABSOLUTELY NO WARRANTY; for details type `show w'.
This is free software, and you are welcome to redistribute it
under certain conditions; type `show c' for details.