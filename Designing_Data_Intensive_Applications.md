## Designing Data-Intensive Applications: The Big Idea Behind Reliable, Scalable, and Maintainable Systems

by Martin Kleppmann

The following excerpt from the "Designing Data-Intensive Applications" book outlines the main points of each chapter using the following structure:

**Part I: Foundations** 

-   **Chapter 1: Reliable, Scalable, and Maintainable Applications**. This chapter introduces the fundamental goals of designing data-intensive applications, focusing on reliability, scalability, and maintainability.
    - **Reliability** involves ensuring that the system functions correctly even in the presence of faults:
      - **Hardware faults**: Hard drives crash, power supplies burn out, RAM becomes faulty, network cables get unplugged.
      - **Software errors**: Bugs in the software can cause the system to behave incorrectly.
      - **Human errors**: People make mistakes, and no system can protect against.
    - **Scalability** is the term we use to describe a system's ability to cope with increased load. 
      - **Describing Load**: Load can be described with a few numbers, such as requests per second, reads versus writes, or the number of concurrent users.
      - **Describing Performance**: Performance is the term we use to describe how well a system is performing. 
      - **Approaches for Coping with Load**: There are two ways to cope with load: _scaling up_ and _scaling out_.
    - **Maintainability** is the ease with which a system can be modified to meet new requirements.
      - **Operability**: Make it easy for operations teams to keep the system running smoothly.
      - **Simplicity**: Make it easy for new engineers to understand the system.
      - **Evolvability**: Make it easy for engineers to make changes to the system in the future.
 

-   **Chapter 2: Data Models and Query Languages**. This chapter compares different data models and query languages and discusses their suitability for various situations.
-   **Chapter 3: Storage and Retrieval**. This chapter explores storage engines and how databases arrange data on disk for efficient retrieval.
-   **Chapter 4: Encoding and Evolution**. This chapter examines data encoding formats (serialization) and strategies for evolving schemas over time.

**Part II: Distributed Data**

-   **Chapter 5: Replication**. This chapter discusses the concept of replication, focusing on techniques to keep copies of data consistent across multiple nodes.
-   **Chapter 6: Partitioning**. This chapter explores partitioning as a method for dividing large datasets into smaller subsets, which is crucial for scalability.
-   **Chapter 7: Transactions**. This chapter delves into transactions, examining their role in ensuring data consistency and integrity in distributed systems.
-   **Chapter 8: The Trouble with Distributed Systems**. This chapter highlights the unique challenges and complexities that arise when dealing with distributed systems.
-   **Chapter 9: Consistency and Consensus**. This chapter investigates consistency and consensus in distributed systems, addressing how to maintain data consistency in the face of network failures and other challenges.

**Part III: Derived Data**

-   **Chapter 10: Batch Processing**. This chapter examines batch processing systems, such as MapReduce, and their significance in building large-scale data systems.
-   **Chapter 11: Stream Processing**. This chapter builds on the concepts of batch processing and applies them to data streams, enabling real-time or near real-time data processing.
-   **Chapter 12: The Future of Data Systems**. This chapter offers perspectives on the future of data systems, exploring new approaches to designing systems that are reliable, scalable, and maintainable. 

The book aims to explain how to create applications and systems that possess those three qualities.  The author highlights that no single database can efficiently handle all possible use cases, prompting applications to compose several software components to meet their goals. 

The book also features a glossary of terms for data systems. For example, the glossary defines the concept of a log as an append-only file for storing data and lists different types of logs and their uses. 
