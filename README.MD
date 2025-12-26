# 🍷 Wine, Proton et UMU : Le Guide du Gaming sur Linux

Ces dernières années, l'écosystème Linux a fait des pas de géant pour permettre l'exécution des applications et jeux Windows. Si les projets **Wine** et **Proton** sont devenus des piliers incontournables, de nouvelles solutions émergent pour s'affranchir du client Steam, notamment le projet **UMU Launcher**.

Ce guide explore les fondations de ces technologies et détaille comment UMU Launcher s'appuie sur les avancées de Valve pour offrir une expérience de jeu autonome et conteneurisée.

---

## 📚 Lexique Essentiel

Avant de plonger dans la technique, voici quelques concepts clés à maîtriser :

> **📦 Conteneur**
> Un environnement isolé dans lequel un logiciel s'exécute. Il inclut ses propres librairies et dépendances sans interférer avec le système hôte.
>
> **🔥 Pressure-vessel**
> Technologie de conteneurisation développée par Valve. Elle crée une "bulle" isolée regroupant tout le nécessaire (bibliothèques graphiques, dépendances) pour que les jeux tournent de manière fiable.
>
> **⚙️ Steam Runtime Tools / Steam Linux Runtime (SLR)**
> Ensemble d'outils et de bibliothèques fournis par Steam. Ils garantissent que les jeux s'exécutent dans un environnement standardisé, peu importe votre distribution Linux (Arch, Ubuntu, Fedora, etc.).

---

## 1. Wine et Proton : Les Fondations

### 🍷 Wine
**Wine** (*Wine Is Not an Emulator*) est un logiciel libre permettant d'exécuter des applications Windows sur Linux ou macOS.
* **Fonctionnement :** Il ne virtualise pas Windows (ce qui serait lent), mais **réimplémente les API Windows**. Il traduit à la volée les appels système Windows en appels compréhensibles par Linux.
* **Usage :** C'est le "couteau suisse" historique pour tout type de logiciel (.exe).

### ⚛️ Proton
**Proton** est un fork (version modifiée) de Wine, développé par Valve.
* **Objectif :** Faciliter et optimiser l'exécution des **jeux** Windows sur Linux via Steam.
* **La différence :** Contrairement à Wine qui vise une compatibilité généraliste, Proton inclut des outils de haute performance comme **DXVK** et **vkd3d-proton** et se concentre sur l'intégration parfaite avec le client Steam.

---

## 2. Le pari de Vulkan : La traduction graphique

Sous Linux, DirectX (technologie Microsoft) n'existe pas nativement. Pour obtenir des performances quasi-natives, Wine et Proton utilisent des couches de traduction vers **Vulkan**, une API graphique ouverte et performante.

| Technologie Windows | Outil de traduction Linux | Résultat |
| :--- | :--- | :--- |
| **DirectX 9, 10, 11** | 👉 **DXVK** | Convertit les appels en Vulkan. |
| **DirectX 12** | 👉 **VKD3D-Proton** | Version optimisée par Valve pour convertir DX12 en Vulkan. |

---

## 3. Les différentes saveurs de Proton

Valve et la communauté proposent plusieurs branches pour répondre à tous les besoins.

### Les versions officielles (Valve)
* **Proton Stable** (ex: 9.0-4) : Version figée et testée en profondeur. C'est le choix de la sécurité.
* **Proton Next** : La future version stable en avant-première, pour tester les régressions.
* **Proton Experimental** : Le laboratoire. Contient les dernières fonctionnalités et correctifs non encore stabilisés. Idéal pour les jeux très récents.
* **Proton Hotfix** : Version d'urgence temporaire déployée pour corriger un bug critique sur un jeu populaire spécifique.
* **Proton Bleeding Edge** : Branche de développement brute mise à jour en continu. Très instable, à réserver aux développeurs/testeurs aguerris.

### Les versions communautaires
* **Proton-GE (Glorious Eggroll)** : C'est la version "Custom" la plus célèbre.
    * **Pourquoi l'utiliser ?** Valve, en tant qu'entreprise, ne peut pas inclure certains codecs propriétaires (licences) ou des patchs non officiels. Proton-GE intègre ces codecs (nécessaires pour certaines cinématiques) et des correctifs rapides issus de Wine-Staging.
    * **Réactivité :** GE réagit souvent plus vite que Valve pour des correctifs spécifiques (ex: un patch pour Battle.net qui casse), là où Valve doit suivre un processus de validation strict (QA).

