# RRRUSH — Téléchargements

Utilitaire macOS de **transfert vérifié** et de **classification automatique** des rushs de projets audiovisuels.


Dézipper puis glisser `RRRUSH.app` dans le dossier **Applications**.

- macOS 15 (Sequoia) ou plus récent
- Application signée **Developer ID** et **notariée par Apple** — aucun avertissement Gatekeeper

## Fonctionnalités

- Détection automatique des cartes caméra (Sony XAVC/XDCAM, Canon XF, DCIM, GoPro/DJI, enregistreurs audio)
- Copie vers une ou plusieurs destinations avec vérification xxHash64 et rapport JSON — la source n'est jamais modifiée
- Classification automatique par gabarits à tokens : `{projet}/{date}/{camera}/{type}`
- Règles de couleurs, timeline de complétude multi-appareils, bibliothèque triable/filtrable, intégration Finder

---
RRRUSH — Manuel d'utilisation
Version 0.1.0 · macOS 15 ou plus récent

RRRUSH décharge vos cartes caméra vers vos disques de travail, vérifie chaque octet copié, et range automatiquement les fichiers selon vos règles. C'est le premier maillon de la chaîne de post-production — celui où une erreur est irrécupérable. RRRUSH est construit autour de deux principes :

Rien n'est jamais perdu. Chaque fichier copié est relu depuis le disque de destination et comparé à la source (empreinte xxHash64). La source n'est jamais modifiée. Rien n'est jamais écrasé.
« Copié » n'est pas « vérifié ». Une copie qui a l'air d'avoir réussi peut être corrompue. RRRUSH ne marque un fichier ✅ que lorsque la relecture prouve que la copie est identique à l'original.
1. Installation
Téléchargez la dernière version depuis github.com/julessourzac-gif/rrrush-releases.
Double-cliquez sur le zip pour le décompresser.
Glissez RRRUSH.app dans votre dossier Applications.
Lancez l'app. Elle est signée et notariée par Apple : aucun avertissement de sécurité.
Mises à jour : l'app vérifie automatiquement les nouvelles versions (Sparkle) et vous propose de les installer en un clic. Vous pouvez aussi retélécharger la dernière version depuis la même page à tout moment.

Optionnel : pour lire les métadonnées des formats exotiques (MXF, codecs RAW), installez ffprobe : brew install ffmpeg dans le Terminal. RRRUSH le détecte automatiquement — tout fonctionne sans lui pour les formats courants.

2. L'interface en un coup d'œil
┌────────────┬─────────────────────────────────┬──────────────┐
│  SIDEBAR   │        TABLE DES CLIPS          │  INSPECTEUR  │
│            │                                 │              │
│ SOURCES    │  Nom · Date · Durée · Codec…    │  Détail du   │
│  ▸ Carte ✓ │  (tri, filtres, recherche)      │  clip ou     │
│  ▸ SSD DJI │                                 │  panneau de  │
│ SORTIES    │                                 │  transfert   │
│  ▸ Projet  │                                 │              │
├────────────┴─────────────────────────────────┴──────────────┤
│ BANDEAU DE TRANSFERT  ▓▓▓▓░░ 62 % · 412 Mo/s · ⏸ · Annuler  │
└──────────────────────────────────────────────────────────────┘
Sidebar, section Sources : les cartes détectées et les dossiers ajoutés.
Sidebar, section Sorties : les fichiers déjà transférés — « Tous les clips », la « Timeline » et un dossier par projet.
Table centrale : les fichiers de la sélection courante.
Inspecteur (droite) : le panneau de transfert (si une source est sélectionnée) ou le détail complet d'un clip.
Bandeau (bas) : la progression du transfert en cours. Il reste visible pendant que vous naviguez ailleurs — aucune fenêtre bloquante.
3. Les sources
Détection automatique
Insérez une carte : elle apparaît dans la sidebar avec son type détecté d'après sa structure — Sony (XAVC/XDCAM), Canon XF, hybrides/DSLR (DCIM), GoPro/DJI, enregistreurs audio (WAV + iXML) — son nombre de médias et sa capacité.

Ajouter un dossier
N'importe quel dossier peut servir de source : bouton « Ajouter un dossier… » en bas de la section Sources. Utile pour un disque de rushs déjà déchargé ou un dossier reçu d'un tiers.

Le badge ✓ vert
Une carte marquée d'un sceau vert a tous ses fichiers médias transférés et vérifiés dans l'index. C'est le feu vert visuel : cette carte peut être formatée sans risque.

Colonne Statut
Dans la table d'une carte, chaque fichier affiche son état :

