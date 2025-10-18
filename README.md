# Salsa2D - MOBA De Danse Top-Down

Ce document présente la vision et les recommandations techniques pour un jeu de danse compétitif en vue de dessus inspiré des mécaniques de MOBA.

## Vision Globale

La proposition de valeur repose sur un gameplay systémique mêlant chorégraphies salsa, rôles complémentaires et stratégie coopérative.
- Carte top-down lisible, combinant déplacements libres et zones d'interaction.
- Compétences inspirées des figures de salsa (basic step, enchufla, shines) avec fenêtres de timing calées sur le beat.
- Rôles leader/follower ou styles spécialisés influençant buffs, portée et gestion d'endurance.
- Modes variés : 1v1, 2v2 en couples, 5v5 en cercle avec capture de zone par synchro, salons coop pour freestyle.

## Paysage Concurrentiel

Quelques références actuelles montrent l'absence d'un titre mêlant top-down et mécaniques de danse compétitive.
- Jeux de danse grand public : Just Dance Now (mobile) et la poussée d'Ubisoft sur les contrôleurs caméra mains libres.
- Battle de danse sur mobile : DanceFight (vidéo battles + classements) et Dance Clash (chorégraphies casual) sans dimension compétences.
- Top-down compétitifs : tendance persistante du look & feel façon LoL (ex. Supervive) prouvant la lisibilité et la nervosité du format.

## Pile Technologique Recommandée

Le stack privilégié vise un développement mobile-first avec une progression claire vers l'échelle compétitive.

### Moteur Et Rendu

Un moteur performant doit équilibrer accessibilité mobile et évolutivité vers des effets plus riches.
1. Unity 6 + URP comme recommandation principale, avec GameObjects classiques ou DOTS/Entities 1.3+ si foules et VFX massifs.
2. Godot 4.3/4.4 en alternative open-source légère avec pipeline GDExtension et intégration fluide avec Nakama.
3. Unreal 5.6 pour une ambition visuelle cinématique et cross-play PC/console, au prix d'une empreinte supérieure.

### Multijoueur Temps Réel

La pile réseau s'articule autour d'une montée en puissance progressive selon les besoins en latence.
1. Prototype/MVP : Unity Netcode for GameObjects + Unity Relay + Lobby pour du host-client simple avec NAT traversal.
2. Phase compétitive : Photon Fusion (Host/Server Mode) pour prédiction, lag compensation sub-tick et robustesse mobile.
3. Scale dédié : Unity Matchmaker + Multiplay ou backend Nakama authoritative pour serveurs gérés.

### Backend Live-Ops Et Économie

Les services back-office couvrent comptes, inventaire, progression et analytics.
- Unity Gaming Services : Economy, Cloud Save, Analytics pour rester dans l'écosystème Unity.
- Nakama : solution open-source complète (comptes, amis, chat, stockage, leaderboards, matchmaking, RPC/cron) avec SDK Unity/Godot.

### Audio Et Synchronisation Du Rythme

La précision du beat est essentielle pour le scoring et les combos tempo-parfait.
- Middleware FMOD ou Wwise pour baliser BPM, mesures et markers et exposer des callbacks fiables côté jeu.
- Appui sur la timeline + DSP clock plutôt que sur un détecteur temps réel approximatif.
- Vision long terme : scoring via caméra joueur (MediaPipe Pose, MoveNet) pour évaluer la synchronisation IRL.

### Animation Et Contenu Danse

Les assets doivent être fidèles aux mouvements salsa tout en permettant de la génération procédurale.
- Packs d'animations + retarget + IK complétés par l'IA motion-to-motion (ex. Kinetix SDK Unity) pour créer des émotes à partir de vidéo/texte.
- Move.ai pour capturer des pas salsa précis via multi-iPhone sans studio spécialisé.

### Monétisation, Analytics Et Social

Les fondations économiques et sociales soutiennent la longévité du jeu.
- Unity IAP 5 + Economy pour la conformité StoreKit 2 / Google Billing v7 et la validation serveur.
- Unity Analytics, GameAnalytics ou Firebase Remote Config pour funnels, rétention D1/D7 et A/B testing.
- Vivox (Unity) pour chat vocal basse latence dans les rooms, couplé aux Lobbies pour invitations privées/publiques.

## Architecture Réseau Progressive

Le déploiement recommandé suit des jalons clairs pour sécuriser l'expérience multijoueur.
- Pré-alpha : gameplay solo avec fantômes IA, intégration FMOD/Wwise, économie locale.
- Alpha : Unity NGO + Relay + Lobby, 4 à 10 joueurs, prédiction simple et premières protections anti-triche.
- Bêta : migration vers Photon Fusion, comptes/inventaire sur Nakama ou UGS prod, matchmaking structuré.
- Scale : règles de matchmaking (ELO, région, latence), serveurs dédiés Multiplay ou orchestrés Nakama, voice Vivox.

## Points De Vigilance

Anticiper les zones de risque facilite l'exécution produit et business.
- Licences musicales : privilégier pistes royalty-free, créations originales ou accords spécifiques pour un modèle F2P centré sur les cosmétiques.
- Latence et beat : choisir des régions relais proches puis passer au serveur dédié pour le PvP avancé.
- Conformité IAP 2025 : Google Play Billing v7 obligatoire fin août 2025, alignement via Unity IAP 5.

## Verdict

L'angle MOBA de danse top-down reste inoccupé et les technos actuelles rendent le concept viable.
- Positionnement blue ocean face aux jeux de danse existants.
- Stack conseillé : Unity 6 + URP + UGS pour le MVP, puis Photon Fusion et Nakama/Multiplay à mesure que la compétition s'intensifie.
