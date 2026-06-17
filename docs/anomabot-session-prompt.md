# Concevoir le dispositif de découverte d'Anomabot

Avant de commencer, lire `README.md` et `docs/index.html` du dépôt `PerigGouanvic/lacuna` — ils décrivent l'architecture actuelle : **Lacunabot** (orchestrateur), **Brandolibot** (compresse et analyse le corpus conformiste), **Anomabot** (indexe publiquement le corpus anomalique). Anomabot existe pour l'instant comme nom et intention dans la doc — pas comme dispositif. C'est l'objet de cette session.

## Fonction d'Anomabot

Trouver, à grande échelle et en continu, des faits scientifiques solides et reconnus — pas seulement des sujets exotiques (ovnis, etc.), mais des anomalies fortes, documentées, qui ont simplement été mises sous le tapis — puis les indexer publiquement, lien par lien, vers leurs sources primaires (études, aveux, contradictions documentées). Pas de base de données secrète.

## Principe de sécurité déjà acté

La protection des faits les plus compromettants ne vient pas d'un accès restreint, mais du volume. Assez de faits "mainstream" forts pour que les plus infamants soient noyés dans la masse plutôt qu'isolés comme une liste ciblable. Si l'architecture proposée recrée un accès à deux niveaux (public/réservé), c'est un retour en arrière — à signaler explicitement.

## À concevoir dans cette session

1. **Mécanisme de découverte** — comment Anomabot trouve des candidats (requêtes PubMed/Semantic Scholar/Cochrane/arXiv déjà utilisées par Brandolibot pour l'audit de source ; détection de motifs du type "aucune étude sérieuse n'a montré X" suivie de vérification ; autre ?)
2. **Format de l'index** — que contient une entrée (lien, citation verbatim, source primaire, contexte de suppression observé) ?
3. **Rythme** — moisson continue en arrière-plan, ou strictement à la demande ? Le besoin de volume pour le camouflage suggère une moisson continue, pas seulement réactive.
4. **Hors scope explicite pour cette session** : la spécialisation multi-agents par domaine (physique, médecine, etc.) — volontairement mise de côté ("on n'est pas rendus là").

## Contrainte de ton

Anomabot n'est pas un projet "ovnis et compagnie". Son centre de gravité : des anomalies reconnaissables comme sérieuses même par un public sceptique, avec les cas plus marginaux noyés dans cette masse plutôt que mis en avant.
