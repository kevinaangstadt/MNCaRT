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
  
- **ANMLZoo**: an automata processing benchmark sweet containing a diverse set
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

- **DFAGE**: a GPGPU-accelerated DFA execution engine
  (https://github.com/vqd8a/DFAGE).

- **Automata-to-Routing**: a tool for experimenting with and evaluating
  spatial-reconfigurable automata processing routing networks
  (https://github.com/jackwadden/Automata-to-Routing).
  
 - **RAPID**: an experimental programming language for defining sequential
   pattern searches (https://github.com/kevinaangstadt/RAPID).

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