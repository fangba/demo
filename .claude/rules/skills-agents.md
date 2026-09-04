# Règles — Skills & Agents

Ce fichier définit les conventions à respecter dans ce dépôt pour créer et organiser les **skills** (`.claude/skills/`) et les **agents** (`.claude/agents/`) de Claude Code.

## 1. Objet de ce fichier

Ce fichier sert de référence commune pour toute personne (ou tout agent) qui ajoute, modifie ou déclenche un skill ou un agent dans ce projet. Il fixe :

- où ranger les fichiers de skills et d'agents ;
- comment les nommer et les structurer ;
- dans quelles conditions un skill doit être déclenché plutôt qu'un traitement ad hoc.

Toute création de skill ou d'agent dans ce dépôt doit être conforme à ces règles. En cas de doute ou de conflit avec une autre instruction, ces règles priment pour tout ce qui concerne l'organisation des skills/agents — sauf instruction explicite contraire de l'utilisateur.

### Tableau entrée/sortie (obligatoire)

Tout fichier de skill (`SKILL.md`) ou d'agent (`.claude/agents/<nom>.md`) doit, **juste après le frontmatter** et avant le reste du contenu, comporter un tableau à une seule entrée décrivant ce que le skill/agent prend en entrée et ce qu'il rend en sortie :

```markdown
| Entrée | Sortie |
| --- | --- |
| Ce que le skill/agent prend (arguments, contexte, fichiers attendus, etc.) | Ce que le skill/agent rend (résultat produit : fichier créé/modifié, rapport, liste de findings, réponse texte, etc.) |
```

- **Entrée** : ce dont le skill/agent a besoin pour fonctionner (arguments passés, type de contenu attendu, pré-requis).
- **Sortie** : ce qu'il produit concrètement en fin d'exécution (le livrable, pas le déroulé des étapes).
- Ce tableau est obligatoire pour tout nouveau fichier créé dans ce dépôt, et doit être ajouté rétroactivement aux fichiers existants lors de leur prochaine modification.

## 2. Structure de nommage

### Agents (`.claude/agents/<nom>.md`)

- Un agent = un fichier Markdown, nommé en **kebab-case** (`code-improver.md`, `security-review.md`).
- Le nom de fichier doit correspondre exactement au champ `name` du frontmatter.
- Frontmatter obligatoire :
  ```yaml
  ---
  name: nom-en-kebab-case
  description: Description précise de ce que fait l'agent et quand l'utiliser (déclencheurs explicites).
  tools: Liste des outils autorisés, séparés par des virgules
  model: sonnet | opus | haiku (optionnel)
  ---
  ```
- Le corps du fichier décrit le rôle, la méthode de travail et les règles strictes de l'agent (ex. lecture seule, périmètre limité).

### Skills (`.claude/skills/<nom>/`)

- Un skill = un dossier en **kebab-case**, dont le nom devient la commande (`/nom-du-skill`).
- Le dossier contient au minimum un fichier `SKILL.md` avec une description claire de ce que fait le skill et des mots-clés de déclenchement.
- Les ressources annexes (scripts, gabarits, références) vivent dans le même dossier, jamais à la racine du dépôt.

### Principe commun

- Noms courts, explicites, en anglais ou en français selon la convention déjà en place dans le fichier concerné (les agents existants de ce dépôt sont documentés en français).
- Pas d'abréviations ambiguës : le nom doit permettre de deviner le rôle sans lire la description.

## 3. Conditions pour déclencher un skill

Un skill doit être invoqué (via l'outil `Skill`) uniquement quand **toutes** les conditions suivantes sont réunies :

1. La tâche demandée correspond clairement à la description d'un skill listé comme disponible — ne jamais deviner ou inventer un nom de skill.
2. Le skill est chargé **avant** de commencer le travail qu'il couvre, pas après coup en complément.
3. L'utilisateur n'a pas explicitly demandé de procéder autrement (dans ce cas, ses instructions priment).
4. Si l'utilisateur tape `/<nom-du-skill>`, c'est une demande explicite : le skill correspondant doit être invoqué directement, sans y substituer une autre approche.

Un skill ne doit **pas** être déclenché :

- pour une tâche ponctuelle et simple qui ne correspond à aucune description de skill disponible ;
- « au cas où » — si la pertinence n'est pas claire, mieux vaut traiter la demande directement ou demander une précision à l'utilisateur ;
- plusieurs fois pour la même tâche si un skill déjà invoqué (ou un agent déjà lancé) couvre encore le besoin.
