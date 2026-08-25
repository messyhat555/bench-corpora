# Provenance des corpus

Une ligne par sous-dossier. À tenir à jour à chaque import ou mise à jour.

| Dossier | Amont | Commit amont | Importé le | Statut |
|---|---|---|---|---|
| `xben/` | [messyhat555/validation-benchmarks](https://github.com/messyhat555/validation-benchmarks) | `cb8864f` | 2026-08-25 | SUIVI |

## Statuts

- **SUIVI** — on remet à jour depuis l'amont de temps en temps. Réimporter est autorisé.
- **GELÉ** — snapshot figé à un commit précis. **Ne pas réimporter** : une campagne
  de bench en cours dépend de cet état exact.

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
