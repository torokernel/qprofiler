# QProfiler

## Introduction

QProfiler stands for **QEMU Profiler** and is a tool to profile a guest by measuring where the time is spent during its execution. QProfiler samples the cpu registers to count what are the most used functions. The tool does so by relying on the QMP interface. This procedure does not need any instrumentation in the guest.      

## How to Use

Compile your multiboot kernel with **-gl** flag to add debug symbols. Then, run qemu by using the **-kernel YourKernel** option and add **-qmp unix:./qmp-sock,server,nowait** to redirect qmp to a socket. To lauch qprofile, execute:     

`./qprofiler --path=./qmp-sock --duration=10 --frecuency=0.05 --filename=YouKernel`

The line above tells qpprofiler to test during **10** seconds and to sample Qemu every **0.05** seconds. In addition, the line specifies in **path** the socket and in **filename** the binary. If everything worked well after 10 seconds, you are going to get something like:

`87 % of time  HLT at Arch.pas:1444 has been executed`

`11 % of time  GETAPICID at Arch.pas:411 has been executed`

`0 % of time   GETCURRENTTHREAD at Process.pas:897 has been executed`

This represents in which functions the 10 seconds has been elapsed.

## Limitations

Qprofiler only samples the booting core.

## License

GPLv3
