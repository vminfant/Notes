# Designing Data-intensive applications

## Foundations

An application is **data-intensive** if data is its primary challenge - the quantity of data, the complexity of data or the speed at which it is changing.

### Key Concepts

Three factors that are important in most software systems.

* **Reliability** - System should continue to work correctly even when things go wrong.
* **Scalability** - System's ability to cope up with increased load.
* **Maintainability** - People should be able to work on the system productively over time.

#### Reliability

System that anticipate faults and can cope up with them are called __fault-tolerant__ or __resilient__.

>  * **fault** - one component of the system fails.
>  * **failure** - system as a whole fails

Three possible kinds of faults,

  1. Hardware Faults
  2. Software Errors
  3. Human Errors

**Hardware Faults**

  * Until recently, redundancy of hardware components were sufficient to cope up with hardware faults.
  * With increased data volume and computing demands, more applications have begun using larger number of machines which proportionally increases the rate of hardware faults.
  * There is a move toward system that can tolerate the loss of entire machines by using software fault-tolerant techniques in preference or in addition to hardware redundancy. this approach has operational advantages.

**Software Errors**

Some examples,

  * A software bug.
  * A runaway process.
  * Unresponsive services.
  * Cascading failures - small fault cascaded to different components.

There is no quick solutions to software errors. Few small steps can help,

  * thinking about assumptions and interactions between the systems
  * thorough testing
  * process isolation
  * allowing processes to crash and restart
  * measuring
  * monitoring
  * analyzing system behavior in production

**Human Errors**

Humans are known to be unreliable. Approaches to avoid human errors.
  * Design systems that minimize the opportunities for error.
  * Decouple the places where people make the most mistakes from the places where they can failures.
  * Test thoroughly at all levels.
  * Allow quick recovery from human errors.
  * Set up detailed and clear monitoring.
  * Implement good management practices and training.

Reliability is for any applications.

#### Scalability

Load can be described in few numbers which we call _load parameters_.

  * Web server - Requests/sec
  * Database - Ratio of reads to writes
  * Cache - hit rate

*fan-out* - Number of requests to other services that we need
to make in order to serve one incoming request.

Two ways to investigate when analyzing load,

  1. When you increase the load parameter and keep the system resources unchanged, how is the system performance affected?
  2. When you increase the load parameter, how much do you need to increase the resources if you want to keep the performance unchanged?

Performance numbers,
  * Batch processing systems (Hadoop) - *throughput*  - the number of records we can process per second or total time it takes to run a job on a dataset of a certain size.
  * Online systems - *response time* - time between client sending a request and receiving a response.

  > * **Latency** and **response time** are not same. response time is what client sees. Latency is the duration that a request is waiting to be handled. (awaiting service)    
  > * **response time** is often service time + network delay + queueing delay.

Use percentiles to report response time.

![Response time](/images/ddia_foundations_001.png)

  * mean is not a good metric to measure response time.
  * median (p50) - half of user requests are served in less than the median response time.
  * High percentiles of response times (also known as tail latencies) to report thresholds. p95 represents 95% of requests are faster than particular threshold.

Percentiles are often used in service level objectives & service level aggrements.
 > service is considered up if p50 is 200ms & p99 is < 1s and the service is required to be up 99.9% of the time.

 * Queuing delays often account for a large part of the high response times.
 * _Only small number of slow requests to hold up the processing of subsequent requests - **head-on-line blocking**_
 * Histogram is the right way of producing response time data.

Approaches for Coping with Load
  * Scaling up - vertical scaling. moving into a powerful machine.
  * Scaling out - Horizontal scaling. Distributing the load across multiple smaller machines. (shared-nothing architecture or stateless)

*elastic* - automatically add computing resources when it detects a load increase. Useful when the load is highly unpredictable.

 > scaling on stateful data systems are often complex. Keep the database in a single node and scale up until scaling cost or high availability requirements forced to make it distributed. however, the future is changing to distributed data systems.

#### Maintainability

Three design principles of software systems that helps Maintainability.

  * **Operability** - easy to operate.(good operations)
  * **Simplicity** - easy to understand. (abstractions)
  * **Evolvability** - easy to accommodate new changes. Adopting to change (Agile). It goes with other names too **extensibility**, **modifiability**, **plasticity**
