# ScholarSearch — Real-Time Academic Literature Discovery Engine

ScholarSearch is a volatile, memory-resident search engine application engineered for the structural indexing, scoring, and optimized boolean retrieval of computer science and scientific research publications. Built from scratch using Python without depending on third-party search indexes or full-text lucene wrappers, the application streams real-time metadata payloads from the **official arXiv Atom API** and processes them through an end-to-end data mining and indexing pipeline.

---

## 🚀 Key Architectural Innovations

* **Linear Pointer Scroller Intersection Algorithm:** Rather than executing intersecting multi-term Boolean `AND` statements via standard nested loop operations—which cost an inefficient time complexity of $O(|Postings_1| \times |Postings_2|)$—ScholarSearch preserves postings lists in strictly sorted sequences. Parallel cursors scroll through these datasets simultaneously in a single pass, dropping intersection computational cost down to an optimal linear runtime complexity of $O(|Postings_1| + |Postings_2|)$.
* **Decoupled Multi-Faceted Ranking Framework:** Candidate records are ordered dynamically at runtime via a log-smoothed TF-IDF equation combined with structural title-matching weights and a normalized temporal recency bonus:
    $$\text{Final Score}(d) = \sum_{t \in Q} \text{TF-IDF}(t, d) + \left( 1.5 \times \text{TitleMatches}(Q_{\text{raw}}, d) \right) + \max\left(0, \frac{\text{Year}_d - 1990}{100}\right)$$
* **Google/Yahoo Style UI Paradigm:** Built over a responsive Gradio interface, the interface maps operations across an intentional user sequence: **Ingestion Pipeline First $\rightarrow$ Exploration Second**. This ensures that the local sparse inverted dictionary map is completely stable before any search queries occur.

---

## 🏗️ System Architecture & Data Flow

The system isolates modular responsibilities into independent object layers to maximize code portability and memory efficiency:

1.  **arXiv Data Harvester (`fetch_arxiv`):** Establishes an unauthenticated network connection to the official Atom XML endpoints to dynamically ingest research metadata, isolate publication dates, parse contributors, and resolve document source mappings.
2.  **Text Preprocessing Pipeline (`Preprocessor`):** Casts textual strings into case-folded tokens, applies a custom regex sanitization pattern (`\b[a-z][a-z0-9]*\b`), filters out high-frequency domain academic noise words (e.g., *paper, method, approach*), and applies the Porter Stemmer algorithm to isolate the underlying word roots.
3.  **Sparse Inverted Index Vault (`InvertedIndex`):** Tracks nested memory matrix blocks following a memory-efficient sparse topology:
    $$\text{term} \longrightarrow \text{doc\_id} \longrightarrow \{\text{"tf"}: \text{int}, \text{"positions"}: \text{list}\}$$
4.  **Central Routing Coordinator (`SearchEngine`):** Coordinates logic checks, parses incoming logical query streams, manages pointer scroller structures, and ranks candidate payloads using an $O(M \log M)$ Timsort operation.

---

## 🛠️ Installation & Environment Setup

ScholarSearch is natively portable and can be deployed inside a cloud runtime environment like Google Colab or on a local machine.

### Prerequisites
Ensure your local environment runs **Python 3.10** or a more recent version.

### Installation Steps
Clone this workspace repository to your directory, navigate to the folder, and deploy the required open-source infrastructure packages:

```bash
# Clone the repository workspace
git clone [https://github.com/your-username/scholarsearch.git](https://github.com/your-username/scholarsearch.git)
cd scholarsearch

# Deploy mandatory third-party packages via pip
pip install -r requirements.txt
