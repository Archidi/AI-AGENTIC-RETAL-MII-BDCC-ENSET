# TP – Agent Chef Cuisinier Personnel

**Master BDCC – SMA et IAD**
**Professeur : RETAL SARA**

---

# Objectif

L'objectif de ce TP est de développer un agent intelligent capable d'aider un utilisateur à cuisiner avec les ingrédients qu'il possède déjà.

L'agent doit être capable de comprendre les besoins de l'utilisateur, de mémoriser certaines informations importantes (préférences, allergies, régime alimentaire), de rechercher des recettes dans une base locale grâce au RAG et, si besoin, de compléter ses réponses avec une recherche sur Internet.

---

# Fonctionnalités

L'agent permet notamment de :

* proposer des recettes à partir des ingrédients disponibles ;
* mémoriser les préférences alimentaires de l'utilisateur ;
* prendre en compte les allergies et les restrictions alimentaires ;
* retrouver les préférences enregistrées lors des prochaines conversations ;
* rechercher des recettes dans une base locale grâce au RAG ;
* utiliser une recherche web lorsque les recettes locales ne suffisent pas.

---

# Structure du projet

```text
TP-Chef_personnel/
│
├── chef_agent.py
├── recipes.txt
├── pyproject.toml
├── .env.example
└── .gitignore
```

## Description des fichiers

**chef_agent.py**

Contient le code principal de l'agent, les outils disponibles ainsi que la boucle de conversation.

**recipes.txt**

Contient une quinzaine de recettes servant de base de connaissances locale.

**pyproject.toml**

Décrit les dépendances du projet.

**.env.example**

Exemple de fichier d'environnement contenant la configuration nécessaire.

**.gitignore**

Liste les fichiers qui ne doivent pas être versionnés.

---

# Prérequis

Avant de lancer le projet, il faut disposer de :

* Python 3.10 ou supérieur ;
* uv ;
* Ollama ;
* du modèle **llama3.2:3b** installé dans Ollama.

Une clé API Tavily peut également être ajoutée afin d'activer la recherche web.

---

# Installation

Installer les dépendances :

```bash
uv sync
```

Créer le fichier d'environnement :

```bash
cp .env.example .env
```

Si vous souhaitez utiliser la recherche web, renseignez votre clé API Tavily dans le fichier `.env`.

---

# Fonctionnement général

Lorsque l'utilisateur envoie un message, l'agent suit les étapes suivantes :

1. récupère les préférences déjà enregistrées ;
2. enregistre les nouvelles préférences si nécessaire ;
3. recherche des recettes dans la base locale ;
4. effectue une recherche web uniquement si cela est utile ;
5. propose plusieurs recettes adaptées à la situation.

L'objectif est de fournir des réponses personnalisées tout en respectant les goûts et les contraintes alimentaires de l'utilisateur.

---

# Base de connaissances (RAG)

Les recettes sont stockées dans le fichier **recipes.txt**.

Au démarrage de l'application, ce fichier est découpé en plusieurs morceaux (chunks). Chaque morceau est transformé en embedding afin de permettre une recherche sémantique.

Cette méthode permet de retrouver les recettes les plus pertinentes même si les mots utilisés par l'utilisateur sont différents de ceux présents dans le document.

Les embeddings utilisés sont basés sur le modèle :

```text
sentence-transformers/all-MiniLM-L6-v2
```

Ils sont ensuite stockés dans un **InMemoryVectorStore**.

---

# Gestion de la mémoire

L'agent possède une mémoire simple permettant de conserver certaines informations importantes concernant l'utilisateur.

Par exemple :

* végétarien ;
* végétalien ;
* cuisine italienne ;
* cuisine marocaine ;
* allergie aux arachides ;
* sans gluten.

Ces informations sont sauvegardées dans un état personnalisé (`ChefState`) puis réutilisées automatiquement lors des prochaines demandes.

Ainsi, l'utilisateur n'a pas besoin de répéter ses préférences à chaque conversation.

---

# Outils disponibles

| Outil              | Description                                                      |
| ------------------ | ---------------------------------------------------------------- |
| searchRecipesRag   | Recherche une recette dans la base locale                        |
| searchWeb          | Recherche des recettes ou des techniques culinaires sur Internet |
| rememberPreference | Enregistre une préférence ou une allergie                        |
| getPreferences     | Retourne les préférences enregistrées                            |

---

# Exécution

Pour lancer le projet :

```bash
uv run --active python chef_agent.py
```

Une fois l'application démarrée, l'utilisateur peut échanger librement avec l'agent.

---

# Exemples d'utilisation

### Exemple 1

**Utilisateur**

Je suis végétarien et j'adore la cuisine italienne.

**Agent**

Très bien, je retiens ces informations.

Voici quelques recettes adaptées :

* Pasta al Pomodoro ;
* Ratatouille ;
* Pâtes au pesto.

---

### Exemple 2

**Utilisateur**

Je suis allergique aux arachides.

**Agent**

C'est enregistré.

Je ne proposerai plus de recettes contenant des arachides et je privilégierai des alternatives compatibles avec votre allergie.

---

### Exemple 3

**Utilisateur**

J'ai des pâtes, des tomates, de l'ail, du basilic, de l'huile d'olive et du parmesan.

**Agent**

Avec ces ingrédients, vous pouvez préparer :

* Pasta al Pomodoro ;
* Pâtes au basilic ;
* Pâtes au parmesan.

---

### Exemple 4

**Utilisateur**

J'ajoute des œufs et des épinards.

**Agent**

Vous pouvez également préparer :

* une pasta aux épinards ;
* une omelette aux épinards ;
* des pâtes crémeuses aux œufs et au parmesan.

---

# Prompt système

Le modèle reçoit un prompt lui indiquant son rôle de chef cuisinier personnel.

Il doit :

* répondre de manière naturelle ;
* mémoriser les préférences de l'utilisateur ;
* respecter les allergies ;
* utiliser la base de recettes avant d'effectuer une recherche web ;
* proposer des recettes adaptées aux ingrédients disponibles.

---

# Conclusion

Ce TP m'a permis de mettre en pratique plusieurs notions abordées en cours, notamment l'utilisation d'un modèle de langage, la recherche documentaire avec le RAG et la gestion d'une mémoire utilisateur.

Le résultat est un assistant capable de proposer des recettes personnalisées tout en tenant compte des goûts, des allergies et des ingrédients disponibles. Même avec une base de recettes relativement simple, l'agent fournit des réponses cohérentes et adaptées aux besoins de l'utilisateur.
