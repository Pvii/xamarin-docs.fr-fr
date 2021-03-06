---
title: Configuration de Visual Studio 2017
description: Cet article décrit différentes options de configuration Xamarin.iOS pour Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 22D82244-890D-4325-B3CC-C0AC49130BCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: ee08cf7d68bd9d10026f1c15d4438077349fe367
ms.sourcegitcommit: dc6ccf87223942088ca926c0dadd5b5478c683cb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2018
---
# <a name="configuring-visual-studio-2017"></a>Configuration de Visual Studio 2017

_Cet article décrit différentes options de configuration Xamarin.iOS pour Visual Studio._

## <a name="using-matching-xamarinios-versions"></a>Utilisation de versions de Xamarin.iOS correspondantes

Visual Studio 2017 doit utiliser la même version de Xamarin.iOS que celle qui est installée sur l’hôte de build Mac. Pour vous en assurer :

 - Si vous utilisez Visual Studio 2017, sélectionnez le canal des mises à jour **Stable** dans Visual Studio pour Mac.

 - Si vous utilisez Visual Studio 2017 Preview, sélectionnez le canal des mises à jour **Alpha** dans Visual Studio pour Mac.

> [!NOTE]
> Depuis [Visual Studio 2017 version 15.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning), Visual Studio 2017 détecte automatiquement si l’hôte de build Mac utilise la même version de Xamarin.iOS que Windows. S’il existe une incompatibilité de versions, Visual Studio 2017 propose une installation à distance de la version correcte sur l’hôte de build Mac. Pour plus d’informations, examinez la section [Provisionnement Mac automatique](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning) du guide [Appairer avec un Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

## <a name="ios-toolbar"></a>Barre d’outils iOS

Quand un projet iOS est ouvert dans Visual Studio 2017, la barre d’outils iOS doit être visible.  Par défaut, elle contient quatre boutons qui sont utiles au développement Xamarin.iOS :

![Barre d’outils iOS de Visual Studio 2017](config-options-images/ios-toolbar.png "Barre d’outils iOS de Visual Studio 2017")

- **Appairer avec un Mac** : ouvre la boîte de dialogue Appairer avec un Mac. Activé quand un projet iOS est ouvert dans Visual Studio 2017.
- **Afficher le simulateur iOS** : sur l’hôte de build Mac, fait passer le simulateur iOS au premier plan. Activé quand un projet iOS est ouvert dans Visual Studio 2017.
- **Journal de l’appareil** : affiche une fenêtre qui vous permet d’examiner les journaux de l’appareil. Activé quand un projet iOS est ouvert dans Visual Studio 2017.
- **Afficher le fichier IPA sur le serveur de builds** : ouvre une fenêtre sur l’hôte de build Mac qui indique l’emplacement du fichier .ipa pour l’application. Activé à la fin d’une build pour laquelle un fichier .ipa a été créé.

Si cette barre d’outils n’apparaît pas, ouvrez le menu **Affichage** dans Visual Studio 2017, puis choisissez **Barres d’outils > iOS** :

![Activation de la barre d’outils iOS](config-options-images/ios-toolbar-enable.png "Activation de la barre d’outils iOS")

## <a name="solution-platforms-drop-down-menu"></a>Menu déroulant Plateformes solution

Le menu déroulant **Plateformes solution** vous permet de choisir si la prochaine build doit cibler un appareil physique ou un simulateur.

Pour vous assurer que ce menu déroulant est visible dans la barre d’outils standard :

- Dans Visual Studio 2017, cliquez sur la flèche bas à droite de la barre d’outils standard.
- Choisissez **Ajouter/supprimer des boutons** 
- Vérifiez que l’élément **Plateformes solution** est coché :

![Activation du menu déroulant Plateformes solution](config-options-images/solution-platforms-enable.png "Activation du menu déroulant Plateformes solution")

Avec un projet iOS ouvert, les barres d’outils **standard** et **iOS** doivent maintenant ressembler à la capture d’écran suivante :

![Barres d’outils standard et iOS](config-options-images/toolbars.png "Barres d’outils standard et iOS")


