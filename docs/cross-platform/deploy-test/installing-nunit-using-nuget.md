---
title: "Installation de NUnit 2.6.4 à l’aide de NuGet"
description: "Ce guide explique comment repasser de la version 3.0 à la version 2.6.4 de NUnit à l’aide de NuGet."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7683F2B8-7FDF-48C4-8E7D-649D4D4E79F0
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: 9dc50aeec88131a1ce49c7e3357382c019774450
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="installing-nunit-264-using-nuget"></a>Installation de NUnit 2.6.4 à l’aide de NuGet

_Ce guide explique comment passer à d’une version NUnit 3.0 à une version antérieure NUnit 2.6.4 à l’aide de NuGet._

Les développeurs qui écrivent des programmes de test dans Visual Studio pour Mac ou Xamarin.UITest doivent utiliser [NUnit 2.6.4](http://nunit.org/index.php?p=docHome&r=2.6.4), car NUnit 3.0 et les versions ultérieures ne sont pas compatibles avec Visual Studio pour Mac et Xamarin.UITest. Si vous utilisez NUnit 3.0, les tests unitaires exécutés dans Visual Studio pour Mac ou Xamarin.UITests échoueront.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Ce guide explique comment installer NUnit 2.6.4 à l’aide de NuGet pour Visual Studio pour Mac. Ces étapes permettront également de désinstaller NUnit 3.0, si nécessaire.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ce guide explique comment repasser de la version 3.0 à la version 2.6.4 de NUnit à l’aide de NuGet dans Visual Studio 2015.

-----

## <a name="requirements"></a>Configuration requise

Ce guide part du principe que vous disposez déjà d’une solution comprenant un projet d’application mobile et un projet de test.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

## <a name="installing-nunit-264-in-visual-studio-for-mac"></a>Installation de NUnit 2.6.4 dans Visual Studio pour Mac

Les étapes qui suivent expliquent comment installer NUnit 2.6.4.


1. **Ouvrez le gestionnaire de package** : Cliquez avec le bouton droit sur **Packages**, puis sélectionnez **Ajouter des packages** dans le menu contextuel :

    [![](installing-nunit-using-nuget-images/add-packages-xs.png "Cliquez avec le bouton droit sur Packages, puis sélectionnez Ajouter des packages dans le menu contextuel")](installing-nunit-using-nuget-images/add-packages-xs.png)
    
1. **Recherchez `NUnit version:2.6.4`** : Visual Studio pour Mac désinstalle NUnit 3.0 (si nécessaire), puis télécharge et installe NUnit 2.6.4. Dans la boîte de dialogue **Ajouter des packages**, entrez le texte `nunit version:2.6.4` dans le champ de **recherche** situé dans le coin supérieur droit. Sélectionnez **NUnit** dans les résultats de la recherche, puis cliquez sur le bouton **Ajouter un package** :

    [![](installing-nunit-using-nuget-images/nunit-search-xs.png "Sélectionnez NUnit dans les résultats de la recherche, puis cliquez sur le bouton Ajouter un package")](installing-nunit-using-nuget-images/nunit-search-xs.png)


Vous pouvez vérifier que NUnit 2.6.4 a été installé en regardant le numéro de version du package NUnit dans le panneau Solutions :

[![](installing-nunit-using-nuget-images/nunit-2-6-4-installed.png "Vérifiez le numéro de version du package NUnit dans le panneau Solutions")](installing-nunit-using-nuget-images/nunit-2-6-4-installed.png)

## <a name="summary"></a>Récapitulatif

Dans ce guide, vous avez vu comment repasser de la version 3.0 à la version 2.6.4 de NUnit dans Visual Studio pour Mac, à l’aide de la console du gestionnaire de package.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="installing-nunit-264-in-visual-studio"></a>Installation de NUnit 2.6.4 dans Visual Studio

Cette section aborde l’utilisation de la _Console du gestionnaire de package NuGet_ dans Visual Studio 2015, en vue de désinstaller NUnit 3.0 et d’installer NUnit 2.6.4.


1. **Démarrez la console du gestionnaire de package NuGet** : Sélectionnez **Outils > Gestionnaire de package NuGet > Console du gestionnaire de package** :

    [![](installing-nunit-using-nuget-images/package-manager-console.png "Démarrez la console du gestionnaire de package NuGet : Sélectionnez Outils > Gestionnaire de package NuGet > Console du gestionnaire de package")](installing-nunit-using-nuget-images/package-manager-console.png)
    
1. **Vérifiez la version de NUnit** : Si vous le souhaitez, vous pouvez vérifier la version de NUnit qui est installée en exécutant la commande `Get-Package -Project <UITEST PROJECT>` :

    ```bash
    [PM] Get-Package -Project [TEST PROJECT NAME]
    
        Id                                  Versions                                 ProjectName
        --                                  --------                                 -----------
    NUnit                               {3.0.1}                                  [TEST PROJECT NAME]
    Xamarin.UITest                      {1.2.0}                                  [TEST PROJECT NAME]
    ```

Si vous voyez NUnit 3.0 ou une version ultérieure, vous devrez passer à la version 2.6.4.

1. **Désinstallez NUnit 3.0** : Utilisez l’applet de commande `Uninstall-Package` pour désinstaller NUnit 3.0 :

        <PM> Uninstall-Package NUnit -Project <TEST PROJECT NAME>
        Attempting to gather dependencies information for package 'NUnit.3.0.1' with respect to project '<TEST PROJECT NAME>', targeting '.NETFramework,Version=v4.5'
        Resolving actions to uninstall package 'NUnit.3.0.1'
        Resolved actions to uninstall package 'NUnit.3.0.1'
        Removed package 'NUnit.3.0.1' from 'packages.config'
        Successfully uninstalled 'NUnit.3.0.1' from <TEST PROJECT NAME>

1. **Installez NUnit 2.6.4** : Installez NUnit 2.6.4 avec l’applet de commande `Install-Package`, comme illustré dans l’extrait de code suivant :

        <PM> Install-Package NUnit -Version 2.6.4 -Project <TEST PROJECT NAME>
        Attempting to gather dependencies information for package 'NUnit.2.6.4' with respect to project '<TEST PROJECT NAME>', targeting '.NETFramework,Version=v4.5'
        Attempting to resolve dependencies for package 'NUnit.2.6.4' with DependencyBehavior 'Lowest'
        Resolving actions to install package 'NUnit.2.6.4'
        Resolved actions to install package 'NUnit.2.6.4'
        Adding package 'NUnit.2.6.4' to folder 'Z:\Desktop\DowngradeNunit\packages'
        Added package 'NUnit.2.6.4' to folder 'Z:\Desktop\DowngradeNunit\packages'
        Added package 'NUnit.2.6.4' to 'packages.config'
        Successfully installed 'NUnit 2.6.4' to <TEST PROJECT NAME>
    
## <a name="summary"></a>Récapitulatif

Dans ce guide, vous avez vu comment repasser de la version 3.0 à la version 2.6.4 de NUnit dans Visual Studio 2015, à l’aide de la console du gestionnaire de package.

-----

## <a name="related-links"></a>Liens associés

- [NUnit 2.6.4](http://nunit.org/index.php?p=docHome&r=2.6.4)
- [Package NUnit 2.6.4 NuGet](https://www.nuget.org/packages/NUnit/2.6.4)