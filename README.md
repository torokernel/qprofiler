# QProfiler

## Introduction
QProfiler stands for **QEMU Profiler** and is a simple tool to profile a guest by measuring where the time is spent during its execution. QProfiler samples the cpu registers to count what are the most used functions. The tool does so by relying on the QMP interface. This procedure does not need any instrumentation in the guest.

## Example
The repo includes a simple example to profile the appliance **HelloWorld** which is based on Toro. To try it, you need to clone the repo, install qemu and then execute the following command:

```bash
kvm -nographic -vnc :0 -drive format=raw,file=HelloWorld.img -smp 1 -m 256 -qmp unix:./qmp-sock,server,nowait
```

In a different terminal, execute:

```bash
./qprofiler --path=./qmp-sock --duration=10 --frequency=0.05 --filename=HelloWorld
```

Note that you have to tell qprofiler the path to the socket previously created by QEMU. You will get something like:

```bash
       1 WRITE_PORTB at Arch.pas:303
      18 MOVE at system.pas:1981
```
## How to Use
Compile your multiboot kernel with **-gl** flag to add debug symbols. Then, run qemu by using the **-kernel YourKernel** option and add **-qmp unix:./qmp-sock,server,nowait** to redirect qmp to a socket. To lauch qprofile, execute:

```bash
./qprofiler --path=./qmp-sock --duration=10 --frequency=0.05 --filename=YouKernel
```

The line above tells qpprofiler to test during **10** seconds and to sample Qemu every **0.05** seconds. In addition, the line specifies in **path** the socket and in **filename** the binary. If everything worked well after 10 seconds, you are going to get something like:

```bash
       1 WRITE_PORTB at Arch.pas:303
      18 MOVE at system.pas:1981
```
This represents in which functions the 10 seconds have been elapsed.

## Limitations

For the moment, Qprofiler only samples the booting core.

## License

GPLv2
