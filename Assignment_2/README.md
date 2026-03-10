# 🔍 Probing BERT for Negation in Natural Language Inference

This project investigates whether **BERT fine-tuned on MNLI** encodes information about **negation** inside its sentence representations.

The experiment follows a **probing methodology**:  
we extract hidden representations from BERT and train a lightweight classifier (probe) to detect which hypothesis in a pair is negated.

The goal is **not to improve BERT**, but to analyze what information is already encoded in its internal representations.

---

# 📚 Dataset

We use the **GLUE MNLI dataset**.

Only samples with labels:

- `entailment`
- `contradiction`

are used.

For each example:

1. Original hypothesis is parsed with **spaCy**
2. A **negated version** of the hypothesis is generated
3. The pair is passed through **BERT MNLI**
4. We keep only examples where **BERT prediction changes after negation**

This ensures the probe studies cases where negation actually affects the model behavior.

---

# 🧠 Model

We use the pretrained model:

```

textattack/bert-base-uncased-MNLI

```

Hidden states are extracted from multiple layers.

Example configuration:

```

LAYER_IDS = [-1, -4]

```

Instead of using only `[CLS]`, we compute a **mean pooled representation of hypothesis tokens**.

The final representation is:

```

concat(layer_-1_mean_pool, layer_-4_mean_pool)

```

---

# 🔬 Probing Task

Each example contains two hypotheses:

```

Premise
Hypothesis A
Hypothesis B

```

Where one hypothesis is the **original** and the other is the **negated version**.

To avoid positional bias, their order is randomized.

The probe must predict:

```

Is the FIRST or the SECOND hypothesis the negated one?

```

---

# 🧮 Probe Architecture

The probe is intentionally simple:

```

Linear Classifier

```

Input features:

```

[first_representation,
second_representation,
|difference|,
elementwise_product]

```

This keeps the probing setup interpretable and avoids giving the probe excessive modeling capacity.

---

# 📊 Evaluation

The probe is evaluated on a held-out validation set.

Metrics reported:

- Accuracy
- Confusion matrix
- Classification report

If accuracy is **significantly above 50%**, it indicates that BERT representations encode information related to negation.

---

# ⚙️ Project Structure

```

project/
│
├── Assignment2.ipynb
│
├── README.md
├── requirements.txt
│
└── artifacts/
├── probe_history.csv
├── train_swap_summary.csv
├── valid_swap_summary.csv
└── linear_probe.pt

````

---

# 🚀 Installation

Clone the repository:

```bash
git clone https://github.com/your-username/bert-negation-probing.git
cd bert-negation-probing
````

Install dependencies:

```bash
pip install -r requirements.txt
```

Download spaCy model:

```bash
python -m spacy download en_core_web_sm
```

---

# ▶️ Running the Experiment

Open the notebook:

```
Assignment2.ipynb
```

Run all cells sequentially.

The pipeline performs:

1️⃣ MNLI loading
2️⃣ Hypothesis negation generation
3️⃣ BERT inference
4️⃣ Hidden state extraction
5️⃣ Representation construction
6️⃣ Probe training
7️⃣ Evaluation

---

# 📈 Expected Outputs

The notebook generates artifacts:

```
artifacts/
```

including:

* probe training history
* prediction swap statistics
* trained probe weights

---

# ⚠️ Limitations

This experiment **does not prove** that BERT truly understands negation.

Probe success only indicates that **negation-related information is recoverable from representations**, which may arise from:

* lexical cues
* syntax patterns
* dataset biases