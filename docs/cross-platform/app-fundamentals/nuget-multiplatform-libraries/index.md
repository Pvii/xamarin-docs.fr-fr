---
title: Projets NuGet (Nugetizer 3000)
description: Ce document décrit comment utiliser l’outil Nugetizer 3000 pour créer automatiquement les packages NuGet pour partager du code entre plateformes.
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: f79ed775173e05dd850fbc74c53127b63c0d5f34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780768"
---
# <a name="nuget-projects-nugetizer-3000"></a>Projets NuGet (Nugetizer 3000)

_Créer automatiquement les packages NuGet pour partager du code entre plateformes à l’aide de la « 3000 Nugetizer' !_

Il est possible de créer automatiquement les packages NuGet pour partager du code entre plateformes à l’aide de la _Nugetizer 3000_. Cela rend est possible de créer des packages NuGet dans des projets de bibliothèque existants ou en créant un **projet de bibliothèque multiplateforme**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Le 3000 Nugetizer est inclus dans Visual Studio pour Mac 6.2.

[![](images/mulitplatform-library-sml.png "Créer une nouvelle fenêtre de la bibliothèque de plateformes multiples")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour utiliser le 3000 Nugetizer dans Visual Studio, veuillez [Téléchargez et exécutez le programme d’installation VSIX](http://bit.ly/nugetizer-2017).

-----

## <a name="building-nuget-packages"></a>Packages NuGet de génération

Une fois configuré, chaque génération du projet génère un package NuGet terminé, ce qui peut être utilisé pour partager du code en interne avec d’autres applications ou téléchargé sur [NuGet.org](https://www.nuget.org).

Il existe trois scénarios d’utilisation de cette fonctionnalité :

- [Projets de bibliothèque existants](existing-library.md)

  Créer un package NuGet dans des projets de bibliothèque de classes portables (ou .NET Standard) existants.

- [Création d’un nouveau projet de bibliothèque multiplateforme](single-codebase.md)

  Créer une nouvelle bibliothèque pour partager le code commun via NuGet, à l’aide d’une bibliothèque de classes portables ou .NET Standard.

- [Création de nouveaux projets de bibliothèque spécifique à la plateforme](platform-specific.md)

  Créer une nouvelle bibliothèque et NuGet qui inclut du code spécifique à la plateforme pour iOS et Android et utilise un projet partagé pour contenir le code commun et les projets spécifiques à la plateforme pour prendre en charge les fonctionnalités spécifiques à iOS ou Android.

Reportez-vous à la [guide de métadonnées](metadata.md) pour plus d’informations sur les métadonnées obligatoires et facultatifs qui doivent être ajoutées à n’importe quel package NuGet.


## <a name="further-nuget-information"></a>Plus d’informations de NuGet

En savoir plus sur [création manuelle de NuGets pour Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md) et comment [incluent un package NuGet dans une application](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Microsoft [Documentation de NuGet](https://docs.microsoft.com/nuget/) contient des informations plus détaillées sur le **.nupkg** format et à l’aide de packages NuGet dans Visual Studio.

La discussion de conception pour les projets de Package NuGet (aussi appelé) NuGetizer 3000) est disponible sur le [référentiel NuGet GitHub](https://github.com/NuGet/Home/wiki/NuGetizer-3000).


## <a name="related-links"></a>Liens associés

- [Cas d’utilisation de NuGetizer-3000](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Créer manuellement les Packages NuGet pour Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)
- [Documentation de NuGet](https://docs.microsoft.com/nuget/)
- [Notes de publication](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#NuGetizer_3000)
