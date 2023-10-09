# Joins and Cycles

As of Numaflow v0.10, Pipeline Edges can be defined such that multiple Vertices send to a single vertex. This includes:

- UDF Map Vertices
- UDF Reduce Vertices
- Sink Vertices

Please see the following examples:

- [Join on Map Vertex](https://github.com/numaproj/numaflow/blob/main/examples/11-join-on-map.yaml)
- [Join on Reduce Vertex](https://github.com/numaproj/numaflow/blob/main/examples/11-join-on-reduce.yaml)
- [Join on Sink Vertex](https://github.com/numaproj/numaflow/blob/main/examples/11-join-on-sink.yaml)

## Numaflow v0.10: Introduction of Join Vertex

We are excited to announce a significant update to [Numaflow](https://numaflow.numaproj.io/), our Kubernetes-native stream processing platform. This update, which has been eagerly requested by our community, is available in [Numaflow v0.10](https://blog.numaproj.io/numaflow-v0-10-whats-new-212002160796).

## Previous Limitations

Previously, Numaflow allowed users to build [pipelines](https://numaflow.numaproj.io/core-concepts/pipeline/) where processing [vertices](https://numaflow.numaproj.io/core-concepts/vertex/) could only read from *one* vertex. This meant that Numaflow could only support simple pipelines or tree-like pipelines.

![Simple pipeline](https://miro.medium.com/v2/resize:fit:1400/1*MAwBZ3-eOQs29fvc36XLDw.png)

![Tree-like pipeline](https://miro.medium.com/v2/resize:fit:1400/1*XXycfwWNvsTZV-cr3lomOA.png)

Supporting pipelines where you had to read from multiple sources or UDFs was cumbersome and required creating redundant vertices.

## The Join Vertex

With Numaflow v0.10, we've introduced the Join Vertex feature. Join vertices allow users the flexibility to read from multiple sources, process data from multiple UDFs, and even write to a single sink.

![Join Vertex](https://miro.medium.com/v2/resize:fit:1400/1*5Ct-5otqpXTAVCNW_SJnNw.png)

There is no limitation on which vertices can be joined. For instance, one can join Map or Reduce vertices as shown below:

![Directed Graph](https://miro.medium.com/v2/resize:fit:1400/1*ldVi_wtuMH4rWFd0UG91cg.png)

## Benefits

The introduction of Join Vertex allows users to eliminate redundancy in their pipelines. It supports many-to-one data flow without needing multiple vertices performing the same job.

## Examples

### Join on Sink Vertex

By joining the sink vertices, we now only need a single vertex responsible for sending to the data sink.

![Join on Sink Vertex](https://miro.medium.com/v2/resize:fit:1400/1*5Ct-5otqpXTAVCNW_SJnNw.png)

### Join on Map Vertex

Two different Sources containing similar data that can be processed the same way can now point to a single vertex.

![Join on Map Vertex](https://miro.medium.com/v2/resize:fit:1400/1*mCXFAgbAPzyXEwJMaluxcQ.png)

### Join on Reduce Vertex

This feature allows for efficient aggregation of data from multiple sources.

![Join on Reduce Vertex](https://miro.medium.com/v2/resize:fit:1400/1*lbuKo7wauFe5CyI4Qv0wvQ.png)

## Creating a Pipeline with Join Vertices

The Pipeline Spec doesn't change at all. Now you can create multiple Edges that have the same “To” Vertex, which was previously prohibited.

Check out our examples on GitHub:

* [Join on Map Vertex](https://github.com/numaproj/numaflow/blob/main/examples/11-join-on-map.yaml)
* [Join on Reduce Vertex](https://github.com/numaproj/numaflow/blob/main/examples/11-join-on-reduce.yaml)
* [Join on Sink Vertex](https://github.com/numaproj/numaflow/blob/main/examples/11-join-on-sink.yaml)
* [Cycle to Self](https://github.com/numaproj/numaflow/blob/main/examples/10-cycle-to-self.yaml)
* [Cycle to Previous](https://github.com/numaproj/numaflow/blob/main/examples/10-cycle-to-prev.yaml)

## Cycles

A special case of a "Join" is a **Cycle** (a Vertex which can send either to itself or to a previous Vertex.) An example use of this is a Map UDF which does some sort of reprocessing of data under certain conditions such as a transient error.

![Cycle](https://miro.medium.com/v2/resize:fit:1400/1*wYokY1wa9LhI1hKYimWiKA.png)

Cycles are permitted, except in the case that there's a Reduce Vertex at or downstream of the cycle. (This is because a cycle inevitably produces late data, which would get dropped by the Reduce Vertex. For this reason, cycles should be used sparingly.)

The following examples are of Cycles:

- [Cycle to Self](https://github.com/numaproj/numaflow/blob/main/examples/10-cycle-to-self.yaml)
- [Cycle to Previous](https://github.com/numaproj/numaflow/blob/main/examples/10-cycle-to-prev.yaml)

## Next Steps

If you're an existing user, try out this new feature. If you're new to Numaflow, visit our [website](https://numaflow.numaproj.io/) and check out our [Quick Start Guide](https://numaflow.numaproj.io/quick-start/) to set up a few example pipelines in just a few minutes!

Join us on the [Numaproj Slack channel](https://join.slack.com/t/numaproj/shared_invite/zt-19svuv47m-YKHhsQ~~KK9mBv1E7pNzfg) and browse the [Numaproj blog](https://blog.numaproj.io/) for the latest features, releases, and use cases.

We welcome feedback and [contributions](https://numaflow.numaproj.io/development/development/). If you like what you see, please consider giving our [Numaflow project](https://github.com/numaproj/numaflow) a star on GitHub.
