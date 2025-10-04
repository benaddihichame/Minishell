# Minishell

Un shell minimaliste implémenté en C, inspiré de bash. Ce projet reproduit les fonctionnalités de base d'un shell Unix.

## 📋 Table des matières

- [À propos](#à-propos)
- [Fonctionnalités](#fonctionnalités)
- [Installation](#installation)
- [Utilisation](#utilisation)
- [Architecture](#architecture)
- [Commandes intégrées](#commandes-intégrées)
- [Exemples](#exemples)
- [Contributeurs](#contributeurs)
- [Licence](#licence)

## 🎯 À propos

Minishell est un projet de l'école 42 qui consiste à créer un interpréteur de commandes simple mais fonctionnel. Le but est de comprendre le fonctionnement interne d'un shell et d'implémenter ses fonctionnalités principales.

## ✨ Fonctionnalités

### Fonctionnalités principales
- **Prompt interactif** avec historique des commandes (readline)
- **Exécution de programmes** avec recherche dans le PATH
- **Gestion des pipes** (`|`) pour chaîner les commandes
- **Redirections** :
  - `<` : redirection d'entrée
  - `>` : redirection de sortie
  - `>>` : redirection de sortie en mode append
  - `<<` : heredoc
- **Variables d'environnement** et expansion (`$VAR`)
- **Gestion des signaux** (Ctrl+C, Ctrl+D, Ctrl+\\)
- **Gestion des quotes** simples et doubles

### Commandes intégrées (builtins)
- `echo` avec option `-n`
- `cd` avec gestion des chemins relatifs et absolus
- `pwd` pour afficher le répertoire courant
- `export` pour définir des variables d'environnement
- `unset` pour supprimer des variables d'environnement
- `env` pour afficher l'environnement
- `exit` pour quitter le shell

## 🚀 Installation

### Prérequis
- GCC ou tout autre compilateur C compatible
- GNU Make
- Bibliothèque readline (`libreadline-dev` sur Ubuntu/Debian)

### Compilation
```bash
# Cloner le repository
git clone https://github.com/benaddihichame/Minishell.git
cd Minishell

# Compiler le projet
make

# Nettoyer les fichiers objets (optionnel)
make clean
```

### Installation de readline (si nécessaire)
```bash
# Sur Ubuntu/Debian
sudo apt-get install libreadline-dev

# Sur macOS avec Homebrew
brew install readline
```

## 💻 Utilisation

### Lancement du shell
```bash
./minishell
```

### Utilisation interactive
```bash
minishell$ echo "Hello World!"
Hello World!

minishell$ ls -la | grep minishell
-rwxr-xr-x 1 user user 123456 Oct  4 10:30 minishell

minishell$ export MY_VAR="test"
minishell$ echo $MY_VAR
test

minishell$ cat << EOF > output.txt
> Hello
> World
> EOF

minishell$ exit
```

## 🏗️ Architecture

Le projet est organisé en plusieurs modules :

```
minishell/
├── include/
│   └── minishell.h          # Headers et définitions
├── src/
│   ├── builtins/            # Commandes intégrées
│   ├── execution/           # Exécution des commandes
│   ├── expander/            # Expansion des variables
│   ├── lexer/               # Analyse lexicale
│   ├── parser/              # Analyse syntaxique
│   ├── signals/             # Gestion des signaux
│   └── utils/               # Fonctions utilitaires
├── Makefile                 # Fichier de compilation
├── minishell.c              # Point d'entrée principal
└── README.md               # Documentation
```

### Modules principaux

#### Lexer
- Tokenisation de l'entrée utilisateur
- Reconnaissance des opérateurs et mots-clés
- Gestion des quotes et échappements

#### Parser
- Construction de l'arbre syntaxique
- Validation de la syntaxe
- Gestion des priorités d'opérateurs

#### Expander
- Expansion des variables d'environnement
- Gestion des substitutions
- Traitement des wildcards (si implémenté)

#### Execution
- Lancement des processus
- Gestion des pipes et redirections
- Contrôle des processus fils

#### Builtins
- Implémentation des commandes intégrées
- Gestion des options et arguments
- Manipulation de l'environnement

## 🔧 Commandes intégrées

### `echo [-n] [args...]`
Affiche les arguments séparés par des espaces.
- `-n` : supprime le saut de ligne final

### `cd [directory]`
Change le répertoire courant.
- Sans argument : retourne au répertoire home
- Gère les chemins relatifs et absolus

### `pwd`
Affiche le chemin absolu du répertoire courant.

### `export [name[=value]...]`
Définit ou affiche les variables d'environnement.
- Sans argument : affiche toutes les variables exportées
- Avec argument : définit la variable

### `unset [name...]`
Supprime les variables d'environnement spécifiées.

### `env`
Affiche toutes les variables d'environnement.

### `exit [status]`
Quitte le shell avec le code de retour spécifié (0 par défaut).

## 📚 Exemples

### Pipes et redirections
```bash
# Pipe simple
minishell$ ls -la | grep .c

# Redirection de sortie
minishell$ echo "Hello" > file.txt

# Redirection d'entrée
minishell$ cat < file.txt

# Append
minishell$ echo "World" >> file.txt

# Heredoc
minishell$ cat << END
> Hello
> World
> END
```

### Variables d'environnement
```bash
# Définir une variable
minishell$ export USER_NAME="John"

# Utiliser une variable
minishell$ echo "Hello $USER_NAME"

# Supprimer une variable
minishell$ unset USER_NAME
```

### Commandes complexes
```bash
# Combinaison de pipes et redirections
minishell$ cat file.txt | grep "pattern" | sort > sorted_results.txt

# Utilisation de variables dans les commandes
minishell$ export LOG_FILE="app.log"
minishell$ echo "Starting application" >> $LOG_FILE
```

## 👥 Contributeurs

- [yzioual](https://github.com/yzioual) - Développeur principal
- [benaddihichame](https://github.com/benaddihichame) - Développeur principal

## 📜 Licence

Ce projet est développé dans le cadre du cursus de l'école 42. Il est libre d'utilisation à des fins éducatives.

---

## 🐛 Debugging

Pour déboguer le programme :

```bash
# Compilation en mode debug
make debug

# Utilisation avec gdb
gdb ./minishell

# Utilisation avec valgrind
valgrind --leak-check=full ./minishell
```

## 📝 Notes de développement

- Le projet utilise une arena d'allocation pour la gestion mémoire
- Les signaux sont gérés de manière non-interactive
- Le parsing suit une approche récursive descendante
- L'exécution utilise fork/exec pour les processus externes

## 🔍 Tests

Pour tester le shell, vous pouvez utiliser des scripts de test ou comparer le comportement avec bash :

```bash
# Test de base
echo "echo Hello" | ./minishell

# Comparaison avec bash
echo "ls | wc -l" | bash
echo "ls | wc -l" | ./minishell
```