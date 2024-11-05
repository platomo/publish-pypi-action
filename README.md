
# Publish Python Package

Diese GitHub-Action erstellt und veröffentlicht ein Python-Paket auf TestPyPI.

## Beschreibung

Diese Action ist nützlich, wenn Sie Python-Pakete auf TestPyPI bereitstellen möchten, um sie vor der Veröffentlichung auf dem offiziellen PyPI-Repository zu testen. Sie baut das Paket, erstellt sowohl eine Binär-Rad- als auch eine Quell-Tarball-Datei und veröffentlicht diese auf der angegebenen TestPyPI-Instanz.

## Eingaben

| Name            | Beschreibung                          | Erforderlich | Standardwert |
|-----------------|--------------------------------------|--------------|--------------|
| `repository-url` | Die URL des TestPyPI-Repositories.   | Ja           | Keine        |

## Verwendung

Erstellen Sie eine Workflow-Datei (z. B. `.github/workflows/publish.yml`) und verwenden Sie diese Action wie folgt:

```yaml
name: Publish to TestPyPI

on:
  push:
    branches:
      - main

jobs:
  publish:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Publish Python Package
        uses: platomo/publish-pypi-action@main
        with:
          repository-url: https://test.pypi.org/legacy/
```

## Schritte im Workflow

1. **Installiere pypa/build**: Installiert das `build`-Paket, das für den Bau des Python-Pakets benötigt wird.
2. **Baue ein Rad und eine Quell-Tarball**: Baut das Paket und erstellt eine `.whl`- und eine `.tar.gz`-Datei im `dist/`-Ordner.
3. **Veröffentliche Paket auf TestPyPI**: Nutzt die [gh-action-pypi-publish](https://github.com/pypa/gh-action-pypi-publish), um die Paketdateien auf das angegebene TestPyPI-Repository hochzuladen.

## Erforderliche Berechtigungen

Um die Action erfolgreich auszuführen, muss ein Authentifizierungstoken in den Repository-Geheimnissen (`secrets`) gespeichert werden, um das Paket auf TestPyPI zu veröffentlichen.
