# jedha-python-docs

Documentation Python indexée par l'assistant pédagogique (RAG n8n + Supabase) pour les étudiants débutants de Jedha.

- `docs/` : les fichiers indexés (.md, .pdf, .txt). Chaque push sur `main` déclenche la ré-indexation des fichiers ajoutés, modifiés ou supprimés.
- Notebooks : convertis avec `jupyter nbconvert --to markdown --ClearOutputPreprocessor.enabled=True --output-dir docs <notebook>.ipynb`.


## `docs/notebooks_v1/` : Python Data Science Handbook

Dossier `notebooks_v1` du [Python Data Science Handbook](https://github.com/jakevdp/PythonDataScienceHandbook) de Jake VanderPlas (commit `d662314`), converti en Markdown. Les sorties texte des cellules sont conservées, les images de sortie retirées, et les liens entre chapitres pointent vers les `.md`. `figures/` est copié tel quel ; `data/`, `helpers_05_08.py` et `Untitled.ipynb` (vide) sont exclus.

Licences d'origine, inchangées : texte sous [CC BY-NC-ND 3.0](docs/notebooks_v1/LICENSE-TEXT), code sous [MIT](docs/notebooks_v1/LICENSE-CODE). Usage non commercial uniquement.