Statut	Signification
🟠 À transférer	Pas encore copié
🔵 Copie… / Vérification…	En cours dans le transfert actif
⏸ En pause	Transfert suspendu
🟢 Transféré	Copié et vérifié — ne sera jamais recopié par erreur
🔴 Échec	Copie non vérifiée — à refaire
4. Transférer
Le panneau de transfert
Sélectionnez une carte : l'inspecteur devient le panneau de transfert.

Projet : le nom du projet (il devient un dossier à la destination).
Destinations : une ou plusieurs — typiquement le SSD de travail et le disque de sauvegarde. Les deux copies sont écrites en parallèle du même flux de lecture, puis vérifiées indépendamment. Les dernières destinations utilisées sont pré-remplies.
Règle de classement : le gabarit de rangement (voir §5).
Aperçu de l'arborescence : l'arborescence exacte qui sera créée, avec le nombre de fichiers par dossier — vous voyez où tout ira avant de lancer.
Transférer et vérifier. Si le bouton est grisé, la raison est affichée juste en dessous.
Par défaut, tout le contenu de la carte est transféré. Pour ne transférer que certains fichiers, sélectionnez-les dans la table (⌘-clic pour une sélection multiple).

Les raccourcis
Bouton ⟶ sur la carte : quand une carte est sélectionnée dans la sidebar, un bouton de transfert apparaît sur sa rangée (pleine visibilité au survol). Un clic lance le transfert complet vers le projet courant, avec les dernières destinations et la dernière règle.
Clic droit sur une carte → « Tout transférer vers "projet" » : la même chose, depuis le menu contextuel.
Depuis la Timeline : sélectionnez des clips encore sur carte (orange) et cliquez « Transférer N fichier(s) de carte… ».
Pendant le transfert
Le bandeau du bas affiche le fichier en cours, la progression globale, le débit et les comptes vérifiés/échecs.

Pause / Reprendre : suspend le transfert après le bloc en cours (8 Mo). La reprise se fait exactement où la copie s'était arrêtée — rien n'est refait, rien n'est perdu. Les clips concernés affichent « En pause » dans leur colonne Statut.
Annuler : arrête la file. Les fichiers déjà vérifiés restent valides ; le fichier en cours est supprimé des destinations (pas de copie partielle orpheline).
Un fichier en échec n'interrompt pas la file : les erreurs s'accumulent dans un panneau consultable depuis le bandeau.
Après le transfert
Un rapport JSON (_RRRUSH_Report_<date>.json) est écrit à la racine de chaque destination : liste complète des fichiers, tailles, empreintes xxHash64, horodatages, statuts de vérification. C'est votre preuve de déchargement.
Un tag Finder est posé sur chaque copie : vert = vérifié, rouge = échec.
Les clips rejoignent la section Sorties de la sidebar.
Garanties techniques
Préallocation de l'espace avant écriture (échec immédiat si le disque est plein, fichiers non fragmentés) ; lectures et écritures hors cache système — la vérification relit le disque physique, pas une copie en mémoire ; dates de création/modification d'origine préservées sur les copies ; collision de nom → suffixe (clip_2.mp4), jamais d'écrasement.

5. Le classement automatique
Chaque fichier est rangé selon un gabarit à tokens, évalué d'après ses métadonnées :

{projet}/{date}/{camera}/{type}
→  PubACME/2026-07-03/Sony FX3/VIDEO/C0042.MP4
Tokens disponibles : {projet}, {date} (AAAA-MM-JJ), {annee}, {mois}, {jour}, {camera}, {type} (VIDEO/AUDIO/PHOTO), {codec}, {resolution}, {carte}, {nom-original}.

Une valeur manquante a toujours un repli lisible (CameraInconnue) — jamais de dossier vide. Quatre presets sont fournis (standard, par date, par type, multi-cam par carte).

Astuce — caméra mal identifiée : si une carte n'expose pas le bon modèle d'appareil (ou aucun), clic droit sur la carte → « Attribuer caméra/enregistreur… ». Le nom s'applique à tous ses fichiers : tables, Timeline et token {camera} des prochains transferts. L'attribution est mémorisée par carte et supprimable.

6. La bibliothèque (Sorties)
« Tous les clips » ou un projet : table de tout ce qui a été transféré.
Tri par n'importe quelle colonne ; filtres par type et caméra ; recherche par nom dans la barre d'outils.
Colonnes personnalisables (toutes les tables) : clic droit sur l'en-tête pour cocher/décocher les colonnes, glisser pour réordonner. Les colonnes techniques (timecode, scène/prise, xxHash, chemins, date de transfert…) sont masquées par défaut. La configuration est mémorisée.
Inspecteur : toutes les métadonnées du clip sélectionné, son empreinte et ses destinations.
Finder : double-clic ou clic droit → « Révéler dans le Finder » ; glisser-déposer des clips vers le Finder ou un logiciel de montage.
7. La Timeline — contrôle de complétude
La vue Timeline aligne tous les clips d'une journée sur un axe temporel commun, une piste par appareil (position par timecode, sinon par heure de prise). Elle sert à répondre à une question : ai-je bien tout déchargé ?

