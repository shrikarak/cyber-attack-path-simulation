# AI for Cybersecurity: Simulating and Analyzing Cyber Attack Paths

**Copyright (c) 2026 Shrikara Kaudambady. All rights reserved.**

## 1. Introduction

Modern cybersecurity defense requires a proactive mindset. Instead of just building taller walls, security teams must "think like an attacker" to understand how their systems could be compromised from within. A key part of this is **Attack Path Management**, which involves identifying the likely paths an attacker would take to move from an initial entry point to a high-value "crown jewel" asset.

This project provides a Jupyter Notebook that demonstrates how to use AI and graph analytics to simulate and analyze these potential attack paths. By modeling the IT network as a graph, we can use pathfinding algorithms to find the "path of least resistance" and use this insight to prioritize our defensive actions.

## 2. The Solution Explained: Graph Theory and Dijkstra's Algorithm

The core of this solution is to abstract the complex reality of a corporate network into a simplified but powerful graph model, and then use a classic AI algorithm to navigate it.

### 2.1. Modeling the Network as a Graph

We represent the network using the `networkx` library:
*   **Nodes:** Each server, workstation, or important device is a node in the graph. Nodes have attributes, such as their value to an attacker (e.g., a Domain Controller is highly valuable) and a list of known vulnerabilities.
*   **Edges:** A connection between two nodes represents network access (e.g., a Web Server can communicate with an App Server).
*   **Edge Weights (The "Cost"):** This is the most critical part of the model. Each edge has a "weight" or "cost" that represents the difficulty for an attacker to make that move. In our simulation, this cost is a function of the target node's security posture:
    *   `cost = base_cost - (number_of_vulnerabilities * discount_factor)`
    *   This means moving to a server with many vulnerabilities is "cheaper" (less risky and difficult) for the attacker, representing the path of least resistance.

### 2.2. Finding the Attack Path with Dijkstra's Algorithm

Once the graph is built, we can ask a clear question: "What is the 'cheapest' path from the initial compromise point (e.g., a public Web Server) to the crown jewel asset (e.g., the Domain Controller)?"

**Dijkstra's algorithm** is a foundational AI pathfinding algorithm designed to find the shortest path between two nodes in a weighted graph. By applying it to our attack graph, we can instantly identify the most likely sequence of machines an attacker would compromise to achieve their objective.

### 2.3. The "What-If" Analysis

The true power of this simulation lies in its ability to perform what-if analysis. The notebook demonstrates this by:
1.  Running the simulation on the baseline network to find the initial, easiest attack path.
2.  Simulating a remediation action (e.g., "patch the vulnerability on the DevMachine"). This action modifies the graph by increasing the cost of moving to that specific node.
3.  Re-running the simulation to see how the attacker's optimal path changes. This provides a data-driven way to measure the impact of a specific security control and helps teams prioritize which fixes will provide the most defensive value.

## 3. How to Use the Notebook

### 3.1. Prerequisites

This project uses standard Python data science and graph analysis libraries. You will need:

```bash
pip install numpy networkx matplotlib
```

### 3.2. Running the Notebook

1.  Clone this repository:
    ```bash
    git clone https://github.com/shrikarak/cyber-attack-path-simulation.git
    cd cyber-attack-path-simulation
    ```
2.  Start the Jupyter server:
    ```bash
    jupyter notebook
    ```
3.  Open `attack_path_simulation.ipynb` and run the cells sequentially. The notebook will output the results of the two simulations and render visualizations of the network and the different attack paths.

## 4. Application in a Real-World Security Program

This notebook is a template for a powerful analytical tool for Blue Teams (defenders) and security architects.

1.  **Automated Model Generation:** The `nodes_data` and `edges_data` can be replaced with data from real systems. A script could be written to pull asset information from a CMDB (Configuration Management Database) and vulnerability data from a scanner (like Tenable or Qualys) to automatically generate an up-to-date attack graph.

2.  **Vulnerability Prioritization:** Instead of using a simple CVSS score, which only measures the severity of a single vulnerability in isolation, this simulation allows for **context-aware prioritization**. A medium-severity vulnerability on a server that lies on the cheapest path to a critical asset is a higher priority to fix than a high-severity vulnerability on an isolated, low-value server.

3.  **Evaluating Security Controls:** Security teams can model proposed changes (e.g., adding a firewall rule, which would remove an edge from the graph) and run the simulation to get a quantitative measure of how much that change increases the "cost" for an attacker, thus justifying the investment.
