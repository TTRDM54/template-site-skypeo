# SKYPEO — Template Mockups de Prospection

## Contexte

Tu génères des **maquettes de prospection** pour SKYPEO, agence web spécialisée dans les artisans du bâtiment et TPE locales (région Grand Est, France). Ces maquettes sont envoyées en amont d'un appel commercial via WhatsApp Business pour déclencher un "wow effect" et closer le prospect.

**Ce ne sont PAS des sites clients en production.** Ce sont des démos one-page, déployées gratuitement sur Vercel, conçues pour impressionner en 10 secondes. Si le prospect signe, le site final sera reconstruit proprement en Astro par le dev SKYPEO.

## Cible & Ton

- **Prospects** : artisans du bâtiment (plombiers, électriciens, couvreurs, maçons, peintres, paysagistes, etc.) et TPE locales
- **Différenciation** : on remplace les Solocal/PagesJaunes du prospect par un site moderne, premium, animé
- **Ton** : confiance, expertise locale, proximité, pas de jargon corporate
- **Objectif émotionnel du mockup** : "putain, je n'imaginais pas que mon site puisse ressembler à ça"

## Stack Technique

Single-file, déployable sur Vercel en quelques secondes.

- **HTML5 unique** : tout dans un `index.html` à la racine
- **Tailwind CSS via CDN** : `<script src="https://cdn.tailwindcss.com"></script>`
- **Police** : Inter via Google Fonts, fallback sans-serif système
- **Aucune dépendance externe** sauf Tailwind, Google Fonts et les assets vidéo
- **Mobile-first** : optimisation mobile obligatoire (la majorité des prospects ouvrent le lien sur leur téléphone)
- **Pas de framework JS** : vanilla JS si strictement nécessaire, rien d'autre

## Structure d'une Maquette

Toute maquette suit cette structure, dans cet ordre :

1. **Hero header** avec animation 3D en background (vidéo `.mp4` fournie par moi) + nom du prospect en H1, ville, accroche métier, CTA téléphone
2. **Section services** (3 ou 4 cartes max, icônes SVG simples ou emojis sobres)
3. **Section "Pourquoi nous"** ou bénéfices, avec animation 3D scroll-driven optionnelle
4. **Section réalisations ou témoignages** (placeholders réalistes, jamais de faux noms grossiers)
5. **Section contact** : téléphone cliquable (`tel:`), formulaire visuel non fonctionnel, adresse, horaires
6. **Footer minimaliste** : mentions légales basiques, zone d'intervention, copyright année courante

Le prospect doit voir SON métier, SON nom, SA ville dès le hero. Personnalisation maximale.

## Workflow

### Étape 1 — Brief prospect (input utilisateur)

Je te fournis :
- Nom de l'entreprise + nom du gérant
- Métier (plombier, couvreur, etc.)
- Ville et zone d'intervention
- Services principaux (3 à 5)
- Téléphone, éventuellement adresse
- Le fichier `hero-animation.mp4` placé à la racine du projet
- Optionnel : screenshot de leur Solocal actuel ou d'un site d'inspiration

### Étape 2 — Génération du HTML

Génère un `index.html` complet avec :
- Hero qui affiche `hero-animation.mp4` en `<video autoplay muted loop playsinline>` en background
- Overlay sombre dégradé (gradient `from-black/60 to-black/20`) pour lisibilité du texte
- Texte hero en blanc, bold, taille `text-4xl md:text-6xl` pour le H1
- CTA principal : bouton appel direct (couleur selon métier, voir palette ci-dessous)
- Le reste des sections suit la structure définie au-dessus

### Étape 3 — Boucle qualité (uniquement si site d'inspiration fourni)

Si je te donne un screenshot d'inspiration, applique la boucle screenshot-compare-fix :
1. Génère le HTML
2. Screenshot le rendu (Puppeteer ou équivalent)
3. Compare au screenshot d'inspiration : espacement, polices, couleurs, alignements
4. Corrige les écarts
5. Re-screenshot et compare
6. Répète jusqu'à 2-3 px de tolérance

