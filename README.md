<div align="center">

<img src="./assets/header.svg" width="100%" alt="Dhairya Rupani — Terminal" />

<p align="center">
  <a href="https://www.linkedin.com/in/dhairya-rupani"><code>LinkedIn</code></a> &nbsp;·&nbsp;
  <a href="https://github.com/Dhairya0531"><code>GitHub</code></a> &nbsp;·&nbsp;
  <a href="https://dhairya0531.github.io/portfolio/"><code>Portfolio</code></a> &nbsp;·&nbsp;
  <a href="https://leetcode.com/u/Dhairya05/"><code>LeetCode (Knight 1869)</code></a> &nbsp;·&nbsp;
  <a href="https://codeforces.com/profile/Dhairya05"><code>Codeforces (Pupil 1271)</code></a> &nbsp;·&nbsp;
  <a href="mailto:dhairyarupani31@gmail.com"><code>Email</code></a>
</p>

</div>

---

### $ cat about.md

I am a final-year Computer Science undergraduate at Ahmedabad University specializing in **Backend Engineering** and **Distributed Systems**.

I focus on building resilient, high-throughput backend services that stay fast under high concurrency. My engineering work centers on custom API gateways, distributed rate limiting, fault-isolation architectures, and graph-theoretic optimization algorithms.

Beyond systems development, I have a disciplined background in competitive programming: **LeetCode Knight (Rating 1869, Top ~4% globally)** with **1,300+ algorithmic problems solved**, and experience authoring contest problems as a competitive programming event lead.

---

### $ ls -la skills/

<table width="100%">
  <thead>
    <tr>
      <th width="28%" align="left">Domain</th>
      <th width="72%" align="left">Technologies &amp; Architecture</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Languages</strong></td>
      <td><code>Go</code> · <code>Python</code> · <code>Java</code> · <code>TypeScript</code> · <code>JavaScript</code> · <code>SQL</code></td>
    </tr>
    <tr>
      <td><strong>Backend &amp; Systems</strong></td>
      <td><code>Node.js</code> · <code>Express.js</code> · <code>RESTful API Design</code> · <code>Microservices</code> · <code>Load Balancing (EWMA / Least-Conn)</code> · <code>Circuit Breakers</code> · <code>Idempotency</code></td>
    </tr>
    <tr>
      <td><strong>Databases &amp; Caching</strong></td>
      <td><code>PostgreSQL</code> · <code>Redis (Sliding Window, Atomic Caching)</code> · <code>MongoDB</code> · <code>MySQL</code></td>
    </tr>
    <tr>
      <td><strong>Cloud &amp; DevOps</strong></td>
      <td><code>Docker</code> · <code>Kubernetes (Rolling Updates, HPA)</code> · <code>AWS (EC2, S3, RDS, VPC, IAM)</code></td>
    </tr>
    <tr>
      <td><strong>Observability &amp; Tools</strong></td>
      <td><code>Prometheus</code> · <code>Grafana</code> · <code>OpenTelemetry</code> · <code>Git</code> · <code>Postman</code> · <code>Linux</code></td>
    </tr>
    <tr>
      <td><strong>Algorithms &amp; Modeling</strong></td>
      <td><code>Complex Networks (Betweenness Centrality)</code> · <code>NetworkX</code> · <code>OSMnx</code> · <code>Constraint Satisfaction (CSP)</code></td>
    </tr>
  </tbody>
</table>

---

### $ ./list_featured_projects.sh

#### 01. [Distributed API Gateway](https://github.com/Dhairya0531/API_Gateway)
*Production-grade microservices reverse proxy, traffic shaping, and observability layer.*