---

## 4. Steam Runtime et WINEPREFIX

### 🛠️ Steam Runtime
C'est l'assurance "anti-casse". Valve fournit un ensemble de bibliothèques figées. Grâce à la technologie `pressure-vessel`, le jeu tourne dans ce runtime isolé plutôt que d'utiliser les bibliothèques de votre système, évitant les conflits de versions.

### 📂 WINEPREFIX
C'est le dossier qui simule le disque `C:\` de Windows.
* Il contient une base de registre simulée et l'arborescence de fichiers.
* **Bonne pratique :** Chaque jeu possède idéalement son propre WINEPREFIX pour éviter qu'un jeu ne casse la configuration d'un autre.

---

## 5. Résumé du processus de lancement

1.  **Isolation :** Création d'un conteneur `pressure-vessel` (environnement stable).
2.  **Initialisation :** Préparation du `WINEPREFIX` (le faux Windows).
3.  **Traduction Système :** Wine/Proton interceptent les appels CPU.
4.  **Traduction Graphique :** DXVK/VKD3D convertissent les graphismes en Vulkan.
5.  **Optimisation :** Application des patchs spécifiques au jeu.

---

## 🌋 UMU Launcher : L'unification hors Steam

### Qu'est-ce que UMU ?
**UMU-launcher** (Unified Linux Wine Game Launcher) est un projet révolutionnaire qui permet d'utiliser la technologie Proton et les Runtimes de Valve **sans lancer le client Steam**.

C'est le chaînon manquant pour les launchers tiers comme **Lutris**, **Heroic Games Launcher** ou **Bottles**.

> **💡 Le saviez-vous ?**
> "Umu" désigne un four traditionnel polynésien chauffé par des pierres volcaniques.
> L'analogie est parfaite : l'Umu est la "plaque de cuisson" sur laquelle on pose le `pressure-vessel` (récipient sous pression) de Valve pour "cuisiner" (exécuter) les jeux.

### Pourquoi UMU est important ?
Avant UMU, lancer un jeu Epic ou GOG avec Proton pouvait être instable car on perdait l'environnement conteneurisé de Steam (`pressure-vessel`).

**Les avantages d'UMU :**
* ✅ **Indépendance :** Plus besoin de lancer Steam en arrière-plan.
* ✅ **Standardisation :** Si un jeu tourne sur Steam, il tournera avec UMU car il utilise le même environnement (Steam Linux Runtime).
* ✅ **Base de données collaborative :** UMU utilise un *Game ID* pour appliquer automatiquement les bons correctifs (fixes) maintenus par la communauté (Glorious Eggroll, Lutris, etc.).
* ✅ **Agnostique :** Le comportement est identique quelle que soit la distribution Linux (Arch, Fedora, Ubuntu...).

---

## 📝 En Résumé : Que choisir ?

Voici la règle d'or pour choisir votre outil :

1.  **Applications Bureautique :** Utilisez **Wine**.
2.  **Jeux sur Steam :** Utilisez **Proton Experimental** (ou Stable).
3.  **Jeux sur Steam (problème de vidéo) :** Utilisez **Proton-GE** dans Steam.
4.  **Jeux HORS Steam (Epic, GOG, Battle.net) :** Utilisez **Proton-GE via UMU** (intégré dans Heroic/Lutris).

## 🔗 Sources et Ressources

* [GitHub officiel de UMU Launcher](https://github.com/Open-Wine-Components/umu-launcher)
* [Wiki officiel de Proton](https://github.com/ValveSoftware/Proton/wiki)
* [GitHub de DXVK](https://github.com/doitsujindev/dxvk)
* [GitHub de VKD3D-proton](https://github.com/HansKristian-Work/vkd3d-proton)
* [Interview de Glorious Eggroll (YouTube)](https://www.youtube.com/watch?v=SAn8N_H9Yv8)
* [Documentation Valve : Pressure-vessel](https://gitlab.steamos.cloud/steamrt/steam-runtime-tools/-/blob/main/docs/pressure-vessel.md)
