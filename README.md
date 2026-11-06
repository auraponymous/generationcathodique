# 📺 Génération Cathodique

> Zappez dans les séries et dessins animés de la télévision française des années 80.

**Génération Cathodique** est une petite encyclopédie interactive consacrée aux programmes télé qui ont marqué les années 80 en France : dessins animés, sitcoms, séries d'action, séries jeunesse et tokusatsu.

On navigue comme devant un vrai poste : on change de chaîne, on zappe au hasard, on consulte le télétexte… puis on ouvre la fiche de la série, présentée comme une page de magazine télé.

---

## ✨ Fonctionnalités

| | |
|---|---|
| 📺 **Le téléviseur** | Un poste des années 80-90 dont l'écran affiche le titre, l'année, le pays et la famille de la série, avec un effet cathodique (neige, lignes de balayage, décrochage de l'image au changement de chaîne). |
| 🔘 **Les boutons** | `▲ CH` / `▼ CH` pour changer de chaîne, `ZAP` pour une chaîne au hasard, `TXT` pour le télétexte, `OK · FICHE` pour ouvrir la fiche. |
| 📟 **Le télétexte** | La liste des séries affichées, façon page 100 : un clic sur un titre règle le poste sur la bonne chaîne. |
| 📼 **Le magnétoscope** | Une recherche plein texte dans toutes les colonnes (titres, auteurs, compositeurs, interprètes…), des filtres **Famille**, **Chaîne** et **Pays**, un tri (ordre de la base, par année, de A à Z) et un compteur de résultats. |
| 📰 **La fiche** | La page magazine d'une série : première diffusion, chaîne, pays, nombre d'épisodes et crédits (création, réalisation, scénario, dessin, musique, générique français, acteurs). Un clic sur un nom affiche toutes les séries où il apparaît. |
| 🎨 **L'ambiance** | La couleur de l'écran et la lueur sur le mur changent selon la chaîne de première diffusion. |
| 🔗 **Les liens directs** | Chaque série a sa propre adresse (`…/#goldorak`), qu'on peut partager. |
| 📱 **Le mobile** | La page s'adapte aux petits écrans, et on zappe en balayant l'écran du doigt. |

### ⌨️ Raccourcis clavier

| Touche | Action |
|---|---|
| `←` `→` | Chaîne précédente / suivante |
| `Entrée` | Ouvrir la fiche |
| `T` | Afficher ou masquer le télétexte |
| `Z` | Zapper au hasard |
| `/` | Aller dans la recherche |
| `Échap` | Fermer la fiche |

---

## 🧱 Architecture

```
generation-cathodique/
├── index.html   ← tout le site (HTML + CSS + JS, sans dépendance)
└── README.md
```

- **Un seul fichier HTML**, sans framework ni étape de compilation.
- **Les données viennent d'un Google Sheet**, lu en direct par l'API *Google Visualization* (gviz), en JSONP. Ce mode de chargement fonctionne aussi bien en ligne qu'en ouvrant le fichier en local.
- **L'hébergement est statique**, sur GitHub Pages.
- **Les polices** viennent de Google Fonts : *Bungee*, *VT323*, *Oswald* et *Source Serif 4*.

Mettre la base à jour se fait **directement dans le Google Sheet** : le site affiche les modifications au rechargement suivant.

---

## 🗂️ Structure du Google Sheet

Le premier onglet doit contenir une ligne d'en-têtes, puis une ligne par série. Le site reconnaît les colonnes à leur nom, sans tenir compte des accents ni des majuscules. L'ordre des colonnes n'a donc pas d'importance.

| Colonne | Exemple |
|---|---|
| Titre | Goldorak |
| Famille | Animation |
| Pays d'origine | Japon |
| Première diffusion France (année) | 1978 |
| Chaîne de première diffusion | Antenne 2 |
| Nombre d'épisodes | 74 |
| Créateur / auteur | Gō Nagai (manga et création des personnages) |
| Réalisateur(s) principal(aux) | Tomoharu Katsumata |
| Scénariste(s) | Shōzō Uehara ; Keisuke Fujikawa |
| Dessinateur / character designer | Kazuo Komatsubara ; Shingo Araki |
| Compositeur(s) musique | Shunsuke Kikuchi |
| Interprète(s) du générique français | Enriqué ; Noam Kaniel |
| Acteurs principaux (séries live) | — |

**Conventions de saisie :**
- Dans une même cellule, plusieurs noms se séparent par ` ; ` (point-virgule entouré d'espaces). Chaque nom devient une étiquette cliquable sur la fiche.
- Une précision entre parenthèses à la fin d'un nom s'affiche en petit, à côté du nom : `Dorothée (générique Club Dorothée)`.
- Une cellule vide fait simplement disparaître la rubrique sur la fiche.
- Pour la chaîne, le filtre ne retient que la première chaîne indiquée : `Antenne 2 / rediffusions` est rangée sous **Antenne 2**.
- Pour le pays, le filtre découpe sur ` / ` : `Japon / Italie` apparaît à la fois sous **Japon** et sous **Italie**.

> 💡 **Conseil :** gardez les colonnes au format texte brut. Une colonne qui mélange des nombres et du texte (par exemple `1980s` au milieu des années) peut perdre des valeurs à la lecture par l'API gviz.

---

## ⚙️ Configuration

En haut du `<script>` dans `index.html` :

```js
const SHEET_ID   = "1Lt7PLl4H_Pz-rzUzIUZe0_Xvo7rJwvpU0bXOe4qXOLI";
const SHEET_NAME = "";   // vide = premier onglet ; sinon nom exact de l'onglet
```

Les couleurs d'ambiance de chaque chaîne se règlent dans `CHANNEL_COLORS` :

```js
const CHANNEL_COLORS = {
  "tf1":"#2f6bff", "antenne 2":"#e63946", "fr3":"#13a89e", "la cinq":"#f08a24",
  "canal+":"#8f5bd6", "m6":"#e0c21b", "ortf":"#c9c2b0", "rtbf":"#4cb3e6"
};
```

---

## 🚀 Mise en ligne sur GitHub Pages

1. **Partager le Google Sheet** en lecture : *Partager → Accès général → Tous les utilisateurs disposant du lien → Lecteur*. Sans ce réglage, l'écran affiche « Pas de signal ».
2. **Déposer `index.html`** à la racine du dépôt.
3. Dans le dépôt, ouvrir **Settings → Pages**, puis choisir *Deploy from a branch*, la branche `main` et le dossier `/ (root)`.
4. Le site est en ligne à l'adresse `https://<utilisateur>.github.io/<dépôt>/`.

Pour le tester en local, il suffit d'ouvrir `index.html` dans un navigateur ; aucun serveur n'est nécessaire.

---

## 📚 Sources des données

La base a été constituée à partir de :
- [Planète Jeunesse](https://www.planete-jeunesse.com/) — fiches détaillées pour l'animation, les séries jeunesse et les tokusatsu ;
- [Wikipédia](https://fr.wikipedia.org/) (fr et en) — séries live ;
- [la chronologie de la télévision française des années 1980](https://fr.wikipedia.org/wiki/Chronologie_de_la_t%C3%A9l%C3%A9vision_fran%C3%A7aise_des_ann%C3%A9es_1980) et le [catalogue de l'INA](https://catalogue.ina.fr/) pour les dates.

Il s'agit d'une **base de travail éditoriale**, pas d'un inventaire d'archives exhaustif. Les crédits peuvent varier selon les épisodes et les versions, et certaines dates restent à confirmer. Les corrections sont les bienvenues.

---

## ⚖️ Mentions

Les titres de séries, les noms de chaînes et les noms de personnes ne sont cités qu'à titre documentaire. Le site n'utilise ni image, ni logo, ni extrait des œuvres : tout l'habillage visuel est d'origine.