Si pas de site d'inspiration, ignore cette étape et applique directement les standards SKYPEO.

### Étape 4 — Déploiement (monorepo GitHub + Vercel automatisé)

**Setup en place — 100% automatisé. Aucune action sur Vercel après l'étape de création de la maquette :**
- Le dossier `C:\Users\timot\template-site-skypeo` est un repo git, branche `main`, remote `origin` = `https://github.com/TTRDM54/template-site-skypeo.git`
- **Un seul projet Vercel** est connecté à ce repo, Root Directory = racine du repo
- À la racine : `vercel.json` (`cleanUrls: true`, `trailingSlash: true`) + un `index.html` privé `noindex` qui sert de placeholder
- `.gitignore` racine exclut `node_modules`, `.vercel`, `*.log`, `.env*`, `.DS_Store`
- Chaque prospect = un sous-dossier (ex. `Sunest/`, `Plomberie-Dupont/`) avec son `index.html` + sa `hero-animation.mp4`
- L'utilisateur travaille sous **Git Bash sur Windows**

**Workflow pour chaque nouvelle maquette — 3 commandes, fin :**

1. Créer le sous-dossier `[NomProspect]/` avec `index.html` + la vidéo hero (placée par le user)
2. Donner au user les commandes à coller dans Git Bash :
   ```bash
   cd /c/Users/timot/template-site-skypeo
   git add [NomProspect]/
   git commit -m "feat: maquette [NomProspect] - [metier] [ville]"
   git push
   ```
3. **Vercel auto-déploie** sous 30 secondes. URL finale : `template-site-skypeo.vercel.app/[NomProspect]/`
4. Donner cette URL au user — c'est ce qu'il enverra au prospect via WhatsApp.

**Aucune intervention sur Vercel n'est jamais requise.** Pas de nouveau projet, pas de réglage de Root Directory, pas de naming.

**Ne jamais init un git dans un sous-dossier prospect** — le `.git` vit uniquement à la racine du monorepo. Si `.git/index.lock` traîne : `rm -f .git/index.lock` avant le commit.

**Convention de nommage** : sous-dossier = nom commercial PascalCase ou tel-quel (`Sunest`, `LT-Services`, `Plomberie-Dupont`). L'URL Vercel reprend ce nom à l'identique.

**Conséquences pour le HTML d'une maquette** :
- Les chemins relatifs (`<source src="hero-animation.mp4">`, `href="#contact"`) fonctionnent parce que `trailingSlash: true` les résout sous `/[NomProspect]/...`
- Toujours utiliser des chemins **relatifs**, jamais absolus avec `/` initial pour les assets locaux
- Les URLs externes (Tailwind CDN, Google Fonts, photos sur sunest.fr...) sont toujours en absolu `https://`, aucun problème
- L'`og:url` doit pointer vers l'URL Vercel finale (`https://template-site-skypeo.vercel.app/[NomProspect]/`)

## Standards Visuels SKYPEO

### Palette par métier

Un seul accent de couleur par site, jamais deux.

- **Plombier / chauffagiste** : bleu acier `#1E40AF`
- **Électricien** : jaune ambre `#F59E0B`
- **Couvreur / maçon** : terre cuite `#B45309`
- **Paysagiste** : vert profond `#15803D`
- **Peintre / décorateur** : bleu nuit `#1E3A8A` ou bordeaux `#991B1B`
- **Multi-services / général** : gris anthracite `#1F2937` + accent doré `#CA8A04`
- **Solaire / photovoltaïque** : bleu ciel `#0EA5E9` (accent principal) + jaune solaire `#FACC15` (highlight, étoiles, soleil)

**Exception double-couleur** : le métier solaire/photovoltaïque autorise un duo bleu ciel + jaune (le jaune reste un accent secondaire très ponctuel : un mot du H1, le soleil du logo, les étoiles, jamais un bouton). Pour tous les autres métiers, la règle « une seule couleur d'accent » s'applique strictement.

**Si le métier du prospect n'est pas dans cette liste**, demande à l'utilisateur quelle palette appliquer en proposant une recommandation basée sur le site source (analyse les couleurs dominantes via web_fetch).

