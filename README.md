# source-framework
Initial sketches for a ResourceSync source framework

---

## Components and variability points

![components](img/comp_01.png)

_Fig. 1. Component Diagram with variability points_

### i01 ResourceSync

Central component, director of execution. 

__Questions:__ Should it be responsible for pasting the rest of the components
together as well or is this a task for an external plugin framework?

__Required interfaces:__
* IParas, parameters, configuration
* IExceute, for executing a resourcesync run
* ISend (optional) for moving/copying/sending resourcesync metadata files and/or resources to the
document root of a web server

__Provided interface:__
* Observable, register observers that will receive notifications of events taking place during 
a resourcesync run

### i02 rs-xml

Component capable of converting resourcesync sitemaps to and from xml and classes.

__Provided interface:__
* IXml, convert to and from hierarchical classes and xml streams/files

### i03 Parameters

Component capable of persisting parameters and configuration details for different configurations.

__Provided interface:__
* IParas, read and write parameters and configuration details from/to file, validate parameters, 
list configurations

### i04 Generator

Component capable of yielding resource metadata items (the data in the element url of an urlset).
There will be ResourceGenerators and ChangeGenerators, each for a specific implementation
 (File system, Elastic Search, etc.) 
 
__Required interfaces:__
* ISelect (optional), select resources (For Generators based on indexing systems 
probably integrated in the query of a generator it self.)
* IFilter (optional, pluggable), filter resources. Filtering can be based on metadata on the resource or the
content of a resource itself. (Filtering based on other criteria?)
* IXml (optional), required if Generator is responsible for comparing present resource state with
previous resource state

__Provided interface:__
* IGenerate, yield applicable resource metadata items

### i05 Executor
Component capable of yielding resourcelists or changelists, resourcedumps or changedumps.

__Required interfaces:__
* IGenerate, source of applicable resource metadata items
* IXml, for producing sitemaps, xml streams/files
* IParas, source of validated parameters, derived parameters

__Provided interface:__
* IExcecute, executing a specific resourcesync run

### i06 Sender

Provides logistics for resourcesync metadata and resources after an execution run. 

__Required interface:__
* IParas, source of validated parameters, derived parameters

__Provided interface:__
* ISend, move/copy/send resourcesync metadata files and/or resources to the
document root of a web server

### i07 Select

Select resources

### i08 Gate

Filter resources

## Variability Model

![var_mod](img/var_mod_01.png)

_Fig. 2. Variability model. Components could be generic, except for components in grayed areas. These
 are implementation specific for use case (File System, Elastic Search etc.)_

### VP01 Configure

There required core parameters (metadata_dir, url_prefix, strategy etc.) and implementation specific
parameters. Should we separate them, make them extensible?

### VP02 Strategy

What should the executor produce? Simple an enumeration of the 4 types.

#### VP03 Execute

Choice of executor. Coupled on Strategy. 

#### VP04 Generate

Choice of type of generator.

#### VP05 Generator Impl.

Choice of implementation of generator.

### VP06 Send

Optional pack/send/move metadata and/or resources to document root of server



