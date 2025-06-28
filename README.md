# Network Asset Centrality Analysis

This project provides a complete pipeline to analyze network traffic data and identify the most critical assets using graph theory. The script generates synthetic network flow (netflow) and Zeek log data, builds a network graph from this data, calculates various centrality metrics, and creates an interactive visualization of the most central nodes.

The entire process is automated within the `main.py` script. No external data files are needed to get started.

---

### Key Features

*   **Synthetic Data Generation:** Automatically creates `netflow.csv` and `zeek.csv` files with realistic-looking network traffic data.
*   **Graph Construction:** Parses the CSV files and builds a network graph where nodes are IP addresses and edges represent connections.
*   **Centrality Analysis:** Calculates four key centrality metrics to identify important nodes:
    *   **Degree Centrality:** Measures the number of direct connections a node has.
    *   **Betweenness Centrality:** Measures how often a node lies on the shortest path between other nodes.
    *   **Closeness Centrality:** Measures the average shortest distance from a node to all other nodes.
    *   **Eigenvector Centrality:** Measures the influence of a node in the network.
*   **Top Asset Reporting:** Prints a clean, ranked list of the most critical assets based on each centrality score.
*   **Interactive Visualization:** Generates an `network_graph.html` file using PyVis, allowing you to explore a subgraph of the most important network assets interactively.

---

### Prerequisites

You will need Python 3 and the following libraries.

*   `pandas`
*   `numpy`
*   `networkx`
*   `pyvis`

You can install all dependencies with a single command using pip:

```bash
pip install pandas numpy networkx pyvis
```

---

### How to Run

1.  **Clone the repository (or just save `main.py`):**
    ```bash
    git clone <your-repo-url>
    cd <your-repo-directory>
    ```

2.  **Execute the script:**
    Run the `main.py` file from your terminal.

    ```bash
    python main.py
    ```

---

### What to Expect (Output)

When you run the script, it will perform the following actions and produce these outputs:

1.  **Console Output:** You will see a step-by-step log printed to your console, showing the progress of:
    *   Data generation
    *   Graph building
    *   Centrality analysis results, including a "Top 5 Assets" report.

2.  **Generated Files:** The following files will be created in the same directory:
    *   `netflow.csv`: The synthetic netflow data.
    *   `zeek.csv`: The synthetic Zeek log data.
    *   `network_graph.html`: **This is the main visual output.** Open this file in your web browser to see and interact with the network graph visualization.

#### Example of the Visualization (`network_graph.html`):

![image](https://user-images.githubusercontent.com/1098293/175829288-750d4f58-06a9-4475-9614-2c6729e1333e.png)
*(Note: This is a sample image of a PyVis graph. Your graph will be interactive.)*

---

### How It Works

The script follows a simple, four-step pipeline defined in the `main()` function:

1.  **`generate_and_save_data()`**: Creates the synthetic data.
2.  **`load_data()` & `build_graph_from_data()`**: Reads the data and constructs a `networkx` graph.
3.  **`analyze_centrality()` & `print_top_assets()`**: Computes the centrality scores and displays the results.
4.  **`visualize_graph()`**: Exports the interactive HTML graph visualization of the most central nodes.
```
