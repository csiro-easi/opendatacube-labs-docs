[ee]: https://github.com/csiro-easi/opendatacube-labs-docs/blob/master/images/Execution_Engine.png "Execution Engine (EE)"
[s3aio]: https://github.com/csiro-easi/opendatacube-labs-docs/blob/master/images/S3AIO.png "S3 Array IO (s3aio)"

# ODC labs

## Rationale

...

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

## S3 Array IO (s3aio)

![alt text][s3aio]

Our approach stores only the raw array data in chunked form and distributed across a distributed key-value store such as S3.
We do this to decouple the metadata and internal b-tree index typically found in EO file formats (e.g. NetCDF/HDF/GeoTIFF).
This decoupling enables us to store the metadata and internal index elsewhere e.g. in a distributed ndindex and allow us more freedom to serve the data to distributed storage to improved IO scalability and performance. This decoupling also ensures the stored data is in a Compute Ready Data format (CRD) that can be readily used natively by CPUs/GPUs/FPGAs.

### What advantages does this provide?
 - IO Performance - Array IO for read and write can happen massively in parallel.
 - Efficient write IO - A bit change only results in 1 small IO as opposed to many IOs to upload the entire ndarray if the array is stored as one object in S3.
 - Efficient Array operations - Array Operations are inherently mapped to distributed storage by default and there is no need to retrieve metadata from each object to compute what bytes to retrieve.
 - Search Performance - All Array data can be represented as one virtual sparse ndarray but actually distributed across S3.
 - Compute Ready Data format - can be used natively with hardware without translations.

#### Future work:
 - ndindex - distributed index to represent all the distributed and sparse nd array data as a single sparse ndarray for search and retrieval.

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

