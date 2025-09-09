# Smart Enumeration (RL-assisted Subdirectory Enumeration)

This project explores **smart, learning-based subdirectory enumeration** for web targets. Instead of brute-forcing fixed wordlists, an RL agent learns to propose **better search queries and crawl paths** by reading the page context, scraping results, and updating its internal state. A companion notebook visualizes metrics and results.

> **Notebooks**
> - `Smart_Enumeration_Draft.ipynb` – core pipeline and training loop for the RL-assisted enumerator (data collection, model setup, environment, training, and logging).
> - `Enumeration_Results.ipynb` – result analysis and visualization (plots, tables, summaries).

---

## Features

- **Custom Gymnasium Environment**
  - Tracks current URL/context and discovered links
  - Actions: search, scrape, follow link, propose next query
  - Rewards informative actions (e.g., discovering plausible subdirectories)

- **Policy & Reasoning**
  - Uses a small LLM (`deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B`) via 🤗 Transformers
  - Suggests search queries and next steps
  - Summarizes or prioritizes links

- **Learning**
  - Trains with **Stable-Baselines3** (PPO by default)
  - Logs metrics such as average distance and reward over time

- **Scraping & Search**
  - `requests` + `bs4` (BeautifulSoup)
  - DuckDuckGo HTML endpoint for search

- **Results**
  - Exports metrics to `training_results.csv`
  - Plots performance in `Enumeration_Results.ipynb`

---

## Repository structure

```
.
├── Smart_Enumeration_Draft.ipynb    # RL environment, LLM-assisted actions, training & logging
├── Enumeration_Results.ipynb        # Visualizations & analysis of training_results.csv
└── training_results.csv             # (created during runs) metrics over time
```

---

## Requirements

**Python**: 3.10+ recommended

**Core dependencies:**
- `torch`, `transformers`
- `stable-baselines3`, `gymnasium`
- `requests`, `beautifulsoup4`
- `numpy`, `pandas`, `matplotlib`, `seaborn`

Optional:
- `flash-attn` (GPU only)

---

## Quick start

1. **Create and activate a virtual environment**
   ```bash
   python -m venv .venv
   source .venv/bin/activate     # Windows: .venv\Scripts\activate
   ```

2. **Install dependencies**
   ```bash
   pip install "transformers==4.37.2" torch stable-baselines3 gymnasium requests beautifulsoup4 numpy pandas matplotlib seaborn
   # Optional (GPU only, and only if compatible)
   pip install flash_attn
   ```

3. **Run training**
   - Open `Smart_Enumeration_Draft.ipynb`
   - Set the target URL and training parameters
   - Run all cells → generates `training_results.csv`

4. **Analyze results**
   - Open `Enumeration_Results.ipynb`
   - Run all cells → plots metrics and summarizes results

---

## Customization

- **Model**: Replace the default Hugging Face model with any small LLM.
- **RL parameters**: Adjust PPO hyperparameters in the training notebook.
- **Reward shaping**: Modify the environment reward function to prioritize your goals.
- **Search/Scraping**: Swap DuckDuckGo or extend scraping logic.

---

## Troubleshooting

- **`flash_attn` install fails**: optional.
- **Out of memory (GPU)**: Use a smaller LLM, reduce batch sizes, or run CPU-only.
- **Slow CPU runs**: GPU is recommended.
- **HTTP/429 errors**: Add polite delays or caching. Follow site policies.

---

## Ethics & Legal

This project is for **research and defensive security**.  
Only enumerate or scrape **targets you own or have explicit permission to test**.  
Respect robots.txt and site terms.