### Couleurs neutres

- **Fond principal** : blanc cassé `#FAFAFA` ou noir profond `#0A0A0A` selon ambiance
- **Texte principal** : `#111827` sur fond clair, `#F9FAFB` sur fond sombre
- **Texte secondaire** : `#6B7280` (clair) ou `#9CA3AF` (sombre)

### Typographie

- **Inter** uniquement, importée depuis Google Fonts
- Hero H1 : `font-bold text-4xl md:text-6xl tracking-tight`
- Section H2 : `font-semibold text-3xl md:text-4xl`
- Body : `text-base md:text-lg leading-relaxed`
- Pas de police décorative, jamais de comic ou serif old-school

### Espacement

- Sections : `py-20 md:py-32`
- Container : `max-w-6xl mx-auto px-6`
- Gap entre cartes : `gap-8`

### Boutons

- Primaire : fond plein couleur accent, texte blanc, `rounded-xl px-6 py-3 font-semibold hover:opacity-90 transition`
- Secondaire : bordure couleur accent, texte couleur accent, transparent au repos
- CTA téléphone toujours en `tel:+33XXXXXXXXX`

### Images

- **Aucune photo générique de banque d'images visible** type Pexels watermark
- Si pas d'image fournie, utilise des placeholders Unsplash via URL directe avec mots-clés métier précis (ex. `https://images.unsplash.com/photo-XXXX?q=80&w=1200`)
- Coins arrondis `rounded-2xl` minimum
- Ombres légères : `shadow-lg shadow-black/5`

## Intégration Vidéo Hero

Le fichier `hero-animation.mp4` est placé dans le sous-dossier du prospect par mes soins (généré séparément sur Higgsfield/Kling).

**Vérifier le nom de fichier au début** — le user uploade parfois `hero-animation.mp4.mp4` (double extension Windows). Si tu détectes ça, renomme-le silencieusement avant d'écrire le HTML : `mv hero-animation.mp4.mp4 hero-animation.mp4`.

Spécifications HTML à respecter :

```html
<video 
  autoplay 
  muted 
  loop 
  playsinline 
  preload="auto"
  class="absolute inset-0 w-full h-full object-cover">
  <source src="hero-animation.mp4" type="video/mp4">
</video>
```

- Toujours appliquer un overlay dégradé par-dessus pour la lisibilité du texte
- Le hero doit fonctionner même si la vidéo ne charge pas (fallback sur fond uni couleur accent)

## Animations Scroll-Driven (optionnel)

Si je le demande explicitement, ajoute une section avec animation scroll-driven utilisant une seconde vidéo (`scroll-animation.mp4` que je placerai à la racine).

Technique pour la fluidité :
1. Extraire les frames de la vidéo en JPEG optimisés (via ffmpeg ou équivalent)
2. Lier la position de défilement à l'index de frame affichée
3. Préchargement des frames au load de la section
4. Texte en overlay qui apparaît progressivement section par section

C'est lourd : à utiliser uniquement sur mockups premium pour gros prospects.

## Performance

- **Poids total cible** : < 5 Mo pour la page complète
- **Vidéo hero** : objectif < 1 Mo, jamais plus de 3 Mo
- **LCP** : < 2,5 secondes sur 4G
- Si la vidéo dépasse 3 Mo, propose-moi une compression ffmpeg : `ffmpeg -i input.mp4 -vcodec libx264 -crf 28 -preset slow -movflags +faststart output.mp4`
- Lazy loading sur tout ce qui n'est pas above-the-fold
- `<video>` avec `preload="auto"` pour le hero, `preload="none"` pour les vidéos en bas de page

## SEO & Aperçu WhatsApp

L'aperçu WhatsApp est critique : c'est ce que le prospect voit avant même d'ouvrir le lien.

À inclure systématiquement dans le `<head>` :
- `<title>` : `[Métier] [Ville] | [Nom entreprise]`
- `<meta name="description">` : 150 caractères max, accrocheuse
- Balises Open Graph complètes : `og:title`, `og:description`, `og:image`, `og:url`
- Twitter Card : `twitter:card` en `summary_large_image`
- Favicon simple (initiale du prospect sur fond couleur accent, en SVG inline)
- `lang="fr"` sur la balise `<html>`

