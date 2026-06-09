# Projet Films des Nounous

Application web progressive (PWA) de suivi de films, avec synchronisation cloud via Supabase.

## Fonctionnalités

- Ajouter, modifier et supprimer des films
- Statuts : **À voir** / **Vu**
- Notes personnelles (Pénélope & Arty)
- Recherche automatique de posters via TMDB
- Statistiques et graphiques de visionnage
- Synchronisation cloud multi-appareils (Supabase)
- Installable comme application mobile (PWA)

---

## Ajouter votre propre dépôt

### 1. Forker le dépôt

1. Cliquez sur **Fork** en haut à droite de la page GitHub.
2. Clonez votre fork en local :
   ```bash
   git clone https://github.com/VOTRE-PSEUDO/topfilms.git
   cd topfilms
   ```

### 2. Configurer Supabase

1. Créez un compte sur [supabase.com](https://supabase.com) et créez un nouveau projet.
2. Dans l'éditeur SQL de Supabase, exécutez ce script pour créer la table :

   ```sql
   create table movies (
     id          text primary key,
     title       text not null,
     director    text,
     year        integer,
     runtime     integer,
     rank        integer,
     cat         text,
     plat        text,
     status      text,
     emoji       text,
     poster      text,
     backdrop    text,
     progress    integer,
     date_watched text,
     description text,
     note_pen    integer,
     note_art    integer,
     note_pen_text text,
     note_art_text text,
     updated_at  timestamp with time zone default now()
   );
   ```

3. Activez Row Level Security (RLS) si vous souhaitez restreindre l'accès :
   ```sql
   alter table movies enable row level security;
   -- Permettre lecture/écriture sans authentification (usage privé) :
   create policy "allow all" on movies for all using (true) with check (true);
   ```

4. Récupérez vos clés dans **Project Settings → API** :
   - `Project URL`
   - `anon / public key`

### 3. Configurer TMDB (optionnel — pour les posters)

1. Créez un compte sur [themoviedb.org](https://www.themoviedb.org) et générez une clé API.
2. Remplacez la clé dans `index.html` :
   ```js
   const TMDB_KEY = 'VOTRE_CLE_TMDB';
   ```

### 4. Mettre à jour les clés dans le code

Dans `index.html`, remplacez les valeurs suivantes par celles de votre projet Supabase :

```js
const SUPABASE_URL = 'https://VOTRE-PROJET.supabase.co';
const SUPABASE_KEY = 'VOTRE_CLE_PUBLIQUE';
```

### 5. Déployer sur GitHub Pages

1. Poussez vos modifications :
   ```bash
   git add index.html
   git commit -m "Configure Supabase credentials"
   git push origin main
   ```

2. Dans les paramètres du dépôt GitHub (**Settings → Pages**), sélectionnez la branche `main` et le dossier `/ (root)`.

3. Votre application sera disponible à l'adresse :
   `https://VOTRE-PSEUDO.github.io/topfilms/`

---

## Structure du projet

```
topfilms/
├── index.html        # Application complète (HTML + CSS + JS inline)
├── js/
│   └── app.js        # Service Worker & PWA install
├── css/              # Styles additionnels
├── icons/            # Icônes PWA
├── manifest.json     # Manifeste PWA
├── service-worker.js # Cache offline
└── sample/           # Données d'exemple
```

---

## Technologies

- **Supabase** — base de données PostgreSQL et sync temps réel
- **TMDB API** — recherche de posters de films
- **PWA** — installation sur mobile et mode hors-ligne
