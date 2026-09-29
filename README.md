# Dérive thématique dans la presse française — 2014 vs 2024

Analyse comparative des sujets traités dans la presse française à dix ans d'écart
(janvier, février et septembre 2014 vs 2024), pour repérer les thématiques
récurrentes ("marronniers" journalistiques) et l'évolution des sujets d'actualité.

## Méthode

```
Corpus d'articles (JSON, par mois/année)
        │
        ▼
[1] Nettoyage + filtrage TF-IDF (stopwords français + termes du domaine)
        │
        ▼
[2] Modélisation de sujets (LDA, gensim) — 10 topics par période
        │
        ▼
[3] Comparaison inter-période par similarité cosinus entre vecteurs de topics
        │
        ▼
[4] Identification des meilleurs matchs (topics les plus proches entre 2014 et 2024)
```

Le notebook (`notebooks/topic_drift_lda.ipynb`) traite trois paires de périodes en
parallèle : janvier, février et septembre, chacune comparée entre 2014 et 2024.

## Résultats

Matchs thématiques les plus forts (similarité cosinus) par période :

| Période | Meilleur match | Similarité |
|---|---|---|
| Septembre 2014 ↔ 2024 | *(sans, vie, monde, quand, dont)* | **0.89** |
| Février 2014 ↔ 2024 | *(sans, vie, faire, quand, monde)* | **0.87** |
| Janvier 2014 ↔ 2024 | *(directeur, général, président...)* | **0.85** |

Ces scores élevés sur des topics thématiquement vagues ("sans, vie, monde, quand")
suggèrent des structures de discours récurrentes plutôt que des marronniers
thématiques précis — voir les graphiques complets pour le détail par sujet.

Visualisations complètes dans [`results/`](results/) :
- `results/heatmaps/` — matrices de similarité complètes (10×10 topics) par période
- `results/matchs/` — meilleurs appariements de topics, triés par similarité

## Stack technique

- **Prétraitement** : NLTK (stopwords français), TF-IDF (scikit-learn)
- **Topic modeling** : LDA (gensim), 10 topics par période, 5 passes
- **Comparaison** : similarité cosinus sur les distributions de mots par topic
- **Visualisation** : matplotlib

## Installation

```bash
git clone <url-du-repo>
cd topic-drift-repo
pip install -r requirements.txt
python -c "import nltk; nltk.download('stopwords')"
```

## Données

**Le corpus source (fichiers JSON d'articles de presse) n'est pas inclus dans ce
dépôt** — seuls le code et les résultats agrégés (visualisations) sont versionnés.

Pour reproduire l'analyse, fournissez 6 fichiers JSON nommés `AAAAMM.json`
(`201401.json`, `202401.json`, `201402.json`, `202402.json`, `201409.json`,
`202409.json`), chacun structuré comme :

```json
{
  "id_article": {
    "journal_clean": "nom_du_journal",
    "texte": "contenu de l'article..."
  }
}
```

Vérifiez les droits de réutilisation de votre corpus avant toute redistribution.

## Limites

- LDA avec un nombre fixe de topics (10) par période — le choix du nombre de
  topics influence directement les résultats et n'a pas été optimisé
  systématiquement (grid search sur la cohérence, par exemple).
- La comparaison inter-période repose sur un vocabulaire réaligné a posteriori,
  ce qui peut favoriser les topics à vocabulaire générique dans les scores
  de similarité.
- Trois mois seulement (jan/fév/sept) — pas nécessairement représentatif de
  l'année complète.

## Licence

MIT — voir [LICENSE](LICENSE).