## Mentions légales (placeholders propres)

En footer, toujours :
- Lien "Mentions légales" (vers `#` ou modal vide)
- "Maquette réalisée par SKYPEO — agence web Nancy" en petit
- Année courante en copyright
- Zone d'intervention listée sobrement

Ne jamais inventer un SIRET. Mettre `[SIRET à renseigner]` ou ne pas l'afficher du tout sur le mockup.

## Règles Absolues

- **Ne jamais ajouter de fonctionnalités non demandées** (pas de chatbot, pas de cookies banner, pas de Google Analytics sur un mockup)
- **Ne jamais inventer de témoignages avec noms réels** (utiliser "Client satisfait — Nancy" plutôt que "Jean Dupont")
- **Ne jamais utiliser des images de personnes générées sans préciser que ce sont des placeholders**
- **Toujours mobile-first** : tester la version mobile avant la desktop
- **Une seule animation 3D hero par mockup** pour ne pas alourdir
- **Si le prospect a un Solocal existant** : le mockup doit clairement faire mieux visuellement, c'est l'argument de vente

## Patterns réutilisables

### Mock widget Google Reviews

Quand le prospect a des avis Google publics (recommandé : présent sur sa fiche Google Business), intègre un faux widget qui imite l'embed Google. Aucune dépendance externe, conforme aux règles SKYPEO.

Composants visuels obligatoires :
- Logo Google « G » en SVG inline 4 couleurs (`#4285F4`, `#34A853`, `#FBBC05`, `#EA4335`)
- Score sur 5 en chiffre énorme (ex. `4,8 / 5`), centré
- Rangée de 5 étoiles couleur jaune solaire `#FACC15`
- Badge « ✓ Vérifiés » vert (`bg-emerald-50 text-emerald-700`)
- Mention « Basé sur + de N avis » avec le vrai nombre récupéré sur la fiche Google
- CTA « Voir tous les avis sur Google » → lien `target="_blank" rel="noopener"` vers la **vraie URL Google search** du prospect (à demander au user, ou récupérée via web_fetch)

Sous le widget : grille de 3 cartes de témoignages verbatim (avis Google publics, on garde les vrais noms — c'est de la donnée publique). Chaque carte a un avatar circulaire avec initiales sur fond accent + le mini-G + mention « Avis Google vérifié ».

### Hero full-background vidéo (standard)

Vidéo en `absolute inset-0 w-full h-full object-cover` qui couvre toute la section hero (`h-screen min-h-[640px]`). Overlay double : un dégradé sombre vertical `from-black/55 via-black/25 to-black/70` pour la lisibilité, plus une teinte couleur accent sur les côtés pour ancrer la palette. Texte blanc, `drop-shadow-2xl` sur le H1.

**Ne pas tenter de variantes** (vidéo masquée centrée, mask gradient inward...) à moins que l'utilisateur le demande explicitement.

### Header transparent au top → opaque au scroll

Le header est `bg-transparent` sur le hero, puis bascule en blanc opaque dès qu'on scroll de 40px. Classe `.nav-scrolled` ajoutée par un petit script vanilla JS au bas du body. Liens nav passent du blanc au sombre via la même classe.

### Ordre des sections recommandé pour bâtiment / TPE locales

1. Hero
2. Équipe / portraits (carousel marquee infini) — ancre la confiance
3. Avis Google (mock widget + 3 cartes)
4. Services (3-4 cartes)
5. Pourquoi nous (4 piliers + photo équipe)
6. Contact (fond noir, formulaire visuel + tel cliquable)
7. Footer

Alternance des fonds : vidéo → gris `#FAFAFA` → blanc → gris → blanc → noir `#0A0A0A`.

## Si tu fais une erreur récurrente

Mets à jour ce CLAUDE.md avec la règle correspondante pour qu'une future session ne refasse pas la même erreur.
