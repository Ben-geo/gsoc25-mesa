# Data Collection Module for Mesa- Frames - Final Report Google Summer of Code 2025

## Overview

### Context
[Project Mesa](https://github.com/projectmesa) Mesa is an open-source Python library for agent-based modeling, ideal for simulating complex systems and exploring emergent behaviors.

[Mesa-Frames](https://github.com/projectmesa/mesa-frames) extends Mesa to support large-scale simulations with thousands or even millions of agents. By storing agents in a DataFrame, Mesa-Frames enables vectorized operations, leading to significant improvements in scalability and computational efficiency, particularly when multiple agents are activated simultaneously.

However, Mesa-Frames originally lacked a data collection module, which led to several difficulties for researchers. For example, data could only be accessed once a simulation had fully completed, making real-time analysis impossible. In addition, failures during execution often resulted in partial or complete loss of valuable data. These were among the issues that motivated the development of a dedicated data collection system in this project.


### GSoC 2025 Goals
The primary objective of this project was to implement a **robust and flexible data collection system for Mesa-Frames** with the following features:  
- **Multiple storage backends** (local files, cloud, and databases).  
- **Model- and agent-level collection** to support both global and granular perspectives.  
- **Event-driven collection**, ensuring only relevant data is gathered, thereby reducing overhead.  

![](Mesa-Frames.jpg)



## Contributions :
1. [Abstract Data Collector [PR]](https://github.com/projectmesa/mesa-frames/pull/156) 
    - Defined a standardized interface for collecting model- and agent-level data.  
    - Added support for reporter functions, conditional triggers, and asynchronous flushing.  
    - Established pluggable storage backends (memory, CSV, Parquet, S3, PostgreSQL) as extension points.  

2. [Concrete Data Collector [PR]](https://github.com/projectmesa/mesa-frames/pull/161) 
    - Collected data via lazy Polars pipelines for efficiency.  
    - Supported immediate and conditional collection for both model and agent data.  
    - Added persistence to local (CSV/Parquet), cloud (S3), and database (PostgreSQL) backends.  
    - Provided validation for inputs and schema integration for PostgreSQL.  
3. [Benchmarking Datacollector implementation [discussion]](https://github.com/projectmesa/mesa-frames/discussions/168)
   - Benchmarked flush strategies (CSV, Parquet, memory, deferred, async) on Boltzmann Wealth Model (1M agents).  
   - Identified file writes as the main bottleneck.  
   - **Async Flush** chosen as default: fastest runtime + best CPU use, at cost of higher memory.  

4. [Data Collector Enhancements [PR] (WIP)](https://github.com/projectmesa/mesa-frames/pull/167) 
    - Introduced **async flushing** to remove I/O bottlenecks and safely handle race conditions.  
    - Supported **multiple collects per step** by batching collections. 
    - Improved Code structure and Quality as well as introduced test cases


## Future Work
- Extend and refine documentation for broader adoption.  
- Test the data collector with additional, diverse Mesa-Frames examples.  
- Incorporate further edge-case testing to guarantee reliability under extreme conditions.  


## Challenges
The most demanding aspect of the project lay in **design decisions rather than coding**. Considerable time was invested in comparing alternative architectures and selecting the most efficient and scalable solutions.  


## Acknowledgement
- [Adam](https://github.com/adamamer20) for collaborating on the design discussions and contributing to the decision-making process, which helped shape the data collector into a more robust and scalable module.