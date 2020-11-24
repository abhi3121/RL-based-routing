# RL-based routing

## Table Of contents:-

- [BookSim](#booksim-interconnection-network-simulator)
- [Download](#downloading)
- [Build](#building)
- [Running](#running)
- [ConFig File](#config-file)
- [Routing Algorithms](#routing-algorithms)
- [Topologies](#topologies)

---

## BookSim Interconnection Network Simulator

BookSim is a cycle-accurate interconnection network simulator.
Originally developed for and introduced with the [Principles and Practices of Interconnection Networks](http://cva.stanford.edu/books/ppin/) book, its functionality has since been continuously extended.
The current major release, BookSim 2.0, supports a wide range of topologies such as mesh, torus and flattened butterfly networks, provides diverse routing algorithms and includes numerous options for customizing the network's router microarchitecture.

## Downloading

We have already provided the prebuild copy of booksim with all valid licencing and to download it just download the zip file or use below code
```bash
$ git clone https://github.com/Varun-Rathore/Minor-2/
```
Although if someone wants the fresh or update copy then latest official release of the simulator can be checked out from our subversion (SVN) repository. Make sure a subversion client is installed on your machine; under UNIX, you can use the following command to check out a working copy:
```bash
$ svn co https://nocs.stanford.edu/cgi-bin/svn.cgi/booksim2.0
```
## Building

The simulator itselfis written in C++ and has been specifically tested with the GNU G++ compiler (version≥3).The front end of the simulator uses LEX and YACC generated parser to process the simulatorconfiguration file; however, unless you plan on making changes to the front end parser, LEX andYACC are not needed. TheMakefileshould be edited so that the first few lines reflect the correctpaths to the tools for your particular system. The defaultMakefileshould work on the StanfordLeland machines. The below code will build the simulator
```bash
$ make
```
## Running 

The simulator is invoked using the following command line:
```bash
$ ./booksim [configfile]
```
The parameter configfile is a file that contains configuration information for the simulator. 

## Config File

All information used to configure a simulation is passed through a configuration file. This section lists the major configuration parameters for the simulator.Additional description can be found in the configuration parameter class file booksimconfig.cpp. A user can incorporate additional options by changing the this file. An example of config file is ilustrated below:-
```
topology = mesh;
k = 8;
n = 2;
// Routingrouting_function = dor;
// Flow controlnum_vcs     = 8;
vc_buf_size = 8;
wait_for_tail_credit = 1;
// Router architecturevc_allocator = islip;
sw_allocator = islip;
alloc_iters  = 1;
credit_delay   = 2;
routing_delay  = 0;
vc_alloc_delay = 1;
sw_alloc_delay = 1;
input_speedup  = 2;
output_speedup = 1;
internal_speedup = 1.0;
// Traffic
traffic = transpose;
packet_size = 20;
// Simulation
sim_type = latency;
injection_rate = 0.005;
```
## Routing algorithms 

The routing function parameter selects a routing algorithm for the topology. Many routing algo-rithms need multiple virtual channels for deadlock freedom. In addition to routefunc.cpp, some topologies source files include additional routing functions. Also, the simulator code is structured so that additional routing algorithms can be added with minimal changes to the overall simulator(see theroutefunc.cppfile in the simulator’s source code).

## Topologies

The topology parameter determines the underlying topology of the network. There is also a set of numerical parameters that describes the size of the networks.
```
k     Network radix, the number of routers per dimension
n     Network dimension
c     Network concentration,the number of nodes sharing a single router. Typically set to 1,¿1 only in networks that has concentration (i.e. cmesh).
x     (NoC simulations only) The number of routers in the X dimension. Used to calculate channel latency between routers.
y     (NoC simulations only) The number of routers in the y dimension. Used to calculate channel latency between routers.
xr    (NoC simulations only) For networks that have c¿1, the number of nodes in the x direction per router. Used to calculate channel latency between routers.
yr    (NoC simulations only) For networks that have c¿1, the number of nodes in the ydirection per router. Used to calculate channel latency between routers.
```
The channel latency of the network must be configured within the source code of the topology files. All topologies by default have channel latency of 1 cycles. Topologies available in BookSim are:
```
fly                 A k-ary n-fly (butterfly) topology. The k parameter determines the network’s radixand thenparameter determines the network’s dimension.  Note: ak-ary 1-fly is essentially a single radix-k router, useful for testing.
mesh                A k-ary n-mesh (mesh) topology. The k parameter determines the network’s radixand thenparameter determines the network’s dimension.
torus               A k-ary n-cube (torus) topology. The k parameter determines the network’s radixand thenparameter determines the network’s dimension.
cmesh               Concentrated mesh topology is a A k-ary n-mesh topology with multiple nodes sharinga single router. The c determines the concentration. The cmesh topology has the option that turns on ”express channels” as described in by default these channels areturned off.
fat tree            Fat Tree topology with 3 levels. Nodes are routers are arranged in a tree structurebut the number of links between levels stays constant. At the bottom k nodes shares a level 0 router.
flattened butterfly A topology based on the paper ”Flattened butterfly: a cost-efficient topol-ogy for high-radix networks” ISCA 2007
dragonfly           A topology based on the paper “Technology-driven, highly-scalable dragonfly topol-ogy.” ISCA 2008
quad tree           A quad tree topology.tree 4anynetA topology based on an user input file specifying connectivity of nodes and routers.
```
---
