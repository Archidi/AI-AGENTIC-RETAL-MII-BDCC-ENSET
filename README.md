# AI Agentic — Master BDCC

Ce dépôt regroupe l'ensemble des travaux pratiques (Labs et TP) réalisés dans le cadre du module **SMA et IAD** du **Master BDCC**, encadré par **Prof. RETAL SARA**.

## Organisation du projet

| Dossier | Description |
|----------|-------------|
| [Lab1-prompt-engineering](./Lab1-prompt-engineering) | Découverte du prompt engineering : tokenisation, Ollama, Groq, OpenAI, génération JSON et images. |
| [Lab2-langchain-agents](./Lab2-langchain-agents) | Développement d'agents avec LangChain : mémoire, recherche web et assistant cuisinier. |
| [Lab3-RAG](./Lab3-RAG) | Mise en œuvre d'un système RAG sur des fichiers PDF avec HuggingFace Embeddings, ainsi qu'un agent SQL basé sur la base Chinook. |
| [Lab4-MCP](./Lab4-MCP) | Découverte du Model Context Protocol (MCP) : communication stdio, serveur de temps et streaming HTTP. |
| [Lab5-LangGraph_Studio](./Lab5-LangGraph_Studio) | Utilisation de LangGraph Studio pour visualiser et déboguer des agents, ainsi que la conception d'un système multi-agents hiérarchique. |
| [Lab6-Contexte_et_Etat](./Lab6-Contexte_et_Etat) | Gestion du contexte d'exécution (`ReaderProfile`) et de l'état persistant (`LibraryState`). |
| [Lab7-Human_In_The_Loop](./Lab7-Human_In_The_Loop) | Réalisation d'un agent avec validation humaine grâce à `interrupt()`, `approve`, `reject` et `edit`. |
| [Lab8-Workflow_avec_LangGraph](./Lab8-Workflow_avec_LangGraph) | Construction de workflows LangGraph : états, reducers, branchements conditionnels et boucles. |
| [Lab9-Agent_avec_LangGraph](./Lab9-Agent_avec_LangGraph) | Création d'un agent complet avec LangGraph : outils, historique, Human-In-The-Loop et mécanisme de fork. |
| [TP-Chef_personnel](./TP-Chef_personnel) | Projet final : agent chef cuisinier intégrant un system prompt, la mémoire, le RAG et la recherche web. |

## Prérequis

Avant d'exécuter les différents labs, assurez-vous d'avoir installé :

- Python 3.10 ou une version supérieure
- **uv** pour la gestion des dépendances
- **Ollama** avec le modèle `llama3.2:3b`

```bash
ollama pull llama3.2:3b
```

## Exécution d'un Lab

Chaque dossier est indépendant et possède son propre environnement.

```bash
cd Lab6-Contexte_et_Etat
uv sync
uv run --active python agent_context.py
uv run --active python agent_state.py
```

Le même principe s'applique aux autres Labs : il suffit d'entrer dans le dossier correspondant, d'installer les dépendances puis d'exécuter le programme souhaité.

## Informations complémentaires

- Les fichiers `.env` ne doivent pas être versionnés sur GitHub. Utiliser les fichiers `.env.example` fournis dans les différents dossiers.
- Certains exercices nécessitent des clés API facultatives, notamment `TAVILY_API_KEY` et `LANGSMITH_API_KEY`.
- Les Labs peuvent être exécutés indépendamment les uns des autres, ce qui facilite leur test et leur compréhension.