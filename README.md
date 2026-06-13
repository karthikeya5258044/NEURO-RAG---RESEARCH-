🧠 NeuroRAG — Explainable EEG-Based Stress Detection via Retrieval-Augmented Generation


The first system combining EEG biomarker embeddings, clinical narrative generation, and RAG-based LLM reasoning for explainable stress assessment.





📌 What is NeuroRAG?

Most EEG stress detection systems work like this:

Brain signals → AI model → "Stressed" ✓

That's it. One word. No explanation. No evidence. No clinical value.

NeuroRAG works like this:

Brain signals → Biomarkers → Clinical Narrative → RAG Retrieval → LLM Assessment
                                                        ↑
                                          5 similar historical EEG cases

A clinician gets: why the person is stressed, which brain regions are involved, how it compares to historical cases, and what to do about it.


📊 Key Results (N = 112 EEG Recordings, 5 Conditions)

ConditionnMean StressMean TAR% HIGH_STRESSSCWT (Stroop Test)240.5022.50533.3%CMPS (Math Problems)220.4282.60313.6%TMCT (Mental Challenge)240.4182.31412.5%MUSIC (Relaxing Music)200.3321.80010.0%HORROR (Horror Video)220.3021.4119.1%

Retrieval Accuracy: 24.1% (27/112) vs 20% random baseline

🔑 Key Finding

Horror video stimulation produces LOW_STRESS labels (9.1% HIGH_STRESS) despite being emotionally intense. NeuroRAG correctly distinguishes prefrontal cognitive stress (TAR, FAA) from limbic emotional arousal — a clinically critical distinction invisible to black-box classifiers.


🏗️ System Architecture

┌─────────────────────────────────────────────────────────────┐
│                        NeuroRAG Pipeline                     │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  EEG Signals (AF3, T7, Pz, T8, AF4)                        │
│       │                                                      │
│       ▼                                                      │
│  Preprocessing (MNE-Python, bandpass 0.5-45Hz, notch 50Hz) │
│       │                                                      │
│       ▼                                                      │
│  Biomarker Extraction                                        │
│  ├── TAR  = Theta/Alpha Ratio     (cognitive fatigue)       │
│  ├── FAA  = Frontal Alpha Asymmetry (withdrawal/anxiety)    │
│  └── EI   = Engagement Index      (cognitive load)          │
│       │                                                      │
│       ▼                                                      │
│  Clinical Narrative Generation (rule-based templates)       │
│  "High TAR (2.50) — cognitive fatigue. Right frontal        │
│   alpha dominance — withdrawal motivation..."               │
│       │                                                      │
│       ▼                                                      │
│  35-dim Biomarker Embedding → SQLite Database               │
│       │                                                      │
│       ▼  (at inference time)                                 │
│  Cosine Similarity → Top-5 Historical Cases Retrieved       │
│       │                                                      │
│       ▼                                                      │
│  LLM (GPT-class) → Evidence-Grounded Clinical Report       │
│                                                              │
└─────────────────────────────────────────────────────────────┘


📁 Repository Structure

NeuroRAG/
├── neurorag_READY.ipynb      # Main pipeline: preprocessing → RAG → LLM
├── neurorag_EDA.ipynb        # EDA: stress distributions, confusion matrix
├── neurorag_summary.csv      # Quantitative results (all 112 recordings)
├── NeuroRAG_Paper.pdf        # Full research paper (Zenodo preprint)
└── README.md


⚙️ Setup & Installation

bashgit clone https://github.com/YOUR_USERNAME/NeuroRAG.git
cd NeuroRAG

pip install mne scipy numpy pandas matplotlib seaborn openai

Set your LLM API key:

python# In neurorag_READY.ipynb, Cell 1:
OPENROUTER_API_KEY = "your_key_here"

Run in order:


neurorag_READY.ipynb — builds the database and runs the RAG pipeline
neurorag_EDA.ipynb — generates all visualizations and the summary CSV



🧬 Biomarker Embedding (35 Dimensions)

DimsFeatureNeuroscience Meaning0–4Mean band powers (δ,θ,α,β,γ)Global brain state5–24Per-channel relative powersSpatial stress distribution25TARCognitive fatigue26FAAEmotional valence / withdrawal27EIActive cognitive load28Stress scoreComposite [0,1]29–33Absolute alpha per channelFrontal relaxation map34Frontal beta asymmetryHemispheric load imbalance


📄 Citation

If you use this work, please cite:

bibtex@misc{karthikeya2026neurorag,
  title   = {NeuroRAG: An Explainable EEG-Based Stress Detection System
             Using Retrieval-Augmented Generation},
  author  = {Karthikeya},
  year    = {2026},
  url     = {https://zenodo.org/YOUR_LINK_HERE},
  note    = {Zenodo Preprint}
}


🔬 Research Paper

📎 Full paper available on Zenodo: [INSERT YOUR ZENODO DOI LINK HERE]


👤 Author

Karthikeya
B.Tech CSE (Data Analytics) — VIT-AP University, India
AI Engineer | Agentic AI | LLM Systems





📜 License

This work is licensed under Creative Commons Attribution 4.0 International (CC BY 4.0).
