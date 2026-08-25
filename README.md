# bench-corpora

Corpus de benchmarks tiers, vendorisés dans un dépôt unique et consommés par
[`benchmark-emile`](https://github.com/FleuretAI/benchmark-emile).

Un sous-dossier par source amont — corpus de challenges (`xben/`) ou application
cible réelle (`fider/`). Pas de sous-modules, pas de forks éparpillés : le contenu
est copié ici, et `SOURCES.md` dit d'où il vient.

## Pourquoi un dépôt unique

Un seul SHA épingle l'état de **tous** les corpus à la fois. Une campagne de bench
devient reproductible en citant un seul numéro, au lieu d'un SHA par fork.

## Le consommer sans tout télécharger

Ce dépôt est volumineux (XBEN pèse à lui seul ~150 Mo). On ne le clone jamais en entier :

```bash
git clone --filter=blob:none --sparse https://github.com/messyhat555/bench-corpora.git external/bench-corpora
```

Puis, depuis ce clone, on sélectionne le seul corpus voulu :

```bash
git sparse-checkout set xben
```

Git ne télécharge alors que les blobs de `xben/`.

## Ajouter un corpus

```bash
git fetch <url-amont> <ref>
git read-tree --prefix=<nom>/ -u FETCH_HEAD
git commit -m "vendor(<nom>): import de <amont> @<sha>"
```

Puis ajouter la ligne correspondante dans `SOURCES.md`. Le statut (`SUIVI` ou `GELÉ`)
n'est pas décoratif : c'est lui qui dit si quelqu'un a le droit de remettre à jour
le dossier depuis l'amont.
