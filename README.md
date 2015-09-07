# ibmetrics


You will need to transfer the script to all the nodes/headnodes and place it the same location or use /cm/shared.

You will first need to add this collection of metrics to CMDaemon:
```
[root@demo ~]# cmsh
[demo]% monitoring
[demo->monitoring]% metrics
[demo->monitoring->metrics]% add ibmetric
[demo->monitoring->metrics*[ibmetric*]]% set classofmetric  prototype
[demo->monitoring->metrics*[ibmetric*]]% show
Parameter                      Value
------------------------------ ------------------------------------------------
Class of metric                prototype
Command
Cumulative                     no
Description
Disabled                       no
Extended environment           no
Maximum                        <range not set>
Metric Unit
Minimum                        <range not set>
Name                           ibmetric
Notes                          <0 bytes>
Only when idle                 no
Parameter permissions          optional
Retrieval method               cmdaemon
Revision
Sampling method                samplingonnode
State flapping count           7
Timeout                        5
Valid for                      node,headnode
[demo->monitoring->metrics*[ibmetric*]]% set command /cm/shared/custom_metrics/ibmetric (you need to create this directory)
[demo->monitoring->metrics*[ibmetric*]]% commit
```

Then you will need to enable this metric collection for the node category ('default' in the example below):
```
[demo]% monitoring
[demo->monitoring]% setup
[demo->monitoring->setup]% use default
[demo->monitoring->setup[default]]% metricconf
[demo->monitoring->setup[default]->metricconf]% add ibmetric
[demo->monitoring->setup*[default*]->metricconf*[ibmetric*]]% show
Parameter                      Value
------------------------------ ------------------------------------------------
Consolidators                  <3 in submode>
Disabled                       no
GapThreshold                   2
LogLength                      3000
Metric                         ibmetric
MetricParam
Only when idle                 no
Revision
Sampling Interval              120
Stateflapping Actions
Store                          yes
ThresholdDuration              1
Thresholds                     <0 in submode>
[demo->monitoring->setup*[default*]->metricconf*[ibmetric*]]% commit
[demo->monitoring->setup[default]->metricconf[ibmetric]]%
Mon Mar 17 10:39:47 2014 [notice] node001: Added metric 'PortXmitData.1'
Added metric 'PortRcvData.1'
Added metric 'PortXmitPkts.1'
Added metric 'PortRcvPkts.1'
Will try to add metric 'PortXmitData.1' to category 'default'
Will try to add metric 'PortRcvData.1' to category 'default'
Will try to add metric 'PortXmitPkts.1' to category 'default'
Will try to add metric 'PortRcvPkts.1' to category 'default'
Success!
Summary for initializing metric collections. Added 4 new metrics, added 4 metrics to category
```
For every port you will see a different metric e.g. PortXmitData.1 and PortXmitData.2 etc.

You can use:
```
cmsh -c "device use node001; sample *"
```
to sample the current values.

![Image of Yaktocat](https://cray.panix.eu/ST6XC9-2.PNG)
