>> **System Design is not about writing code (initially). It's about making high-level decisions and trade-offs before a single line of code is written.**

1. **Scalability:** How will the system handle a large number of users or requests simultaneously?
2. **Latency and Performance:** How can we reduce response time and ensure low-latency performance under load?
3. **Communication:** How do different components of the system interact with each other?
4. **Data Management:** How should we store, retrieve, and manage data efficiently?
5. **Fault Tolerance and Reliability:** What happens if a part of the system crashes or becomes unreachable?
6. **Security:** How do we protect the system against threats such as unauthorized access, data breaches, or denial-of-service attacks?
7. **Maintainability and Extensibility:** How easy is it to maintain, monitor, debug, and evolve the system over time?
8. **Cost Efficiency:** How can we balance performance with infrastructure cost?
9. **Observability and Monitoring:** How do we monitor system health and diagnose issues in production?
10. **Compliance and Privacy:** Are we complying with relevant laws and regulations (e.g., GDPR, HIPAA)?


---

## **1. Bandwidth**

- **Definition:** Maximum data transfer capacity of a network in a given time.
    
- **Measure:** Bits per second (bps), Mbps, Gbps.
    
- **Example:** Agar network bandwidth 100 Mbps hai toh theoretically ek second mein 100 megabits transfer ho sakte hain.
    
- **Misconception:** Bandwidth ≠ Speed; bandwidth data ki _quantity_ hai, speed data ka _delay_ hota hai.
    

---

## **2. Latency**

- **Definition:** Data ko ek point se doosre point tak jaane mein jo delay hota hai.
    
- **Measure:** Milliseconds (ms).
    
- **Types:**
    
    - Network Latency
        
    - Disk Latency
        
    - Application Latency
        
- **Example:** Request server ko bhejne ke baad 200 ms mein response aata hai → latency = 200 ms.
    
- **Trade-off:** High bandwidth ≠ low latency.
    

---

## **3. Throughput**

- **Definition:** Actual rate at which data successfully transfer hota hai (effective data rate).
    
- **Measure:** Bits per second ya requests per second.
    
- **Relation:**  
    `Throughput ≤ Bandwidth` (due to overhead, packet loss, congestion).
    

---

## **4. Availability**

- **Definition:** System kitne percent time functional hai (uptime).
    
- **Measure:** Percentage (e.g., 99.99% uptime).
    
- **Formula:**
    
    ```
    Availability = (Uptime / Total Time) × 100
    ```
    
- **Example:** 99.9% uptime → ~43 minutes downtime per month.
    

---

## **5. Reliability**

- **Definition:** System consistently correct results deta hai bina failure ke.
    
- **Focus:** Correctness + Fault tolerance.
    
- **Difference from Availability:** High availability system up ho sakta hai but galat result de raha ho toh reliable nahi hoga.
    

---

## **6. Scalability**

- **Definition:** System ki ability workload increase hone par handle karna.
    
- **Types:**
    
    - **Vertical Scaling:** More CPU/RAM add karo.
        
    - **Horizontal Scaling:** More servers add karo (load balancing).
        

---

## **7. Load Balancing**

- **Definition:** Traffic ko multiple servers mein distribute karna for better performance and reliability.
    
- **Techniques:**
    
    - Round Robin
        
    - Least Connections
        
    - IP Hash
        
- **Hardware vs Software:**  
    Nginx (software), F5 (hardware).
    

---

## **8. Caching**

- **Definition:** Frequently accessed data ko fast memory (RAM) mein store karna to reduce latency.
    
- **Types:**
    
    - Client-side cache
        
    - Server-side cache
        
    - CDN cache
        
- **Examples:** Redis, Memcached.
    

---

## **9. CDN (Content Delivery Network)**

- **Definition:** Geo-distributed servers jo static content ko user ke paas wale location se serve karte hain.
    
- **Purpose:** Reduce latency, improve load time.
    
- **Example:** Cloudflare, Akamai.
    

---

## **10. CAP Theorem**

- **Definition:** Distributed system ek time mein **Consistency, Availability, Partition Tolerance** mein se sirf 2 hi guarantee kar sakta hai.
    
- **Scenarios:**
    
    - CA (No partition) → Traditional SQL DB.
        
    - CP (Partition + Consistency) → Zookeeper.
        
    - AP (Partition + Availability) → DynamoDB.
        

---

## **11. Consistency**

- **Definition:** Har client ko same data milega, chahe woh kisi bhi replica se read kare.
    
- **Strong vs Eventual Consistency:**
    
    - **Strong:** Sab replicas turant sync.
        
    - **Eventual:** Time lag sakta hai sync hone mein.
        

---

## **12. Partition Tolerance**

