# Évaluation finale – Agentic RAG
### Éducation financière personnelle au Maroc

Ce projet consiste à développer un système **RAG agentique** avec **LangGraph**, sans utiliser `create_agent`. L'objectif est de répondre à des questions liées à l'éducation financière au Maroc en s'appuyant sur une base documentaire officielle.

Les thèmes couverts sont notamment :

- Gestion du budget
- Épargne
- Crédit
- Investissement
- Marché des capitaux
- Assurance

---

## Technologies utilisées

Le projet repose sur les outils suivants :

- **LLM :** Ollama (`llama3.2:3b`)
- **Embeddings :** `sentence-transformers/all-MiniLM-L6-v2`
- **Base vectorielle :** Chroma (stockée dans `data/chroma_db/`)
- **Orchestration :** LangGraph avec `StateGraph`

---

## Installation

Installer les dépendances du projet :

```bash
uv sync --group dev
```

Ensuite, vérifier qu'Ollama est lancé et télécharger le modèle si nécessaire :

```bash
ollama pull llama3.2:3b
```

---

## Documents utilisés

Le système s'appuie sur quatre guides officiels publiés par des organismes marocains.

| Document | Organisme |
|----------|-----------|
| `bkam_guide_grand_public.pdf` | Bank Al-Maghrib |
| `ammc_guide_investisseur_circuits.pdf` | AMMC |
| `ammc_guide_instruments_financiers.pdf` | AMMC |
| `acaps_guide_assure.pdf` | ACAPS |

Déposer les fichiers PDF dans :

```text
data/pdfs/
```

Puis créer la base vectorielle :

```bash
uv run python ingest.py
```

Cette étape découpe les documents en plusieurs segments et les indexe dans Chroma.

---

## Démarrage

Pour lancer l'assistant :

```bash
uv run python main.py
```

Le programme ouvre un chat interactif. Chaque conversation possède son propre
`thread_id`, ce qui permet de conserver le contexte pendant toute la session.

---

## Fonctionnement du graphe

Le schéma du workflow peut être généré avec :

```bash
uv run python generate_graph.py
```

Cette commande produit :

- `graph.mmd`
- `graph.png` (si la génération est disponible)

Le fonctionnement général est le suivant :

1. L'agent analyse la question.
2. Il choisit soit de répondre directement, soit d'utiliser un outil.
3. Les documents récupérés sont évalués.
4. Si les résultats sont insuffisants, la requête est reformulée.
5. Une nouvelle recherche est effectuée (maximum deux tentatives).

La mémoire des conversations est gérée grâce à un `InMemorySaver`, associé au
`thread_id` de chaque utilisateur.

---

## Outils disponibles

Le projet met à disposition plusieurs outils :

### Recherche documentaire

```text
retrieve_documents(query)
```

Effectue une recherche sémantique dans la base Chroma afin de retrouver les passages les plus pertinents.

### Projection d'épargne

```text
compute_savings_projection(...)
```

Calcule l'évolution d'une épargne selon :

- le capital initial,
- le versement mensuel,
- le taux annuel,
- la durée.

### Calcul d'un crédit

```text
compute_loan_payment(...)
```

Retourne la mensualité d'un prêt ainsi que son coût total.

---

## Exécution des tests

Pour vérifier le bon fonctionnement du projet :

```bash
uv run pytest -v
```

---

## Évaluation

Le script suivant permet d'évaluer automatiquement les performances de l'agent :

```bash
uv run python -m evaluation.run_evaluation
```

L'évaluation comprend deux séries de questions :

- 10 questions simples
- 10 questions plus complexes

Pour chaque requête, le programme mesure le temps de réponse et conserve les documents utilisés comme sources.

Les résultats sont enregistrés dans :

```text
evaluation/results/results.csv
```