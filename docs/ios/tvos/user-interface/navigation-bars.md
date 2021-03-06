---
title: Utilisation de tvOS, barres de Navigation dans Xamarin
description: Ce document décrit comment utiliser des barres de navigation dans une application de tvOS générée avec Xamarin. Elle explique la configuration de barres de navigation dans un plan conceptuel et répondre aux événements à partir de ces boutons.
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: b6c8ff8551c91578b9399b88e90e94c6af12ac68
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789289"
---
# <a name="working-with-tvos-navigation-bars-in-xamarin"></a>Utilisation de tvOS, barres de Navigation dans Xamarin

Barres de navigation peuvent être ajoutés en haut des vues pour afficher le titre et des boutons de barre de Navigation facultatifs. En règle générale, ils sont utilisés lorsque l’utilisateur a accédé à partir d’une page principale, comme une Table de vue, la Collection ou le Menu pour un sous-affichage présentant les détails de l’élément sélectionné.

[![](navigation-bars-images/navbar01.png "Exemple de barre de Navigation")](navigation-bars-images/navbar01.png#lightbox)

En outre, le titre (qui s’affiche dans le centre), les barres de Navigation peut contenir un ou plusieurs boutons de barre de Navigation (`UIBarButtonItem`) sur les côtés gauche et droit de la barre.

> [!IMPORTANT]
> Barres de navigation sont totalement transparents par défaut. Soyez vigilant pour vérifier que le contenu de la barre de Navigation reste accessible en lecture sur le contenu qu’il contient. Par exemple, lorsque le contenu dans un tableau ou Collection défile en dessous.

<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>Barres de navigation et les animations

Pour travailler avec les barres de Navigation dans une application Xamarin.tvOS, le plus simple consiste à ajouter à l’interface utilisateur de l’application d’utiliser le concepteur iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Dans le **Solution remplissage**, double-cliquez sur `Main.storyboard` de fichier et l’ouvre pour modification.
1. Faites glisser un **barre de Navigation** à partir de la **boîte à outils** et déposez-la sur la vue en haut de l’écran : 

    [![](navigation-bars-images/navbar02.png "Une barre de Navigation")](navigation-bars-images/navbar02.png#lightbox)
1. Double-cliquez sur le **barre de Navigation** pour **élément de Navigation**. Dans le **Widget** onglet de la **propriétés remplissage**, vous pouvez définir le **titre**: 

    [![](navigation-bars-images/navbar03.png "Définir le titre")](navigation-bars-images/navbar03.png#lightbox)
1. Ensuite, vous pouvez ajouter un ou plusieurs **éléments de la barre** à chaque extrémité de la barre : 

    [![](navigation-bars-images/navbar04.png "Un élément de bouton de la barre")](navigation-bars-images/navbar04.png#lightbox)
1. Enfin, des fils le **les éléments de bouton de barre de** à des Actions de la **événements** onglet de la **l’Explorateur de propriétés**: 

    [![](navigation-bars-images/navbar05.png "Une Action de l’élément bouton de la barre")](navigation-bars-images/navbar05.png#lightbox)
1. Enregistrez les modifications apportées.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. Dans le **l’Explorateur de solutions**, double-cliquez sur `Main.storyboard` de fichier et l’ouvre pour modification.
1. Faites glisser un **barre de Navigation** à partir de la **boîte à outils** et déposez-la sur la vue en haut de l’écran : 

    [![](navigation-bars-images/navbar02-vs.png "Une barre de Navigation")](navigation-bars-images/navbar02-vs.png#lightbox)
1. Double-cliquez sur le **barre de Navigation** pour **élément de Navigation**. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, vous pouvez définir le **titre**: 

    [![](navigation-bars-images/navbar03-vs.png "Définir le titre")](navigation-bars-images/navbar03-vs.png#lightbox)
1. Ensuite, vous pouvez ajouter un ou plusieurs **éléments de la barre** à chaque extrémité de la barre : 

    [![](navigation-bars-images/navbar04-vs.png "Un bouton éléments à barres")](navigation-bars-images/navbar04-vs.png#lightbox)
1. Enfin, des fils le **les éléments de bouton de barre de** à des Actions de la **événements** onglet de la **l’Explorateur de propriétés**: 

    [![](navigation-bars-images/navbar05-vs.png "Un bouton élément Actions à barres")](navigation-bars-images/navbar05-vs.png#lightbox)
1. Enregistrez les modifications apportées.


-----

> [!IMPORTANT]
> Bien qu’il soit possible d’attribuer des événements tels que `TouchUpInside` à un élément d’interface utilisateur (par exemple, un UIButton) dans le concepteur iOS, il ne sera jamais appelée car Apple TV n’a pas une fonction tactile d’écran ou de prendre en charge les événements tactiles. Vous devez toujours utiliser le `Primary Action` événement lors de la création de gestionnaires d’événements de tvOS éléments d’interface utilisateur.

Le code suivant donne un exemple de gestionnaires d’événements sur trois BarButtonItems différents : `ShowFirstHotel`, `ShowSecondHotel`, et `ShowThirdHotel`. Lorsque vous cliquez sur chaque élément, l’image d’arrière-plan `HotelImage` est modifiée. Il est modifié dans le contrôleur de la vue (exemple `ViewController.cs`) fichier :

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void ShowFirstHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel01.jpg");
        }

        partial void ShowSecondHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel02.jpg");
        }

        partial void ShowThirdHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel03.jpg");
        }
        #endregion
    }
}
```

Tant qu’un bouton de `Enabled` propriété est `true` et il n’est pas couverte par un autre contrôle ou une vue, elle peut être rendue de l’élément actif à l’aide de l’élément distant Siri.

Pour plus d’informations sur l’utilisation des plans conceptuels, veuillez consulter notre [Hello, Guide de démarrage rapide de tvOS](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert la conception et l’utilisation des barres de Navigation à l’intérieur d’une application Xamarin.tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
