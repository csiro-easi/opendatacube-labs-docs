[ee]: https://github.com/csiro-easi/opendatacube-labs-docs/blob/master/images/Execution_Engine.png "Execution Engine (EE)"
[s3aio]: https://github.com/csiro-easi/opendatacube-labs-docs/blob/master/images/S3AIO.png "S3 Array IO (s3aio)"

# Open Data Cube Labs
__Developing features and extensions related to the ODC code-base that may or may not be incorporated into the ODC in the future.__

## Rationale
The goal of the  ODC Labs environment is to develop and test new features for improving or adapting the ODC that may involve breaking, experimental or otherwise extensive changes not suited to the [production ODC code base](https://github.com/opendatacube) and the [community](https://www.opendatacube.org/) it supports. Improvements and adaptations may include speed, ease of use, memory performance, adaptability, modularity, API changes and so on.

CSIRO as a founding member of the ODC and a science organisation hosts the ODC Labs on behalf of the ODC community. This isn't a separate initiative to the ODC production area but a fully integrated part of the ecosystem. The separate hosting is simply to help reduce confusion in the production area and to support creative new directions and innovation in a collaborative environment. 

**You or your organisation are welcome to propose an ODC Labs project or get involved in an existing one. All ODC community members are welcome to participate.
- Contact the [CSIRO EASI core team](https://github.com/orgs/csiro-easi/teams/easi-core) to get started.**

Research and operational agencies may choose to adopt new features from the ODC Labs when the new features can be shown to work better for their deployment(s). There is no expectation, nor requirement, that any or all ODC Labs features will be adopted into the production open data cube code. Indeed, the nature of an experimental test-bed is that backwards-compatibility may well be broken in the advance of new or bespoke features.

To help get your creative juices thinking here are a range of ODC Labs projects people are thinking about:
+ Linking computational modelling and the ODC for forecasting events
+ Machine learning
+	Feature detection and feature based API
+	Interoperability challenges between EO Data Cubes
+	Algorithms for generating decision-ready products
+ Multi-sensor data integration and analysis
+ Hyperspectral and SAR analysis
+ New innovative tools and solutions to work with EO Data Cubes
+ Support for high resolution EO data
+ Cloud-based computation and storage models
+ Architecture design of EO Data Cubes (HPC, Distributed Computing, Super Computers)
+ Multi-source and destination data cache management for organisation deployments 


# CSIRO ODC

## Rationale

We want to be able to index sparse high-dimensional data of any size and be able to search, retrieve, and perform auto-parallised computation on the data in a performant way and in a simple user-friendly way.

To make this happen, we need the following:
  1. S3 Array IO (s3aio) - a performant distributed storage library for large ndarrays in the Cloud or Disk.
  2. ndindex - a distributed index to represent all of the distributed and sparse nd array data as one virtual sparse ndarray for search and retrieval.
     - Not implemented
  3. Execution Engine - a performant distributed execution of functions over data of any size with results mapped back to the distributed storage in the Cloud.
  4. API - a simple user-friendly interface to the system.
     - Not implemented


## Execution Engine

![alt text][ee]

Our objective is to achieve performant distributed execution of functions over data of any size with results mapped back to the distributed storage in the Cloud.

Our approach allows submission of a Python function along with data queries for processing. It performs processing by splitting the selected data volumes into small independent jobs executed on multiple EC2 instances. This is the simplest embarrassingly parallel execution model and requires no data or computation sharing between EC2 instances. It is well suited for scaling on the Cloud and, whilst simple, is a very common requirement for EO applications.

We also allow to use Execution Engine as a parallel map compute system where the user supplies a set of independent tasks to be executed.

### What advantages does this provide?
 - Automatic scaling across EC2 instances via data subsetting or by user provided tasks
 - Represents distributed results as one virtual sparse ndarray.
 
#### Further work:
 - Task Paralleliser - (previously known as Task Decomposition) - Given a python function/workflow, automatically create a parallel version and execute.

