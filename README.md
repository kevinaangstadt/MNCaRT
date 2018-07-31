# MNCaRT
The MNRL Network Computation and Research Testbed (MNCaRT) is an open-source,
multi-architecture automata processing ecosystem. MNCaRT collects a diverse set
of automata processing tools and algorithms into a central location and will
grow as new tools are developed. Central to the ecosystem is MNRL (the MNRL
Network Representation Language), a JSON-based language for storing state
machines.

Software in the MNCaRT repository is pinned at a stable revision and may
therefore lag behind the individual repositories. If you would like to add new
functionality or update the revision of an included tool, please consider
contributing (see below)!

When cloning MNCaRT, don't forget to initialize and update the submodules:
```
git submodule update --init
```

## Tools Provided with MNCaRT
The following tools are included in the current release of MNCaRT:

- **MNRL**: a JSON schema specification of the language and APIs (Python and C++)
  for manipulating MNRL files (https://github.com/kevinaangstadt/MNRL).
  
- **ANMLZoo**: an automata processing benchmark suite containing a diverse set
  of finite automata and representative input stimuli
  (https://github.com/jackwadden/ANMLZoo).

- **HSCompile**: an extension to Intel Hyperscan
  (https://github.com/01org/Hyperscan) that supports compilation of PCRE to
  MNRL, compilation of MNRL to Hyperscan pattern databases, and execution of
  Hyperscan pattern databases (https://github.com/kevinaangstadt/HSCompile).

- **VASim**: an open-source and extensible automata simulation, analysis, and
  transformation framework. VASim can manipulate both ANML (Micron's Automata
  Network Markup Language) and MNRL files (https://github.com/jackwadden/VASim).

- **Automata Lab**: a GUI front-end for simulating automata using VASim
  (https://github.com/dankramp/AutomataLab).

- **DFAGE**: a GPGPU-accelerated<sup>*</sup> DFA execution engine
  (https://github.com/vqd8a/DFAGE).
  
- **iNFAnt2**: a GPGPU-accelerated<sup>+</sup> NFA execution engine
  (https://github.com/vqd8a/iNFAnt2).
  
- **REAPR**: a tool for generating an FPGA<sup>^</sup> kernel and associated
  reporting architecture for an input automaton (https://github.com/ted-xie/REAPR).

- **Automata-to-Routing**: a tool for experimenting with and evaluating
  spatial-reconfigurable automata processing routing networks
  (https://github.com/jackwadden/Automata-to-Routing).
  
 - **RAPID**: an experimental programming language for defining sequential
   pattern searches (https://github.com/kevinaangstadt/RAPID).

<sup>*</sup>Requires an Nvidia graphics card with compute capability >= 3.5.<br />
<sup>+</sup>Requires an Nvidia graphics card with compute capability >= 6.1.<br />
<sup>^</sup>Requires an SDAccel-compatible FPGA and the SDAccel software stack.

## Docker Image
For ease of use, we provide a Docker image (https://www.docker.com) with all
software tools configured; the image may be downloaded
[here](https://hub.docker.com/r/kevinaangstadt/mncart/).

### Basic usage
To use the Docker image, you much launch an interactive docker container:
```
$ docker run -i -t kevinaangstadt/mncart
```

Usage of Automata Lab requires port 9090 of the container to be forwarded to the
main system:
```
$ docker run -p 9090:9090 -i -t kevinaangstadt/mncart
```

It is also possible to mount a local directory inside of the the docker
container for experiments:
```
$ docker run -v /full/path/on/local/system:/full/path/in/container -i -t kevinaangstadt/mncart
```

Further information regarding usage of docker may be found in the [official documentation](https://docs.docker.com).

**NOTE:** Usage of GPU engines requires the installation of `nvidia-docker`,
which exposes the GPU to the Docker container. Be sure to start the container
using `nvidia-docker` rather than `docker`. Details on installing the required
packages may be found [here](https://github.com/NVIDIA/nvidia-docker/wiki).

### Building from scratch
A `Dockerfile` is provided in the the `docker` directly. You may build the
Docker image from within this directory:
```
$ cd docker/
$ docker build <name of image> .
```
You may provide command line arguments for `hs_build_threads` and
`vtr_build_threads` to specify the number of jobs used by `make` for building
Hyperscan and Verilog-to-Routing. The default for both is 8.

## Contributing
If you have developed a tool for the MNCaRT ecosystem (or would like to update
one of the existing repositories to a more recent commit), please submit a pull
request.  Your pull request should include:

- the new git submodule containing the project or an updated commit of an
  existing submodule
- new documentation in this file (README.md) describing the tool
- modifications to `docker/Dockerfile` to include building of your tool for the
  Docker image

## License
Each tool is licensed separately; please refer to the individual project for
their respective licenses.
  
## Publications
The following publications are associated with the MNCaRT repository:

- Kevin Angstadt, Jack Wadden, Vinh Dang, Tex Xie, Dan Kramp, Westley Weimer,
  Mircea Stan, and Kevin Skadron, "MNCaRT: An Open-Source, Multi-Architecture
  Automata-Processing Research and Execution Ecosystem," in IEEE Computer 
  Architecture Letters, vol. 17, no. 1, pp. 84-87, 2018.
  
  Bibtex:
    ```
    @ARTICLE{8166734, 
        author={K. Angstadt and J. Wadden and V. Dang and T. Xie and D. Kramp and W. Weimer and M. Stan and K. Skadron}, 
        journal={IEEE Computer Architecture Letters}, 
        title={{MNCaRT}: An Open-Source, Multi-Architecture Automata-Processing Research and Execution Ecosystem}, 
        year={2018}, 
        volume={17}, 
        number={1}, 
        pages={84-87}, 
        doi={10.1109/LCA.2017.2780105}, 
        ISSN={1556-6056}, 
        month={Jan}
    }
    ```

- Kevin Angstadt, Jack Wadden, Westley Weimer, and Kevin Skadron, "MNRL and
  MNCaRT: An open-source, multi-architecture state machine research and
  execution ecosystem," University of Virginia, Tech. Rep. CS2017-01, 2017.
  
  Bibtex:
    ```
    @techreport{mnrl,
        Author = {Angstadt, Kevin and Wadden, Jack and Weimer, Westley and Skadron, Kevin},
        Title = {{MNRL} and {MNCaRT}: An Open-Source, Multi-Architecture State Machine Research and Execution Ecosystem},
        Institution = {University of Virginia},
        Number = {CS2017-01},
        Year = {2017}
    }
    ```

## Acknowledgements
This work was supported in part by grants from the NSF (CCF-1116673,
CCF-1629450, CCF-1629450, CCF-1619123, CNS-1619098), Air Force
(FA8750-15-2-0075), Jefferson Scholars Foundation, Achievement Rewards for
College Scientists (ARCS) Foundation, a grant from Xilinx, and support from
C-FAR one of six centers of STARnet, a Semiconductor Research Corporation
program sponsored by MARCO and DARPA.