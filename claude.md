# CLAUDE.md — Contexte du projet Unity

> Lis ce fichier avant toute modification. Il décrit le projet, la charte et les
> règles à respecter. **Ne modifie que ce qui est demandé ; le reste reste intact.**

## Le projet

**Unity** est un site vitrine pour une plateforme multiculturelle qui :
- accueille les **internationaux** arrivant en Belgique et en France ;
- **réunit et fédère les diasporas** (la diaspora congolaise est la première mise en avant).

Le site est **statique** (HTML/CSS/JS pur, aucune étape de build) et **bilingue FR/EN**.

## Structure des fichiers (tous à la racine)

| Fichier | Rôle |
|---|---|
| `index.html` | Page d'accueil (hero, plateforme, diasporas, events, vision, CTA) |
| `evenements.html` | Page Événements (À venir + Passés) |
| `vision.html` | Page Vision |
| `comment-ca-marche.html` | Page « Comment ça marche ? » |
| `diaspora-congolaise.html` | Page diaspora congolaise |
| `devenir-membre.html` | Page d'inscription |
| `netlify.toml` | Config de déploiement Netlify (ne pas casser) |

Chaque page est **autonome** : son CSS et son JS sont inclus dans le fichier (pas de fichiers externes), et les **images sont intégrées en base64** dans le HTML.

## Charte graphique

**Couleurs (variables CSS `:root`)**
- Fond : `--cream:#FFFFFF` (blanc), `--cream-2:#F4F4F2` (gris très clair pour sections alternées), `--paper:#FFFFFF` (cartes)
- Texte : `--ink:#1C1426`, `--ink-soft:#4A3D55`
- Accents : `--coral:#FF4D2E` (boutons), `--marigold:#FFB400`, `--teal:#0E9E89`, `--indigo:#3B2C8E`, `--magenta:#E8407A`, `--sky:#1E86C9`

**Règles de design à conserver**
- Fond **blanc simple** — **pas de grosses boules/blobs** en arrière-plan (`.blob{display:none}`).
- **Boutons orange** (coral) avec ombre portée façon « bouton 3D ».
- Coins arrondis généreux (`--radius:22px`).

**Polices** (chargées via Google Fonts)
- Titres : **Bricolage Grotesque** (poids 700/800)
- Corps : **Hanken Grotesk**
- Pas d'italique dans les gros titres : on les laisse simplement en gras.

## Système bilingue (FR/EN) — IMPORTANT

- Chaque texte traduisible porte un attribut `data-i18n="cle"` (ou `data-i18n-html="cle"` si le texte contient du HTML comme `<br>` ou `<em>`).
- En bas de page, un objet JS contient les traductions :
  ```js
  const I18N = { fr:{ cle:"texte FR", ... }, en:{ cle:"texte EN", ... } };
  ```
  (sur les sous-pages l'objet s'appelle `D` au lieu de `I18N`).
- **Pour ajouter/modifier un texte traduisible** : mets `data-i18n="ma_cle"` sur l'élément, puis ajoute `ma_cle` **dans `fr` ET dans `en`**. Une clé présente dans le HTML mais absente du dictionnaire laisse l'élément vide → toujours fournir les deux langues.
- **Persistance de la langue** : la langue choisie est propagée entre les pages via le paramètre d'URL `?lang=` (fonction `langLinks` qui réécrit les liens internes, + `history.replaceState`). Ne pas casser ce mécanisme. Tout nouveau lien interne `*.html` est automatiquement géré.

## Liens externes (Heartbeat) — à réutiliser tels quels

- **Rejoindre / S'inscrire / Je rejoins** → `https://app.heartbeat.chat/welcome-unity/invitation?code=J32DJ4`
- **Se connecter** → `https://app.heartbeat.chat/login/welcome-unity?redirectTo=%2Fwelcome-unity`
- **Event Namur (Welcome Day)** → `https://app.heartbeat.chat/welcome-unity/events/859447`

Tous ces liens externes ouvrent dans un nouvel onglet (`target="_blank" rel="noopener"`).

## Repères de structure (index.html)

- **Nav** (sur TOUTES les pages, identique) : Accueil · Événements · Comment ça marche ? · Vision + sélecteur FR/EN + boutons « Se connecter » et « Rejoindre ».
- **Hero** : 2 colonnes. Gauche = grand titre (`h1.hero-title`) + sous-titre (`.hero-sub`) + 2 boutons (`.hero-cta`) + bulles avatars et « 2 pays ouverts : Belgique - France » (`.hero-social`). Droite = photo de groupe (`.hero-photo`, image base64).
- **Marquee** : bandeau défilant « Unité » dans plusieurs langues.
- **Plateforme** (`#features`) : 6 cartes en **menu défilant horizontal** (`.feat-scroll` > `.fcard` avec en-tête `.fc-img` + emoji et corps `.fc-body`).
- **Diasporas** (`#diasporas`), **Événements** (`#events`, 2 cartes cliquables), **Vision** (`#vision`), **CTA** final, **footer**.

## Règles de déploiement (ne pas casser)

- Le site se déploie sur **Netlify via GitHub** : chaque `git push` sur la branche `main` redéploie automatiquement.
- `index.html` doit rester **à la racine**.
- `netlify.toml` impose : **aucune commande de build**, `publish = "."`. Ne pas ajouter de `package.json`, `bun.lock`, ni d'étape de build.
- Garder le site **100 % statique** (pas de framework, pas de dépendances).

## Conventions de travail

- Modifier le **strict nécessaire** ; ne pas réécrire des sections non demandées.
- Préserver les **images base64** existantes (ne pas les supprimer ni les régénérer sans raison).
- Après une modif de texte traduisible, vérifier que la clé existe bien en **FR et EN**.
- Garder la **nav identique** sur toutes les pages.
- Workflow type : modifier → relire → `git add -A` → `git commit -m "..."` → `git push`.
