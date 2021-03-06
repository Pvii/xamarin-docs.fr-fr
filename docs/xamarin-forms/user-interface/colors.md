---
title: Couleurs de Xamarin.Forms
description: Xamarin.Forms fournit une classe de couleur inter-plateformes flexible. Cet article explique les fonctionnalités fournies par la classe de couleur et comment l’utiliser.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 45adcb8a0fe25e729211e8b166be51ce2c4d93bd
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243016"
---
# <a name="colors-in-xamarinforms"></a>Couleurs de Xamarin.Forms

_Xamarin.Forms fournit une classe de couleur inter-plateformes flexible._

Cet article présente les différentes façons la `Color` classe peut être utilisée dans Xamarin.Forms.

La `Color` classe fournit plusieurs méthodes pour créer une instance de couleur

-  **Couleurs nommées** -une collection de commun les couleurs nommées, y compris `Red`, `Green`, et `Blue`.
-  **FromHex** -valeur similaire à la syntaxe utilisée dans le code HTML, par exemple, « 00FF00 » de la chaîne. Alpha est peut être spécifiée en tant que la première paire de caractères (« CC00FF00 »).
-  **FromHsla** -teinte, saturation et luminosité `double` valeurs, avec une valeur alpha facultative (0.0 1.0).
-  **FromRgb** -rouge, vert et bleu `int` valeurs (0-255).
-  **FromRgba** -rouge, vert, bleu et alpha `int` valeurs (0-255).
-  **FromUint** -définir une seule `double` valeur représentant **ARVB**.

Voici certaines couleurs de l’exemple, assignés à la `BackgroundColor` certaines étiquettes à l’aide de différentes variations de la syntaxe autorisée :

```csharp
var red    = new Label { Text = "Red",   BackgroundColor = Color.Red };
var orange = new Label { Text = "Orange",BackgroundColor = Color.FromHex("FF6A00") };
var yellow = new Label { Text = "Yellow",BackgroundColor = Color.FromHsla(0.167, 1.0, 0.5, 1.0) };
var green  = new Label { Text = "Green", BackgroundColor = Color.FromRgb (38, 127, 0) };
var blue   = new Label { Text = "Blue",  BackgroundColor = Color.FromRgba(0, 38, 255, 255) };
var indigo = new Label { Text = "Indigo",BackgroundColor = Color.FromRgb (0, 72, 255) };
var violet = new Label { Text = "Violet",BackgroundColor = Color.FromHsla(0.82, 1, 0.25, 1) };

var transparent = new Label { Text = "Transparent",BackgroundColor = Color.Transparent };
var @default = new Label    { Text = "Default",    BackgroundColor = Color.Default };
var accent = new Label      { Text = "Accent",     BackgroundColor = Color.Accent };
```

Ces couleurs sont affichées sur chaque plateforme ci-dessous. Notez la couleur finale - `Accent` -une couleur blue-ish pour iOS et Android ; cette valeur est définie par Xamarin.Forms.

 [![Démonstration de couleur](colors-images/colors-sml.png "couleur démonstration")](colors-images/colors.png#lightbox "démonstration de couleur")

## <a name="colordefault"></a>Color.Default

Utilisez le `Default` pour définir (ou nouveau) une valeur de couleur à la valeur par défaut de plate-forme (comprendre que cela représente une couleur sous-jacente différente sur chaque plateforme pour chaque propriété).

Les développeurs peuvent utiliser cette valeur pour définir un `Color` propriété mais doit **pas** cette instance de son composant valeurs RVB (ils sont tous la valeur -1) de la requête.

## <a name="colortransparent"></a>Color.Transparent

Définissez la couleur à effacer.

## <a name="coloraccent"></a>Color.Accent

Sur iOS et Android cette instance est définie sur une couleur de contraste qui est visible sur l’arrière-plan par défaut, mais n’est pas identique à la couleur du texte par défaut.

## <a name="additional-methods"></a>Méthodes supplémentaires

`Color` les instances d’incluent des méthodes supplémentaires qui peuvent être utilisés pour créer de nouvelles couleurs :

-  **AddLuminosity** -retourne une nouvelle couleur en modifiant la luminosité le delta fourni.
-  **WithHue** -retourne une nouvelle couleur, en remplaçant la teinte avec la valeur fournie.
-  **WithLuminosity** -retourne une nouvelle couleur, en remplaçant la luminosité avec la valeur fournie.
-  **WithSaturation** -retourne une nouvelle couleur, en remplaçant la saturation avec la valeur fournie.
-  **MultiplyAlpha** -retourne une nouvelle couleur en modifiant la valeur alpha, multipliant par la valeur alpha fournie.

## <a name="implicit-conversions"></a>Conversions implicites

Conversion implicite entre les `Xamarin.Forms.Color` et `System.Drawing.Color` types peuvent être effectuées :

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

Cet extrait de code utilise le `Device.RuntimePlatform` propriété à définir une couleur de manière sélective un `ActivityIndicator`:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>À l’aide de XAML

Couleurs peuvent également être facilement référencées dans XAML en utilisant les noms de couleur définie ou les représentations sous forme de Hex indiqués ici :

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

## <a name="summary"></a>Récapitulatif

Le Xamarin.Forms `Color` classe est utilisée pour créer des références de couleur plateforme prenant en charge. Il peut être utilisé dans le code partagé et XAML.


## <a name="related-links"></a>Liens associés

- [ColorsSample](https://developer.xamarin.com/samples/WorkingWithColors)
- [Sélecteur pouvant être lié (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