Bleu / vert : vidéo / audio transférés. Orange pointillé : fichiers encore sur les cartes connectées, pas transférés.
Rouge : trou suspect — un appareil est muet pendant qu'au moins un autre enregistrait. Chaque anomalie est listée dans le panneau du bas (« FX3 : rien entre 10:25:03 et 10:26:40 alors que 2 autres appareils enregistraient »).
Filtre par projet, sélecteur de journée, zoom temporel. Clic = sélection, ⌘-clic = sélection multiple, double-clic = révéler dans le Finder.
Cas typique : le WAV de l'enregistreur couvre toute la prise, une caméra n'a pas de clip sur une partie → carte oubliée ou fichier perdu, repéré avant d'être en salle de montage.

8. Les codes couleurs
Paramètres (roue crantée en bas de la sidebar) → Codes couleurs des rangées.

Chaque règle = un champ (caméra, codec, carte, scène, fps, durée, taille, statut… n'importe quelle métadonnée) + un opérateur + une valeur + un effet + une couleur.

Opérateurs : contient, ne contient pas, est égal à, est différent de, est supérieur à, est inférieur à, n'a aucune valeur (cible les champs vides — ex. caméra inconnue).
Effet Pastille : un point coloré devant le nom (façon tags Finder). Effet Rangée : toute la ligne surlignée.
La première règle « Pastille » qui correspond colore le point ; la première règle « Rangée » qui correspond surligne. Les règles s'appliquent à toutes les tables et sont activables/désactivables individuellement.
Exemples utiles : « Caméra contient FX3 → Pastille bleue », « Statut est égal à échec → Rangée rouge », « Scène n'a aucune valeur → Pastille grise ».

9. Formater une carte
Clic droit sur une carte → « Formater la carte… » (volumes amovibles uniquement).

Si tous les fichiers médias sont transférés et vérifiés : message vert, confirmation simple.
Sinon : avertissement rouge avec le nombre exact de fichiers non sécurisés, et une case « je comprends que ces fichiers seront définitivement perdus » à cocher avant de pouvoir formater.
La carte est effacée avec son nom et son format d'origine (exFAT, FAT32…) et réapparaît vide, prête à retourner en tournage. Le formatage est indisponible pendant un transfert. C'est la seule action de RRRUSH qui écrit sur une carte.

10. Nouveau projet et réinitialisation
« + Nouveau projet » (bas de la sidebar) : repart sur une interface vierge — sélection, dossiers ajoutés, nom de projet et index des Sorties sont réinitialisés. Les fichiers copiés restent évidemment intacts sur vos disques, avec leurs rapports. Vos réglages (couleurs, colonnes) sont conservés.
Paramètres → « Tout réinitialiser… » : efface les règles de couleurs, les configurations de colonnes et les champs mémorisés. Ne touche ni aux fichiers ni à l'index.
11. Questions fréquentes
Ma carte n'apparaît pas dans les Sources. Vérifiez qu'elle est montée dans le Finder. Les volumes internes du système sont ignorés volontairement. Un dossier peut toujours être ajouté manuellement en source.

Un fichier est en « Échec » après transfert. C'est grave ? Cela signifie que la relecture de la copie ne correspond pas à la source : la copie n'est pas fiable (disque défaillant, câble, secteur). Relancez le transfert de ce fichier (il n'est pas marqué « Transféré », il repartira). Si les échecs se répètent sur la même destination, méfiez-vous du disque.

Durée, codec ou caméra affichent « — ». Le format n'est pas lisible par macOS. Installez ffprobe (brew install ffmpeg) pour étendre la prise en charge (MXF, RAW). Pour la caméra, utilisez « Attribuer caméra/enregistreur… ».

Puis-je fermer l'app pendant un transfert ? Non — un transfert interrompu par la fermeture n'est pas repris au redémarrage (les fichiers déjà vérifiés restent acquis). Utilisez la pause si vous devez libérer le disque ou la machine.

Où est la preuve de mes transferts ? Le rapport _RRRUSH_Report_<date>.json à la racine de chaque destination, plus l'index des Sorties dans l'app (empreinte xxHash de chaque fichier dans l'inspecteur et la colonne « xxHash »).

L'index des Sorties a été vidé (nouveau projet), les badges ✓ ont disparu. Normal : le badge et la protection au formatage se fondent sur l'index. Les rapports JSON sur vos disques restent la preuve durable.
