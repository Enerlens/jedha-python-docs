# jedha-python-docs

Documentation Python indexée par l'assistant pédagogique (RAG n8n + Supabase) pour les étudiants débutants de Jedha.

- `demo-day/` : les fichiers indexés (.md, .pdf, .txt). Chaque push sur `main` déclenche la ré-indexation des fichiers ajoutés, modifiés ou supprimés.
- Notebooks : convertis avec `jupyter nbconvert --to markdown --ClearOutputPreprocessor.enabled=True --output-dir demo-day/<dossier> <notebook>.ipynb`.


## `demo-day/python-data-science-handbook/` : Python Data Science Handbook

Dossier `notebooks_v1` du [Python Data Science Handbook](https://github.com/jakevdp/PythonDataScienceHandbook) de Jake VanderPlas (commit `d662314`), converti en Markdown. Les sorties texte des cellules sont conservées, les images de sortie retirées, et les liens entre chapitres pointent vers les `.md`. `figures/` est copié tel quel ; `data/`, `helpers_05_08.py` et `Untitled.ipynb` (vide) sont exclus.

Licences d'origine, inchangées : texte sous [CC BY-NC-ND 3.0](demo-day/python-data-science-handbook/LICENSE-TEXT), code sous [MIT](demo-day/python-data-science-handbook/LICENSE-CODE). Usage non commercial uniquement.

## `demo-day/python.org/` : documentation officielle Python

Les 36 PDF de la [documentation officielle Python](https://docs.python.org/3.13/download.html) en version 3.13.7 (générés le 1er octobre 2025) : tutoriel, référence du langage, bibliothèque standard, API C, extensions, installation, utilisation, FAQ, nouveautés et les 27 guides « HOWTO ». Fichiers non modifiés.

Licence d'origine : [PSF License](https://docs.python.org/3.13/license.html), et les exemples de code sous Zero-Clause BSD.
