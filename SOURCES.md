# Provenance des corpus

Une ligne par sous-dossier. À tenir à jour à chaque import ou mise à jour.

| Dossier | Amont | Commit amont | Importé le | Statut |
|---|---|---|---|---|
| `xben/` | [messyhat555/validation-benchmarks](https://github.com/messyhat555/validation-benchmarks) | `cb8864f` | 2026-08-25 | SUIVI |
| `fider/` | [getfider/fider](https://github.com/getfider/fider) | `e084dff` (tag `v0.33.0`) | 2026-08-25 | GELÉ |
| `photoview/` | [photoview/photoview](https://github.com/photoview/photoview) | `f79d379` (tag `v2.4.0`) | 2026-08-25 | GELÉ |

## Statuts

- **SUIVI** — on remet à jour depuis l'amont de temps en temps. Réimporter est autorisé.
- **GELÉ** — snapshot figé à un commit précis. **Ne pas réimporter** : une campagne
  de bench en cours dépend de cet état exact.

## Images de conteneur épinglées

Même logique que le digest-pin d'XBEN : l'étiquette est gardée pour la lisibilité,
le digest fait autorité. Une étiquette peut être repoussée sur une autre image ;
un digest, non.

| Cible | Image | Digest |
|---|---|---|
| `fider/` | `getfider/fider:v0.33.0` | `sha256:72b844800fc68f6b665a2a5312a03c0ce62b9556b5a77375bb3eaee44b2656b7` |
| `photoview/` | `photoview/photoview:2.4.0` | `sha256:79229bd913b2453d93ebcb890b7e8c1dddd46215afee1ffa10c5fdb05ba3eb35` |

Les deux digests ont été résolus contre Docker Hub le 2026-08-25 et vérifiés.
Noter que l'étiquette Photoview est **`2.4.0` sans `v`** : `v2.4.0` n'existe pas
côté registre, alors que le tag Git, lui, s'écrit `v2.4.0`.

## Réserve : le tag Git n'est qu'une approximation du code testé

Pour `fider/` et `photoview/`, ce qui est reproductible avec certitude est le
**runtime** : les images ci-dessus sont exactement celles qui ont été testées.

Le **code source**, lui, l'est moins. Le rapport d'origine indique que les
plateformes ont reçu « our fork of the repo » sur « the specific scanner branch » —
fork et branche non documentés, donc introuvables. Le tag correspondant à l'image
est la meilleure approximation disponible, mais ce n'est **pas** strictement l'arbre
que les plateformes ont vu. En tenir compte avant de conclure quoi que ce soit d'une
comparaison ligne à ligne avec un finding du rapport.

## Détail

### `xben/` — XBEN 104

Les 104 challenges `XBEN-001-24` → `XBEN-104-24`, gate de la Phase 1 (épic
FleuretAI/fleuret-emile#16).

Chaîne de provenance : `xbow-engineering/validation-benchmarks` → fork
`arthurgervais/validation-benchmarks` → fork `messyhat555/validation-benchmarks`,
d'où vient cet import.

Patchs locaux déjà présents dans l'import, par rapport à l'upstream :

- 12 builds réparés + cible `build-all` (PR #1)
- `benchmark.json` : champ `level` normalisé en entier (PR #2)
- `docs/fork-fixes.md` : récapitulatif des correctifs du fork (PR #3)
- images de base épinglées par digest `image:tag@sha256`, 137 références sur
  136 fichiers (PR #4) — c'est le vrai levier anti-dérive des scores

Le corpus amont est périmé et largement cassé (dépôts Debian archivés, dépendances
mortes, Compose invalides). Il reste la référence du domaine : c'est pour ça qu'on
le répare au lieu d'en changer.

### `fider/` — application cible réelle

**Ce n'est pas un corpus de challenges.** C'est une application unique et réelle
([Fider](https://github.com/getfider/fider), plateforme de collecte de feedback,
Go + React/TypeScript), vendorisée pour servir de **cible** au bench — un vrai
logiciel plutôt qu'un challenge fabriqué. Cf. FleuretAI/fleuret-emile#158.

Épinglé au **tag `v0.33.0`** (`e084dff`), statut **GELÉ** : le tag est le point de
référence, changer de version changerait la cible et rendrait les scores
incomparables. Ne pas réimporter sans décision explicite.

Licence amont : **AGPL-3.0**. Le copyleft de l'AGPL se déclenche à la distribution
et à la mise à disposition en réseau, pas à l'usage interne. Un déploiement de
bench, local et non exposé, ne le déclenche pas.

#### Attention : fichiers d'instructions d'agent

L'arbre amont contient `fider/CLAUDE.md` et `fider/WARP.md` — des consignes pour
assistants de code, écrites par le projet amont. Ils sont **conservés tels quels**
pour que le snapshot corresponde exactement à `v0.33.0`.

Conséquence : n'ouvre pas de session d'agent depuis l'intérieur de `fider/`, ces
fichiers seraient chargés comme des consignes. Ce sont des données tierces, pas des
instructions à suivre — et à chaque montée de version, ce sont des fichiers à relire
avant de les accepter.

### `photoview/` — application cible réelle

**Ce n'est pas un corpus de challenges**, même remarque que pour `fider/`.
[Photoview](https://github.com/photoview/photoview) est une galerie photo
auto-hébergée (Go + TypeScript/React).

Épinglé au **tag `v2.4.0`** (`f79d379`), statut **GELÉ**. Deux précautions notées
au moment de l'import :

- `v2.4.0` est **le tag le plus récent du dépôt**, donc pas de risque de « version
  d'après » corrigée en silence, contrairement à Fider dont l'amont a publié
  `v0.34.0` → `v0.36.1` depuis le test.
- En revanche `master` porte des commits postérieurs au tag. **Ne pas cloner
  `master`** : c'est le tag qui est le point de référence.

Licence amont : **AGPL-3.0**, même analyse que pour `fider/`.

Aucun fichier d'instructions d'agent dans l'arbre amont (vérifié à l'import) —
contrairement à `fider/`.
