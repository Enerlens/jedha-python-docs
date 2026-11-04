# jedha-python-docs

Documentation Python indexée par l'assistant pédagogique (RAG n8n + Supabase) pour les étudiants débutants de Jedha.

- `docs/` : les fichiers indexés (.md, .pdf, .txt). Chaque push sur `main` déclenche la ré-indexation des fichiers ajoutés, modifiés ou supprimés.
- Notebooks : convertis avec `jupyter nbconvert --to markdown --ClearOutputPreprocessor.enabled=True --output-dir docs <notebook>.ipynb`.

Contenu des cours : © Jedha.
