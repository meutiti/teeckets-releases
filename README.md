# Teeckets — téléchargements et mises à jour

Ce dépôt public héberge le catalogue de mises à jour de Teeckets. Les fichiers de l’application sont distribués dans les [Releases](https://github.com/meutiti/teeckets-releases/releases).

**Le canal est initialisé. Aucune version de l’application n’est encore publiée ici.** Le catalogue signé est donc vide : il n’annonce aucune mise à jour et n’impose aucun blocage.

## Pour les utilisateurs

Dans Teeckets, choisir **Teeckets → Rechercher des mises à jour…**. L’application vérifie la signature avant d’installer une version.

Les projets, réglages et historiques sont conservés. Si une session d’agent est encore ouverte, terminer et fermer cette session pour permettre l’installation.

## Pour publier une version

La préparation et la signature se font depuis le projet privé Teeckets. Ce dépôt ne contient ni le code de l’application, ni les clés privées, ni les identifiants Apple.

1. Depuis Teeckets, fabriquer le ZIP signé et validé par Apple avec `bash script/build_and_run.sh --release`.
2. Copier le nouveau ZIP et son JSON dans `artifacts/`, avec les archives précédentes. Ce dossier reste local et est ignoré par Git. Copier également le `appcast.xml` actuel dans `artifacts/` pour conserver les anciennes URL.
3. Ajouter les notes de version, portant le même nom de base que le ZIP.
4. Depuis le projet Teeckets, préparer le nouveau catalogue. Remplacer `TAG` par le nom de la nouvelle release, par exemple `v0.1.1` :

```sh
python3 -B script/prepare_updates.py ../teeckets-releases/artifacts \
  --download-url-prefix https://github.com/meutiti/teeckets-releases/releases/download/TAG/
```

5. Créer une release en brouillon dans ce dépôt, y joindre le nouveau ZIP, son JSON et les notes référencées par le catalogue. Publier la release complète et vérifier que les fichiers se téléchargent sans connexion GitHub.
6. Copier `artifacts/appcast.xml` à la racine de ce dépôt, puis commiter et pousser **ce fichier signé**. Cette dernière étape annonce la version aux clients.

Publier toujours les fichiers **avant** le catalogue qui les annonce. Ne jamais modifier le XML après sa signature. Ne jamais remplacer le contenu d’une archive déjà publiée : créer une nouvelle version.

Conserver les anciennes releases et les anciennes entrées du catalogue, notamment pour les Mac qui ne peuvent pas installer la version la plus récente. Le dossier local `artifacts/` peut être reconstitué depuis les releases et leurs manifestes en cas de changement de Mac.

## Adresse utilisée par l’application

```text
https://raw.githubusercontent.com/meutiti/teeckets-releases/main/appcast.xml
```

Cette adresse reste la même à chaque version. Les ZIP utilisent les adresses propres à chaque release. Aucun hébergement ou abonnement supplémentaire n’est nécessaire.

Le guide complet, y compris les mises à jour obligatoires, est dans `docs/updates.md` du projet privé Teeckets. Les nouvelles versions restent facultatives tant qu’aucune obligation n’y est configurée.
