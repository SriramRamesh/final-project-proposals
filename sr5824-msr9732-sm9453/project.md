---
title: "Static Code Analysis for Hetrogeneous Cloud Applications"
author:
  - Sriram Ramesh <sriram.ramesh@nyu.edu>
  - Metarya Ruparel <msr9732@nyu.edu>
  - Safwan Mahmood <sm9453@nyu.edu>
---

> In what follows instructions (including this one) are in blockquotes, while
> text not in blockquotes is a sample. You should of course replace the title
> author metadata that shows up above with appropriate values.
>
> You should start your proposal by stating a problem statement or an overall
> goal  as below.

For our Big Data and Machine Learning final project we plan to develop a
static code analyser for applications targeting hetrogeneous cloud environment.

# Motivation
> Next you should talk about why, citing sources for your beliefs.

Recently, there has been a lot of excitement around quantum computing, and in the
last three years [Google](https://www.nature.com/articles/s41586-019-1666-5),
[Microsoft](https://azure.microsoft.com/en-us/solutions/quantum-computing/) and
others have begun to demonstrate that we are getting close to the point where we
can feasibly build and run programs on quantum computers. A lot of the
excitement around quantum computing stems from its ability to solve some
problems exponentially faster than classical computers. However, all current
quantum computers have limited memory resources, mostly only a few qubits.
Therefore, actual applications are likely to require resources across multiple
quantum computers, but this problem has not been considered thus far.

# Proposed Solution
> Next you should talk about how you plan to address the problem. 
> This should include information about what infrastructure (e.g., CloudLab
> resources) you plan to use, how you plan to make progress, and how you plan to
> evaluate your view.

Since quantum computers are not easily accessible at the moment, we plan to
rely on the [Qx](http://qutech.nl/qx-quantum-computer-simulator/) simulator, and
build on the work done in [ScaffCC](https://github.com/epiqc/ScaffCC). Currently
QX can only simulate O(10) qubits and hence closely matches our assumptions
above. Our plan then proceeds as follows:

* We plan to start by creating large circuits in ScaffCC, building test programs
that cannot be simulated using Qx. 
* Next, we will partition these circuits by hand into portion that can be run on
  individual Qx instances. 
* Next, we will develop a wrapper around Qx so that results from one instance
    can be communicated to another. We will use this wrapper to assemble the
    individual pieces of the circuit into one program and show that we can
    compute the entire program.
* Next, we plan to work on developing techniques to automatically partition code
  circuits rather than hand partitioning them. We will begin by developing
  simple heuristics for this, and extend if time allows.

We plan to run our experiments on multiple CloudLab nodes, each of which
executes one instance of QX.

> You should also talk about how you will evaluate your progress, and what you
> think the ideal end goal would look like.

We will evaluate our project by running a variety of circuits both large and
small. We will use large circuits to demonstrate that our approach makes some
computations feasible, while we will use small circuits (which can be run on
both a single instance and distributed) to measure the performance overhead.

Examples:

Big data, large-scale parallel and high-performance computing applications are been deployed
at higher rates than ever. With burst of such use cases, heterogenous cloud deployment should not
be a bottleneck affecting fast deployments in various public clouds.

We narrow down to a use case of batch jobs, a capability provided by all public clouds, or atleast the ones in 
the scope of our discussion.

Usually, batch jobs require spinning up of cluster of workers, followed by jobs submitted to
these workers. When we look at the implementations of this API across all the cloud platforms,
the underlying principle remains constant. 

Let's consider a particular case, integration of batch job APIs with Java. Each cloud provider has it's
own library which implements the required functionalities along with dependencies required (For example: Maven).
When the library is used, same flow is being executed across all clouds.

Ex:

Azure

The code snipped looks like.

```
// create the batch client for an account using its URI and keys
BatchClient client = BatchClient.open(new BatchSharedKeyCredentials("https://API.eastus.batch.azure.com", "user", batchKey));

// configure a pool of VMs to use 
VirtualMachineConfiguration configuration = new VirtualMachineConfiguration();
configuration.withNodeAgentSKUId("batch.node.ubuntu 16.04");
client.poolOperations().createPool(poolId, poolVMSize, configuration, poolVMCount);
```

While a dependicy is being added to pom.xml file.

```
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>4.0.0</version>
</dependency>
```

Similarly, when we look at AWS.

```
public class BatchClient {
public static void main(String[] args) {
        AWSBatch client = AWSBatchClientBuilder.standard().withRegion("us-east-1").build();
SubmitJobRequest request = new SubmitJobRequest().withJobName("example").withJobQueue("new-queue").withJobDefinition("sleep30:4");
SubmitJobResult response = client.submitJob(request);
System.out.println(response);    
}
}
```

While a dependicy is being added to pom.xml file.

```
 <!-- https://mvnrepository.com/artifact/com.amazonaws/aws-java-sdk-batch -->
    <dependencies>
    <dependency>
    <groupId>com.amazonaws</groupId>
    <artifactId>aws-java-sdk-batch</artifactId>
    <version>1.11.470</version>
</dependency>
    </dependencies>
```

When we look closely, if such regions are indentified in the code which execute the same common end goal
across the cloud platforms, we could come up with ways to suggest snippets for the targeted cloud. Thus providing the 
same functionality with minimal development overhead.

There are many features across the cloud providers which at core deliver the same functionalities. For the sake of discussion, 
we explore limited examples. The use cases can extended across features and languages used.


# Timeline
> You should lay out a plan for what you hope to have done by each checkin. Note
> we understand that sometimes unexpected bugs or other problems lead to slip
> ups, but the idea behind the timeline is to provide you with the ability to
> determine where you are in this process. It is important you propose a
> realistic timeline that accounts for classes, interviews and other things
> going on in your life.

* Checkin I (03/30): Have QX and ScaffCC running, have a large circuit that
    cannot be run on one machine, and a manual partitioning where each part can
    be run on a machine.
* Checkin II (04/20): We plan to have an initial implementation where multiple
    Qx instances can communicate with each other. We will have an initial
    measure of performance overheads. We also plan to have decided whether and
    how to optimize our implementation to reduce these overheads.
* Final Handin (05/11): We will have the distributed implementation, and an
    initial system that uses heuristics to partition the circuit.