- **Definition:** Network failure hone par bhi system kaam karta rahe.
    
- **Example:** Agar cluster ke kuch nodes alag ho jayein (network split) toh bhi system degrade na ho.
    

---

## **13. Availability in CAP**

- **Definition:** Har request ko response milta hai (sahi/galat matter nahi, bas fail nahi hona chahiye).
    

---

## **14. Replication**

- **Definition:** Data ko multiple servers par copy karna.
    
- **Purpose:** Fault tolerance, high availability.
    
- **Types:**
    
    - Master-Slave
        
    - Master-Master
        

---

## **15. Sharding**

- **Definition:** Data ko horizontally partition karna across multiple databases.
    
- **Purpose:** Handle large datasets.
    
- **Example:** UserID ke range ke basis par shards.
    

---

## **16. Fault Tolerance**

- **Definition:** Component fail hone par bhi system kaam karta rahe.
    
- **Approach:** Replication, retries, failover.
    

---

## **17. Failover**

- **Definition:** Primary system fail hone par automatically secondary system par switch ho jana.
    
- **Example:** Active-Passive DB setup.
    

---

## **18. Load Shedding**

- **Definition:** Overload hone par low-priority requests ko reject karna to save system.
    

---

## **19. Rate Limiting**

- **Definition:** API requests per user ko restrict karna (e.g., 100 requests per minute).
    
- **Purpose:** Abuse prevent, fair usage.
    

---

## **20. Backpressure**

- **Definition:** Producer-Consumer architecture mein jab consumer slow ho toh producer ko signal dena ki slow ho jaye.
    

---

## **21. Message Queue**

- **Definition:** Asynchronous communication ke liye queue (e.g., Kafka, RabbitMQ).
    
- **Use:** Decouple services, retry, buffering.
    

---

## **22. Idempotency**

- **Definition:** Same request multiple baar send karne par same result aaye.
    
- **Example:** Payment retry karne par double debit na ho.
    

---

## **23. Eventual Consistency**

- **Definition:** Time ke baad sab replicas consistent ho jayenge.
    
- **Use:** High availability distributed DBs (DynamoDB, Cassandra).
    

---

## **24. Throughput vs Latency**

- **Throughput:** Kitna kaam ek time mein hota hai.
    
- **Latency:** Ek kaam hone mein kitna time lagta hai.
    

---

## **25. SLA (Service Level Agreement)**

- **Definition:** Agreement ki kitna uptime aur performance guarantee milega.
    
- **Example:** 99.99% uptime SLA.
    

---

## **26. SLO (Service Level Objective)**

- **Definition:** SLA achieve karne ka target.
    
- **Example:** Aim for 99.95% uptime internally.
    

---

## **27. SLI (Service Level Indicator)**

- **Definition:** Metric jo measure karta hai (e.g., actual uptime %).
    

---

## **28. Data Durability**

- **Definition:** Once data is written, it won’t be lost.
    
- **Example:** AWS S3 durability 99.999999999%.
    

---

## **29. Quorum**

- **Definition:** Distributed systems mein read/write ke liye minimum nodes ka consensus.
    
- **Formula:** R + W > N (Consistency ensure karne ke liye).
    

---

## **30. Leader Election**

- **Definition:** Cluster mein ek node ko leader choose karna (e.g., Raft, Zookeeper).
    

---

## **31. Heartbeat**

- **Definition:** Nodes ke beech periodic signals jo batate hain ki node alive hai ya nahi.
    

---

## **32. Gossip Protocol**

- **Definition:** Nodes ek doosre se info share karte hain jaise gossip spread hota hai.
    

---

## **33. Circuit Breaker**

- **Definition:** Service fail ho rahi hai toh automatic calls stop karke fallback provide karna.
    

---

## **34. Throttling**

- **Definition:** Requests ki speed limit karna overload prevent karne ke liye.
    

---

## **35. Fan-out / Fan-in**

- **Fan-out:** Ek service ka data multiple services ko bhejna.
    
- **Fan-in:** Multiple services ka data combine karna.
    

---

## **36. Write-Ahead Log (WAL)**

- **Definition:** Changes ko disk par log karna before applying to ensure crash recovery.
    

---

## **37. Hot vs Cold Path**

- **Hot Path:** Real-time processing (fast).
    
- **Cold Path:** Batch processing (delayed).
   

---

## **38. Blue-Green Deployment**

- **Definition:** Do identical environments – ek live (blue) aur ek idle (green). Deploy green pe, test karo, phir switch karo.
    

---

## **39. Canary Release**

- **Definition:** New version ko chhote % users ke liye rollout karna test ke liye.
    

---

## **40. Sticky Sessions**

- **Definition:** Same user ko hamesha same server pe route karna (session consistency ke liye).
    
