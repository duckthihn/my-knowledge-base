---
type: entity
title: Apache Spark Architecture
created: 2026-11-13
updated: 2026-11-13
sources: ["[[learn-spark]]"]
tags: [spark, architecture, cluster, executors, driver, infrastructure]
status: stable
---

# Apache Spark Architecture

Apache Spark is a distributed general-purpose computing system that coordinates parallel data processing across a cluster using a master-worker architecture (see [[learn-spark]]). The architecture decouples physical host infrastructure (Workers) from application-specific compute processes (Executors), coordinated by a central Driver and cluster manager (Master) (see [[learn-spark]]).

## Core Cluster Components

Spark cluster architecture consists of four primary runtime and infrastructural abstractions (see [[learn-spark]]):

1. **Spark Master (Cluster Manager)**:
   - Acts as the cluster resource manager (e.g., Standalone Master, Kubernetes, YARN, or Mesos) (see [[learn-spark]]).
   - Tracks available cluster hardware and allocates compute resources to requesting applications upon submission (see [[learn-spark]]).

2. **Driver**:
   - The orchestrating process representing a single Spark application (see [[learn-spark]]).
   - Executes user code, constructs the Directed Acyclic Graph (DAG), negotiates resources with the Master, schedules stages, and sends tasks directly to executors (see [[learn-spark]], [[spark-execution-model]]).

3. **Workers**:
   - The underlying host instances (e.g., EC2 virtual machines, bare-metal servers) providing CPU and memory resources (see [[learn-spark]]).
   - **Independent of the Spark cluster itself**: Worker machines exist before an application starts, remain running after it finishes, and may host multiple independent workloads simultaneously (see [[learn-spark]]).

4. **Executors**:
   - Ephemeral JVM worker processes launched on Worker machines specifically for a given Spark application (see [[learn-spark]]).
   - *Do not confuse Workers with Executors*: Workers are long-lived infrastructure, whereas Executors are short-lived, application-bound processes that execute assigned tasks on partitions and terminate when the application completes (see [[learn-spark]], [[spark-partitioning-and-shuffle]]).

## Application Lifecycle (`spark-submit`)

The execution lifecycle of a Spark application proceeds through the following sequential stages (see [[learn-spark]]):

```
+---------------+      1. Submits App     +------------------------+
| spark-submit  | ──────────────────────> | Master (Cluster Mgr)   |
+---------------+                         +──────────┬─────────────+
                                                     │ 3. Allocates Resources &
                                                     │    Spawns Executors
                                                     ▼
+---------------+    2. Requests Res.    +─────────────────────────+
| Driver        | ──────────────────────>| Worker 1 / Worker 2     |
| (App Brain)   |                        | ┌─────────────────────┐ │
|               | <──────────────────────┼─┤ Executor A / B      │ │
+───────┬───────+   4. Registers Ready   | └─────────────────────┘ │
        │                                +─────────────────────────+
        │ 5. Dispatches Tasks & Receives Results
        ▼
   (Executors process partitions and write output)
```

1. **Submission**: The user runs `spark-submit` specifying cluster master URLs, resource configurations, and application code (see [[learn-spark]]).
2. **Driver Launch & Negotiation**: The Driver process initializes, begins executing code, and requests the required executors, cores, and memory from the Master (see [[learn-spark]]).
3. **Executor Spawning**: The Master locates available Worker nodes with unallocated capacity and instructs them to launch Executor JVM processes (see [[learn-spark]]).
4. **Registration**: Newly launched Executors establish a network connection back to the Driver to report readiness (see [[learn-spark]]).
5. **Task Execution**: When an action occurs, the Driver converts the DAG into stages and distributes individual partition tasks directly to available Executors (see [[learn-spark]], [[spark-execution-model]]).
6. **Teardown**: When the application completes, the Driver releases its state, the Master terminates the application's Executors, and Worker nodes return to the idle resource pool (see [[learn-spark]]).

## Related

- [[learn-spark]]
- [[spark-execution-model]]
- [[spark-partitioning-and-shuffle]]
- [[debugging-slow-spark-jobs]]
