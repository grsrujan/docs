
components
Orchestration components:
tPreJob
The tPreJob component starts the first series of Subjobs that will be executed, when your Job starts. Functionally, it performs no action that you could not achieve by connection your components in the correct order; however, it does provide a clear demarcation between your Job's initialisation sequence, and the main tasks that the Job is intended to perform.

The following tasks could be considered as pre-Job tasks: -

* Load Context
* Validate external environment
* Establish database connections
* Test connectivity to external services
* Check input files exist
* logging the start of your Job

tPostJob

The tPostJob component starts the final series of Subjobs that will be executed, when your Job finishes; that is, after tPreJob has completed and any other Subjobs that you have defined. A key aspect to the execution of this component is that it is always executed, even when an Exception is thrown by any previous processing. This makes it ideal for controlling de-initialisation and clean-up.
The following tasks could be considered as post-Job tasks: -

* Deletion of temporary files
* Database disconnection
* logging the finish of your Job


Processing Components
tBufferInput
The tBufferInput component works in partnership with the component tBufferOutput.

Data that has been previously written to tBufferOutput may be read, later, from tBufferInput. For more information on the usage of these components, read our article on tBufferOutput.

tBufferOutput


tMap
