# Static Timing Analysis
![types of setup hold](https://user-images.githubusercontent.com/110485513/191718450-d1d1df78-db40-4ccc-a8ab-883f295a8bfc.png)
* Time Graph

![timgph](https://user-images.githubusercontent.com/110485513/191718720-b33d3cc2-1b64-424f-9584-ae4777b4fbab.png)

* Slack Compute

![slack compute](https://user-images.githubusercontent.com/110485513/191718781-9fe682df-f4f4-4f0f-be82-1e1239322568.png)

* Pin Node Conventions
![pin node conv](https://user-images.githubusercontent.com/110485513/191719326-4516a0b8-21a4-4d47-85b8-818aaa31c830.png)


* Jitter
![jitter](https://user-images.githubusercontent.com/110485513/191719090-e37a9efe-005e-4db2-83f0-ab550cb05da0.png)

* Generating Jitter Values (EYE DIAGRAM)
![eyediag](https://user-images.githubusercontent.com/110485513/191719699-a673fc0b-5514-47a3-ae78-2cd625c7acc5.png)



# OpenTimer
OpenTimer is a new static timing analysis (STA) tool to help IC designers quickly verify the circuit timing. It is developed completely from the ground up using C++17 to efficiently support parallel and incremental timing.
Key features are:
* Industry standard format (.lib, .v, .spef, .sdc) support
* Graph- and path-based timing analysis
* Parallel incremental timing for fast timing closure
* Award-winning tools and golden timers in CAD Contests
# Compile OpenTimer
## System Requirements
OpenTimer is very self-contained and has very few dependencies. 
To compile OpenTimer, you need a C++17 compiler and currently support:

* GNU C++ Compiler v7.3 with -std=c++1z
* Clang C++ Compiler v6.0 with -std=c++17
In addition, you need a tcl shell interpreter:
* tclsh (most Unix/Linux/OSX distributions already include tclsh)
OpenTimer has been tested to run well on Linux distributions and MAC OSX.
## Build through CMake
We use CMake to manage the source and tests. We recommend using out-of-source build.
```
$ git clone https://github.com/OpenTimer/OpenTimer.git
$ cd OpenTimer
$ mkdir build
$ cd build
$ cmake ../
$ apt install cmake --classic (If it shows that cmake is not installed already)
$ make 
```
After successful build, you can find binaries and libraries in the folders ```bin and lib```, respectively.
## Run Tests
OpenTimer uses Doctest for unit tests and TAU15 benchmarks for integration/regression tests. These benchmarks are generated by an industry standard timer and are being used by many EDA researchers.
```
$ make test
```
# Get Started with OpenTimer
```
$ ./bin/ot-shell
```

![OpenTimer](https://user-images.githubusercontent.com/110485513/190913917-dcbde034-274a-463f-98d4-f24976c89d79.png)

# Design Philosophy
OpenTimer has a unique software architecture to efficiently enable parallel incremental timing. We draw two levers on performance and useability by grouping each timing operation to one of the three categories, builder, action, and accessor.

![image](https://user-images.githubusercontent.com/110485513/190917688-52aa4f48-db6a-4f5c-b738-c5b0ef1f51d7.png)

## Builder: OpenTimer Lineage
OpenTimer maintains a lineage graph of builder operations to create a task execution plan (TEP). A TEP starts with no dependency and keeps adding tasks to the lineage graph every time you call a builder operation. It records what transformations need to be executed when an action has been called.

![image](https://user-images.githubusercontent.com/110485513/190917926-aed85927-907b-4768-b430-4a29dbb3399b.png)

The above figure shows an example lineage graph of a sequence of builder operations. The cyan path is the main lineage line with additional tasks attached to enable parallel execution. OpenTimer use Cpp-Taskflow to create dependency graphs.

## Action: Update Timing
A TEP is materialized and executed when the timer is requested to perform an action operation. Each action operation triggers timing update from the earliest task to the one that produces the result of the action call. Internally, OpenTimer creates task dependency graph to update timing in parallel, including forward (slew, arrival time) and backward (required arrival time) propagations.

## Accessor: Inspect OpenTimer
The accessor operations let you inspect the timer status and dump timing information. All accessor operations are declared as const methods in the timer class. Calling them promises not to alter any internal members. For example, you can dump the timing graph into a DOT format and use tools like GraphViz for visualization.

# OpenTimer Shell
OpenTimer shell is a powerful command line tool to perform interactive analysis. It is also the easiest way to get your first timing report off the ground. The program ot-shell can be found in the folder bin/ after you Compile OpenTimer.
## Commands
The table below shows a list of commonly used commands.

![image](https://user-images.githubusercontent.com/110485513/190918528-7decda77-0a71-4056-a8f2-fb648deb6832.png)

# OpenTimer C++ API
The class Timer is the main entry you need to call OpenTimer in your project. The table below summarizes a list of commonly used methods.

![image](https://user-images.githubusercontent.com/110485513/190918656-41e9a46a-f624-4042-bfd9-51927cfa92a3.png)

# Contributors
* Aashish Tiwary
* Kunal Ghosh
# Acknowledgments
* Kunal Ghosh, Director, VSD Corp. Pvt. Ltd.
* Dr. Tsung-Wei Huang and Dr. Martin Wong (The University of Illinois at Urbana-Champaign, IL, USA)


# Contact Information
* Aashish Tiwary, Research Scholar, International Institute of Information Technology, Bangalore aashish.tiwary@iiitb.ac.in 
* Kunal Ghosh, Director, VSD Corp. Pvt. Ltd. kunalghosh@gmail.com


