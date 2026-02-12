# Laboratory 4 – NLP for Cybersecurity

![Polito Logo](resources/logo_polito.jpg)

This repository collects the material produced for Laboratory 4 of the **AI and Cybersecurity** course. The lab focuses on **Natural Language Processing (NLP)** applied to Bash command analysis. The goal is to classify sequences of Bash commands into malicious tactics (based on the MITRE ATT&CK framework) to identify attacker intent.

## At a Glance

- **Goal**: Classify Bash command sessions into tactics (e.g., *Discovery, Execution, Persistence*).
- **Data**: Real-world Bash session logs labeled with tactics.
- **Workflow**: Tokenization → Embeddings (TF-IDF, Word2Vec) → Sequence Modeling (LSTM/GRU) → Classification.
- **Deliverables**: interactive notebooks under `lab/notebooks/` and a LaTeX report in `report/`.

## Repository Layout

```
Laboratory4/
├── lab/
│   ├── data/           # Dataset files (train.json, test.json)
│   ├── notebooks/      # Task-specific Jupyter notebooks (Task1–Task4)
│   └── Plots/          # Generated figures for the report
├── report/             # LaTeX report sources
├── resources/          # Logos and reference material
└── README.md           # This file
```

## Dataset: Bash Command Sessions
The dataset consists of user sessions, where each session is a sequence of Bash commands entered by a user (or attacker).
- **Training Data**: `train.json` (labeled sessions).
- **Test Data**: `test.json` (used for final evaluation).
- **Labels (Tactics)**: 
    - **Discovery**: Gathering information about the system (e.g., `ls`, `whoami`, `netstat`).
    - **Execution**: Running malicious code or binaries (e.g., `./malware`, `bash -i`).
    - **Persistence**: Maintaining access across restarts (e.g., modifying `crontab`, `.bashrc`).
    - **Defense Evasion**: Hiding traces (e.g., `rm .bash_history`, `history -c`).

## Notebook Guide & Key Experiments

| Task | Notebook | Focus | Key Findings |
| --- | --- | --- | --- |
| **Task 1** | `lab/notebooks/Task1.ipynb` | **Exploratory Data Analysis (NLP)** | - Analyzed command frequency and length distributions.<br>- Tokenization strategies: splitting by space vs special characters.<br>- Identified most common commands per tactic (e.g., `wget` for Execution). |
| **Task 2** | `lab/notebooks/Task2.ipynb` | **Feature Extraction & Baselines** | - **TF-IDF**: Effective for identifying tactic-specific keywords.<br>- **Word2Vec**: Learned semantic relationships between commands (e.g., `curl` is close to `wget`).<br>- **Baseline Models**: Logistic Regression and Random Forest on aggregated session vectors. |
| **Task 3** | `lab/notebooks/Task3.ipynb` | **Sequential Models (RNNs)** | - **LSTM/GRU**: Modeled the *order* of commands, which is crucial for intent.<br>- **Embedding Layer**: Learned dense representations for command tokens.<br>- **Results**: RNNs significantly outperformed baselines by capturing temporal dependencies. |
| **Task 4** | `lab/notebooks/Task4.ipynb` | **Advanced & Explainability** | - **Attention Mechanisms**: Highlighted which specific commands contributed most to the classification.<br>- **Confusion Matrix analysis**: Revealed overlap between *Discovery* and *Persistence* strategies.<br>- **Error Analysis**: Misclassified sessions often contained ambiguous or multi-purpose commands. |

### Highlights per Task

- **Text Processing (Task 1 & 2)**
    - Custom tokenization was required to handle bash syntax (flags, pipes `|`, redirects `>`).
    - Static embeddings (Word2Vec) captured functional similarities between tools.

- **Deep Learning for Sequences (Task 3)**
    - Treating a session as a "sentence" of commands proved highly effective.
    - Bidirectional LSTMs captured context from both past and future commands in a session.

- **Model Interpretability (Task 4)**
    - Attention weights showed that the model correctly focuses on "payload" commands (e.g., downloading a file) rather than common utility commands (e.g., `cd`, `ls`).

## Reproducing the Experiments

1. **Environment**: Install standard data science and NLP libraries (`pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`, `torch`, `gensim`, `nltk`).
2. **Execution**: Run notebooks in order (`Task1.ipynb` $\to$ `Task4.ipynb`).
    - *Note*: Seed setting is included for reproducibility.
3. **Data**: Ensure `train.json` and `test.json` are in `lab/data/`.

## Authors

| Name | GitHub | LinkedIn | Email |
| --- | --- | --- | --- |
| Renato Mignone | [![GitHub](https://img.shields.io/badge/GitHub-Profile-informational?logo=github)](https://github.com/RenatoMignone) | [![LinkedIn](https://img.shields.io/badge/LinkedIn-Profile-blue?logo=linkedin)](https://www.linkedin.com/in/renato-mignone/) | [![Email](https://img.shields.io/badge/Email-Send-blue?logo=gmail)](mailto:renato.mignone@studenti.polito.it) |
| Claudia Sanna | [![GitHub](https://img.shields.io/badge/GitHub-Profile-informational?logo=github)](https://github.com/sannaclaudia) | [![LinkedIn](https://img.shields.io/badge/LinkedIn-Profile-blue?logo=linkedin)](https://www.linkedin.com/in/claudiasanna1/) | [![Email](https://img.shields.io/badge/Email-Send-blue?logo=gmail)](mailto:claudia.sanna@studenti.polito.it) |
| Chiara Iorio | [![GitHub](https://img.shields.io/badge/GitHub-Profile-informational?logo=github)](https://github.com/iochia02) | [![LinkedIn](https://img.shields.io/badge/LinkedIn-Profile-blue?logo=linkedin)](https://www.linkedin.com/) | [![Email](https://img.shields.io/badge/Email-Send-blue?logo=gmail)](mailto:chiara.iorio@studenti.polito.it) |
