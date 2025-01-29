[![Publish Documentation](https://github.com/Hermine-HRC/documentation/actions/workflows/publish_documentation.yml/badge.svg)](https://github.com/Hermine-HRC/documentation/actions/workflows/publish_documentation.yml)

# Documentation

Voici la documentation de l'équipe HRC.

Elle est générée sur GtiHub en page web à l'aide de Sphinx.
Vers la [documentation](https://hermine-hrc.github.io/documentation/).

# Contribuer

La documentation est écrite en ReStructuredText puis générée en HTML avec Sphinx.

## Installation

```bash
sudo apt install python3-pip
python3 -m venv .venv

# pour activer l'environnement virtuel
source .venv/bin/activate # In Linux
.venv/Scripts/actviate.bat # In Windows CMD
.venv/Scripts/Actviate.ps1 # In Windows Powershell

pip3 install -r requirements.txt # pour installer les dépendances

deactivate # pour désactiver l'environnement virtuel
```

## Compilation

Dans le dossier racine de la documentation, exécutez la commande suivante :

```bash
make html
```

Pour voir la documentation, ouvrez le fichier `_build/html/index.html` dans votre navigateur.

> [!TIP]
> Sur Linux, vous pouvez exécuter la commande ci-dessous pour compiler et ouvrir la documentation dans votre navigateur :
> ```bash
> make html && xdg-open _build/html/index.html
> ```


