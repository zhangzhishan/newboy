---
title: Powershell is the best
tag: powershell
date: 2019-03-26 09:36:55
---

[A good document with many expamles](https://ss64.com/ps/syntax.html)

use it to do some quick jobs.

`Get-Content ./log.log -Wait -Tail 10`
## Cmdlet Format
A cmdlet always consists of a verb (or a word that functions as a verb) and a noun, separated with a hyphen (the “verb-noun” rule). For example, some of the verbs include:

* Get — To get something
* Set — To define something
* Start — To run something
* Stop — To stop something that is running
* Out — To output something
* New — To create something (“new” is not a verb, of course, but it functions as one)

## Get system information

[Get-Counter (Microsoft.PowerShell.Diagnostics)](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/get-counter?view=powershell-5.1)

some examples:

```ps
Get-Counter -Counter "\Processor(_Total)\% Processor Time" -Continuous
get-counter -Counter "\Processor(*)\% Processor Time" -SampleInterval 2 -MaxSamples 10
get-counter -ListSet *
get-counter -listset * | sort-object countersetname | format-table countersetname
(get-counter -listset memory).paths
(get-counter -listset memory).paths | where {$_ -like "*cache*"}
```

Save the Disk Reads/sec counter path in the $diskreads variable..  Pipe the counter path (in $diskreads) to Get-Counter:
```ps
$diskreads = "\LogicalDisk(C:)\Disk Reads/sec"
$diskreads | get-counter -computer Server64, Server65 -maxsamples 10
```

Get a list of 50 randomly selected servers from the Servers.txt file and store in a variable: Save a counter path in the $Counter variable and then get the counter values for all the servers. 
```ps
$servers = get-random (get-content servers.txt) -count 50
$counter = "\Processor(*)\% DPC Time"
get-counter -Counter $counter -computername $servers
```

Get a single value for all of the performance counters in the memory counter set on the local computer. First get the counter paths and then get the counter data for each counter:
```ps
$memCounters = (get-counter -list memory).paths
get-counter -counter $memCounters
```

The properties of the CounterSamples object can be used to examine, select, sort, and group data:
```ps
$counter = "\\SERVER64\Process(Idle)\% Processor Time" 
$data = get-counter $counter
$data.countersamples | format-list -property *
```

Run a Get-Counter command as background job.
```ps
$counters = "\LogicalDisk(_Total)\% Free Space"
start-job -scriptblock {get-counter -counter $counters -maxsamples 1000)
```

Find the '% free disk space' on 50 computers selected randomly from Servers.txt
```ps
get-counter -computername (get-random servers.txt -count 50) -counter "\LogicalDisk(*)\% Free Space"
```

### K drive speed

```ps
PS D:\> (get-counter "\LogicalDisk(K:)\Disk Reads/sec" -SampleInterval 1 -MaxSamples 2).CounterSamples | Out-File .\test.txt
PS D:\> (get-counter "\LogicalDisk(K:)\Disk Read Bytes/sec" -SampleInterval 1 -MaxSamples 2).CounterSamples | Out-File .\test.txt
```
### Network stream

```ps
Get-Counter "\Network Adapter(*)\Bytes Received/sec"
Get-Counter "\Network Adapter(*)\Bytes Sent/sec"
Get-Counter "\Network Adapter(*)\Bytes Received/sec" -Continuous | Out-File .\receivedbytes.txt
Get-Counter "\Network Adapter(*)\Bytes Sent/sec" -Continuous | Out-File .\sentbytes.txt
```

### Environment info
```ps
[Environment] | gm -s
[Environment]::ProcessorCount
```

## start-transcript
`yyyyMMddHHmmss` 为其时间格式

Get-ChildItem cmdlet can be used to get files or folders list

an example

```ps
$path = "k:\Index\1\"
# $path = "D:\zhizha\tmp\"

$files = Get-ChildItem $path -file

foreach ($file in $files) {
    $filename = $file.Name
    $fullname = $path + $filename
    $timeRecordName = "D:\zhizha\" + $filename + "runwstime.txt"
    $parsedIndexName = "D:\zhizha\parsedindex\" + $filename + ".bin"

    Start-Transcript $timeRecordName

    Write-Output $fullname "satasics results:"
    Write-Output ""
    Write-Output ""

    .\ParseIndexzhizha.exe $fullname ws
   # .\ParseIndexzhizha.exe $fullname allword $parsedIndexName

    Stop-Transcript
	Write-Output ""
}
```
## Pipes
A pipe passes data from one cmdlet to another. I used a pipe earlier to get all properties of an object.

## For loop
