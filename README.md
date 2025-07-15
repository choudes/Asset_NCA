# Graph-Based Asset Criticality Analysis

This project provides a Python-based toolkit for identifying critical assets within a network. It moves beyond traditional methods by employing a composite analysis approach, combining network traffic analysis with user and asset context from Active Directory (AD). The result is a multi-faceted view of criticality that considers not only an asset's operational importance but also its security risk.

The script performs four independent analyses, each centered on a key graph theory metric: Degree, Betweenness, Closeness, and Eigenvector centrality.

## Key Features

* **Synthetic Data Generation:** Creates realistic, correlated datasets for Netflow, Zeek, and Active Directory to simulate an enterprise network.
* **Graph-Based Modeling:** Constructs a network graph from traffic logs to visualize relationships between assets.
* **Comprehensive Centrality Analysis:** Calculates four key centrality metrics to measure different aspects of an asset's importance.
* **Active Directory Correlation:** Enriches the analysis by incorporating crucial security context, such as user privilege levels and system roles (e.g., Domain Controller).
* **Tiered Matrix Assessment:** Uses a quantitative, rule-based matrix to determine a final criticality level (`CRITICAL`, `High`, `Medium`, `Low`).
* **Rich Visualizations:** Generates separate, interactive HTML graphs for each centrality analysis, allowing for easy exploration of results.

## How It Works

The analysis pipeline is executed in a series of automated steps:

### 1. Data Generation and Graph Construction

* The script first generates three CSV files: `netflow.csv`, `zeek.csv`, and `ad_data.csv`.
* `netflow.csv` and `zeek.csv` contain simulated network traffic, primarily in a client-to-server (hub-and-spoke) model.
* `ad_data.csv` provides the security context, assigning roles and privilege levels to each asset.
* A network graph is then constructed from the traffic logs, where IPs are nodes and communication events are edges.

### 2. Centrality Analytics and AD Correlation

The core of the analysis is an iterative process that is repeated for each of the four centrality metrics. For each metric:

1.  **Calculate Centrality Score:** A quantitative score is calculated for every node in the graph.
2.  **Assign Tiers:** Each asset is assigned two tiers based on the criteria below.
3.  **Assess Final Criticality:** The final criticality level is determined by looking up the intersection of the two tiers in the matrix.

#### The Tiered Matrix Model

The final criticality score is determined by combining the asset's Network Tier (operational impact) with its AD Tier (security risk).

* **AD Tier (Security Risk) Criteria:**
    * **Tier 0 (Crown Jewel):** Asset is a Domain Controller or has a 'Domain Admin' logon.
    * **Tier 1 (High-Value):** Asset has a 'Privileged User' logon.
    * **Tier 2 (Standard):** Asset has a 'Standard User' logon.
* **Network Tier (Operational Impact) Criteria:**
    * **Tier 1 (Critical Hub):** Centrality score is in the top 20% (>= 80th percentile).
    * **Tier 2 (Connector):** Centrality score is in the middle 20% (60th to 80th percentile).
    * **Tier 3 (Endpoint):** Centrality score is in the bottom 60% (< 60th percentile).

|                          | **Network Tier 1 (Critical Hub)** | **Network Tier 2 (Connector)** | **Network Tier 3 (Endpoint)** |
| :----------------------- | :-------------------------------- | :----------------------------- | :---------------------------- |
| **AD Tier 0 (Crown Jewel)** | CRITICAL                          | CRITICAL                       | High                          |
| **AD Tier 1 (High-Value)** | CRITICAL                          | High                           | Medium                        |
| **AD Tier 2 (Standard)** | High                              | Medium                         | Low                           |

## Installation

The project requires Python 3 and the following libraries. You can install them using pip:

```bash
pip install pandas numpy networkx pyvis
```

## Usage

To run the full analysis pipeline, simply execute the Python project Asset Criticality Analysis with AD Correlation (Full Centrality) at Google Colab:

The script will generate the synthetic data, perform the analysis, print the results to the console, and create the HTML visualization files in the same directory.

## Understanding the Output

### Console Reports

For each of the four centrality metrics, the script will print a report listing the **Top 5 Most Critical Assets**. This allows you to quickly see which assets are most important according to each analytical lens.

```
--- Top 5 Most Critical Assets (based on DEGREE) ---
Asset: 10.0.0.81       | Criticality: CRITICAL
Asset: 10.0.0.30       | Criticality: CRITICAL
...
```

### Interactive Visualizations

The script generates four HTML files, one for each analysis:

* `criticality_graph_degree.html`
* `criticality_graph_betweenness.html`
* `criticality_graph_closeness.html`
* `criticality_graph_eigenvector.html`

Open these files in a web browser to explore the interactive graph.

* **Node Color** represents the final criticality level (Red = CRITICAL).
* **Node Size** represents the asset's degree (number of connections).
* **Borders** highlight the top 5 assets for that specific analysis.
* **Hovering** over a node reveals a tooltip with its detailed centrality scores.