* **What it solves:** Direct microservice communication creates cascading failures, localized hotspotting, uncoordinated client bursts, and distributed telemetry gaps.
* **Why it is technically interesting:** Implements pluggable traffic distribution algorithms (**Round-Robin**, **Least-Connections**, and **EWMA latency weighting**), distributed sliding-window rate limiting backed by Redis atomic scripts, and an in-memory three-state **Circuit Breaker** (`Closed` → `Open` → `Half-Open`) with fallback semantics.
* **Tech Stack:** `Go 1.25` · `Redis` · `PostgreSQL` · `Docker` · `Kubernetes` · `Prometheus` · `Grafana` · `OpenTelemetry`
* **Important Engineering Detail:** Sustained **10,000+ requests/min** with **p95 latency under 2 ms** (1.89 ms measured) and zero failed requests under simulated load via k6. Audit logs are asynchronously batched to PostgreSQL to protect the hot request path. Deployed with Kubernetes Horizontal Pod Autoscaling.
* **Code:** [`github.com/Dhairya0531/API_Gateway`](https://github.com/Dhairya0531/API_Gateway)

<details>
  <summary><strong>View System Topology &amp; Algorithm Notes</strong></summary>

  ```
  [ Clients ] ──> [ Distributed API Gateway (Go) ]
                         │
                         ├──> [ Redis: Sliding-Window Rate Limiter ]
                         ├──> [ Resilience: Circuit Breaker & Fallbacks ]
                         ├──> [ Router: EWMA Latency Load Balancer ]
                         └──> [ Telemetry: OpenTelemetry Traces + Prometheus ]
                                      │
                                      ▼
                        [ Upstream Microservices ]
  ```

  * **EWMA Routing:** Updates running moving-average latency $S_t = \alpha Y_t + (1 - \alpha) S_{t-1}$ to shift traffic away from degrading instances prior to hard health-check failures.
  * **Quick Start:**
    ```bash
    git clone https://github.com/Dhairya0531/API_Gateway.git
    docker compose -f docker/docker-compose.yml up --build -d
    ```
</details>

<br/>

#### 02. [Urban Traffic Signal Optimization using Complex Networks](https://github.com/Dhairya0531/Complex_Network)
*Graph-theoretic traffic flow control using structural network centrality over localized heuristics.*

* **What it solves:** Conventional static fixed-time and purely greedy local-queue traffic controllers fail to prevent city-wide gridlock because they ignore structural road network topology and downstream bottleneck accumulation.
* **Why it is technically interesting:** Models metropolitan road networks as weighted directed graphs using OpenStreetMap data. Computes **Betweenness Centrality** to identify structural traffic chokepoints and incorporates global topology into a dynamic green-split controller:
  $$\text{Priority} = (\alpha \cdot \text{Queue Length}) + (\beta \cdot \text{Cumulative Wait Time}) + (\gamma \cdot \text{Betweenness Centrality})$$
* **Tech Stack:** `Python` · `NetworkX` · `OpenStreetMap (OSMnx)` · `Complex Network Analysis` · `Discrete-Time Simulation`
* **Important Engineering Detail:** Evaluated across 4 global metro topologies (Bengaluru, Berlin, London, Sydney) on graphs with up to **12,846 nodes and 28,412 edges**. Demonstrated up to **+198% throughput improvement** over adaptive Backpressure controllers and reduced travel times by up to **26.6%** over fixed-time baselines across bottleneck corridors.
* **Code:** [`github.com/Dhairya0531/Complex_Network`](https://github.com/Dhairya0531/Complex_Network)

<details>
  <summary><strong>View Graph Analysis Details</strong></summary>

  * **Topology Modeling:** Real-world driving graphs ingested via OSMnx with parsed lane counts, speed limits, and intersection flow capacities.
  * **Demand Synthesis:** Non-uniform traffic demand modeled with stochastic Poisson arrival distributions ($\lambda = 18\text{ vehicles/min}$).
  * **Betweenness Centrality:** $C_B(v) = \sum_{s \ne v \ne t} \frac{\sigma_{st}(v)}{\sigma_{st}}$, quantifying the exact structural bottleneck frequency of each intersection.
</details>

<br/>

#### 03. [TablingTime — University Timetable Automation Platform](https://github.com/PrashamMehta-04/TablingTime-Backend)
*Full-stack constraint-satisfaction scheduling system resolving university-wide academic logistics.*

* **What it solves:** Coordinating conflict-free lecture schedules across 350+ courses, 150+ professors, and 2,000+ students with constrained room capacities creates a high-dimensional combinatorial conflict challenge.
* **Why it is technically interesting:** Formulates and solves a constraint-satisfaction problem (CSP) enforcing hard bounds (no professor overlaps, room capacity constraints) and soft optimizations (student cohort schedule balance), backed by high-throughput room allocation REST APIs.
* **Tech Stack:** `React.js` · `TypeScript` · `Node.js` · `Express.js` · `MongoDB` · `JWT`
* **Important Engineering Detail:** Generates completely conflict-free academic schedules for the entire institution in **under 6 minutes**; engineered real-time room occupancy management endpoints for campus administrative workflows.
* **Code:** [`github.com/PrashamMehta-04/TablingTime-Backend`](https://github.com/PrashamMehta-04/TablingTime-Backend)

---

### $ cat engineering_interests.json

```json
{
  "distributed_systems": [
    "Fault tolerance, consensus models, and leader election",
    "Distributed caching, rate limiting, and circuit breaker patterns",
    "Microservice resilience and zero-downtime rolling deployments"
  ],
  "backend_engineering": [
    "High-throughput, low-latency API design in Go and Node.js",
    "Database indexing, connection pooling, and transactional consistency",
    "Asynchronous background workers and audit logging pipelines"
  ],
  "systems_modeling": [
    "Complex network topology analysis and structural graph centrality",
    "Constraint-satisfaction algorithms (CSP) for scheduling logistics"
  ],
  "cloud_and_observability": [
    "Container orchestration with Kubernetes (HPA, health probes)",
    "Distributed tracing with OpenTelemetry and metrics collection via Prometheus"
  ]
}
```

---

### $ sysctl -n hw.achievements

```
● LeetCode Knight (1869)           Top ~4% globally | 1,300+ DSA problems solved
● Codeforces Pupil (1271)          Active competitive programming contest participant
● PAIRATHON 2026                   Event Lead & Problem Setter; authored 7 original OOP/DSA problems
● Lakshya 2.0 National Hackathon   Top 10 finalist out of 400+ participants and 120 teams
● AWS Academy                      Cloud Foundations certified (EC2, S3, RDS, VPC, IAM)
```

---

### $ tail -n 5 currently_learning.log

```
[CURRENT_FOCUS] Deepening Go internals, runtime scheduler ergonomics, and concurrency primitives.
[CURRENT_FOCUS] Studying distributed consensus implementations (Raft state machines and log replication).
[CURRENT_FOCUS] Refining telemetry architectures with OpenTelemetry distributed trace context propagation.
```

---

### $ ./connect.sh

```
  Email      :  dhairyarupani31@gmail.com
  LinkedIn   :  https://linkedin.com/in/dhairya-rupani
  GitHub     :  https://github.com/Dhairya0531
  Portfolio  :  https://dhairya0531.github.io/portfolio/
  LeetCode   :  https://leetcode.com/u/Dhairya05/
  Codeforces :  https://codeforces.com/profile/Dhairya05
```

<div align="center">
  <sub>Designed with Unix CLI simplicity. Built for performance and reliability.</sub>
</div>
