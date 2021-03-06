---
title: Transformations de matrice dans SkiaSharp
description: Cet article explore plus approfondis pour les transformations SkiaSharp avec la matrice de transformation polyvalente et cela est illustré par l’exemple de code.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: 816a5f47a4a4f5c01a1fb20b5c8e7a2fc83a64b0
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244875"
---
# <a name="matrix-transforms-in-skiasharp"></a>Transformations de matrice dans SkiaSharp

_Plonger dans les transformations SkiaSharp avec la matrice de transformation polyvalente_

Toutes les transformations appliquées à la `SKCanvas` objet sont consolidées dans une seule instance de la [ `SKMatrix` ](https://developer.xamarin.com/api/type/SkiaSharp.SKMatrix/) structure. Il s’agit d’une matrice de transformation de 3-par-3 standard similaire à celles de tous les systèmes de graphiques 2D modernes.

Comme vous l’avez vu, vous pouvez utiliser des transformations SkiaSharp sans savoir à propos de la transformation de matrice, mais la matrice de transformation est important du point de vue théorique, et il est essentiel lors de l’utilisation de transformations pour modifier des chemins d’accès ou pour la gestion des entrées tactiles complexes, les deux qui sont présenté dans cet article et la suivante.

![](matrix-images/matrixtransformexample.png "Une image bitmap soumise à une transformation affine")

La matrice de transformation actuelle appliquée à la `SKCanvas` n’est disponible à tout moment en accédant à la lecture seule [ `TotalMatrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.TotalMatrix/) propriété. Vous pouvez définir une nouvelle matrice de transformation à l’aide du [ `SetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.SetMatrix/p/SkiaSharp.SKMatrix/) (méthode) et que vous pouvez restaurer cette matrice de transformation pour les valeurs par défaut en appelant [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix/).

Le seul autre `SKCanvas` membre qui fonctionne directement avec la matrice de transformation de la zone de dessin est [ `Concat` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Concat/p/SkiaSharp.SKMatrix@/) qui concatène deux matrices en multipliant les ensemble.

La matrice de transformation par défaut est la matrice d’identité et se compose de 1 dans les cellules en diagonale 0 partout ailleurs :

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

Vous pouvez créer une matrice d’identité à l’aide de la méthode statique [ `SKMatrix.MakeIdentity` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeIdentity()/) méthode :

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

Le `SKMatrix` constructeur par défaut ne *pas* renvoyer une matrice d’identité. Il renvoie une matrice avec toutes les cellules à zéro. N’utilisez pas le `SKMatrix` constructeur sauf si vous envisagez de définir ces cellules manuellement.

Lorsque SkiaSharp rend un objet de graphique, chaque point (x, y) est converti efficacement en matrice avec un 1 dans la troisième colonne 1-par-3 :

<pre>
| x  y  1 |
</pre>

Cette matrice 1-par-3 représente un point en trois dimensions par la coordonnée Z la valeur 1. Il existe des raisons mathématiques (décrits plus loin) raison pour laquelle une transformation de matrice à deux dimensions requiert le travail en trois dimensions. Vous pouvez considérer cette matrice 1-par-3 qui représente un point dans un système de coordonnées 3D, mais toujours sur le plan 2D où Z est égal à 1.

Cette matrice 1-par-3 est ensuite multipliée par la matrice de transformation et le résultat est le point de s’affiche sur la zone de dessin :

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

Les points convertis à l’aide de la multiplication des matrices standard, sont les suivantes :

x' = x

y' = y

z' = 1

Qui est la transformation par défaut.

Lorsque le `Translate` méthode est appelée sur le `SKCanvas` objet, le `tx` et `ty` arguments à la `Translate` méthode deviennent les deux premières cellules de la troisième ligne de la matrice de transformation :

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

La multiplication est désormais comme suit :

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

Voici les formules de transformation :

x' = x + tx

y' = y + ty

Les facteurs d’échelle ont une valeur par défaut de 1. Lorsque vous appelez le `Scale` méthode sur un nouvel `SKCanvas` de l’objet, la matrice de transformation résultant contient la `sx` et `sy` arguments dans les cellules en diagonale :

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

Les formules de transformation sont les suivantes :

x' = sx · x

y' = sy · y

La matrice de transformation après l’appel `Skew` contient les deux arguments dans les cellules de matrice adjacents les facteurs d’échelle :

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

Les formules de transformation sont :

x' = x + xSkew · y

y' = ySkew · x + y

Pour un appel à `RotateDegrees` ou `RotateRadians` pour un angle de α, la matrice de transformation est la suivante :

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

Voici les formules de transformation :

x' = cos(α) · x - sin(α) · y

y' = sin(α) · x - cos(α) · y

Lorsque α est 0 degrés, il est la matrice d’identité. Lorsque α est 180 degrés, la matrice de transformation est la suivante :

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

Une rotation de 180 degrés est équivalente à retourner un objet horizontalement et verticalement, qui est également possible en définissant les facteurs d’échelle de -1.

Tous ces types de transformations sont classés comme *affine* transforme. Transformations affines impliquent jamais la troisième colonne de la matrice, qui reste à la valeur par défaut de 0, 0 et 1. L’article [transformations affines Non](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md) traite des transformations non affines.

## <a name="matrix-multiplication"></a>Multiplication des matrices

Un des avantages majeurs à l’aide de la matrice de transformation sont que les transformations composites peuvent être obtenues en multiplication des matrices, qui est souvent appelée dans la documentation SkiaSharp *concaténation*. Les méthodes de transformation de nombreux `SKCanvas` fait référence à « concaténation préliminaire » ou « pre-concat. » Cela fait référence à l’ordre de multiplication, ce qui est importante, car la multiplication de matrice n’est pas commutative.

Par exemple, la documentation relative à la [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) méthode indique qu’il « Pré-concats la matrice actuelle avec la traduction spécifiée, » lors de la documentation pour le [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) méthode indique qu’il « Pré-concats la matrice actuelle avec l’échelle spécifiée. »

Cela signifie que la transformation spécifiée par l’appel de méthode est le multiplicateur (l’opérande de gauche) et la matrice de transformation actuelle est multiplicande (l’opérande de droite).

Supposons que `Translate` est appelée suivi `Scale`:

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

Le `Scale` transformation est multipliée par la `Translate` transformer pour la matrice de transformation composite :

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale` peut être appelée avant `Translate` comme suit :

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

Dans ce cas, l’ordre de la multiplication est inversée, et les facteurs d’échelle sont effectivement appliquées à des facteurs de traduction :

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

Voici le `Scale` méthode avec un point de pivot :

```csharp
canvas.Scale(sx, sy, px, py);
```

Elle est équivalente aux appels traduire et l’échelle suivants :

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Les trois matrices de transformation sont multipliés dans l’ordre inverse à partir de la façon dont les méthodes apparaissent dans le code :

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

### <a name="the-skmatrix-structure"></a>La Structure SKMatrix

Le `SKMatrix` structure définit neuf propriétés en lecture/écriture de type `float` correspondant aux neuf cellules de la matrice de transformation :

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` définit également une propriété nommée [ `Values` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix.Values/) de type `float[]`. Cette propriété peut être utilisée pour définir ou obtenir les valeurs de neuf en une seule opération dans l’ordre `ScaleX`, `SkewX`, `TransX`, `SkewY`, `ScaleY`, `TransY`, `Persp0`, `Persp1`, et `Persp2`.

Le `Persp0`, `Persp1`, et `Persp2` cellules sont décrites dans l’article, [transformations affines Non](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md). Si ces cellules ont leurs valeurs par défaut de 0, 0 et 1, la transformation est multipliée par un point de coordonnée comme suit :

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

x' = ScaleX · x + SkewX · y + TransX

y' = SkewX · x + ScaleY · y + TransY

z' = 1

Il s’agit de la transformation affine à deux dimensions terminée. La transformation affine conserve les lignes parallèles, ce qui signifie qu’un rectangle n’est jamais transformé en autre chose qu’un parallélogramme.

Le `SKMatrix` structure définit plusieurs méthodes statiques pour créer `SKMatrix` valeurs. Elles retournent toutes `SKMatrix` valeurs :

- [`MakeTranslation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeTranslation/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/System.Single/System.Single/) avec un point pivot
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/) pour un angle en radians
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/System.Single/System.Single/) pour un angle en radians, avec un point pivot
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/)
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/System.Single/System.Single/) avec un point pivot
- [`MakeSkew`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeSkew/p/System.Single/System.Single/)

`SKMatrix` définit également plusieurs méthodes statiques qui concatènent deux matrices, ce qui signifie que pour les multiplier. Ces méthodes sont nommées `Concat`, `PostConcat`, et `PreConcat`, et il existe deux versions de chaque. Ces méthodes n’ont aucune valeur de retour ; au lieu de cela, elles font référence à existant `SKMatrix` valeurs via `ref` arguments. Dans l’exemple suivant, `A`, `B`, et `R` (pour « result ») sont tous `SKMatrix` valeurs.

Les deux `Concat` méthodes sont appelées à ceci :

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

Ces effectuer la multiplication suivante :

R = B × A

Les autres méthodes ont seulement deux paramètres. Le premier paramètre est modifié et lors du retour de l’appel de méthode, contient le produit de deux matrices. Les deux `PostConcat` méthodes sont appelées à ceci :

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

Ces appels effectuer l’opération suivante :

A = A × B

Les deux `PreConcat` méthodes sont similaires :

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

Ces appels effectuer l’opération suivante :

A = B × A

Les versions de ces appels de méthode avec tous les `ref` arguments sont légèrement plus efficaces en appelant les implémentations sous-jacent, mais il peut être difficile de la lecture de votre code et en supposant qu’il n’est pas défini avec un `ref` argument est modifié par la méthode. En outre, il est souvent plus commode de passer un argument qui est un résultat de l’un de le `Make` méthodes, par exemple :

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

Cette opération crée le tableau suivant :

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

Il s’agit de la transformation d’échelle multipliée par la transformation de traduction. Dans ce cas particulier, la `SKMatrix` structure fournit un raccourci avec une méthode nommée [ `SetScaleTranslate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.SetScaleTranslate/p/System.Single/System.Single/System.Single/System.Single/):

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

C’est un des délais quelques lorsqu’il est plus sûr d’utiliser le `SKMatrix` constructeur. Le `SetScaleTranslate` méthode définit tous les neuf cellules de la matrice. Il est également recommandé d’utiliser le `SKMatrix` constructeur avec la méthode statique `Rotate` et `RotateDegrees` méthodes :

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

Ces méthodes effectuent *pas* concaténer une transformation de rotation à un fichier existant. Les méthodes de définissent toutes les cellules de la matrice. Elles sont fonctionnellement identiques à la `MakeRotation` et `MakeRotationDegrees` méthodes, sauf qu’elles n’instanciez le `SKMatrix` valeur.

Supposez que vous avez un `SKPath` objet que vous souhaitez afficher, mais que vous préférez qu’il a une orientation différente ou un autre point. Vous pouvez modifier toutes les coordonnées de ce chemin d’accès en appelant le [ `Transform` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Transform/p/SkiaSharp.SKMatrix/) méthode `SKPath` avec un `SKMatrix` argument. Le **transformer du chemin d’accès** page montre comment effectuer cette opération. Le [ `PathTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs) références de classe le `HendecagramPath` objet dans un champ mais utilise son constructeur pour appliquer une transformation à ce chemin d’accès :

```csharp
public class PathTransformPage : ContentPage
{
    SKPath transformedPath = HendecagramArrayPage.HendecagramPath;

    public PathTransformPage()
    {
        Title = "Path Transform";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        SKMatrix matrix = SKMatrix.MakeScale(3, 3);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(360f / 22));
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(300, 300));

        transformedPath.Transform(matrix);
    }
    ...
}
```

Le `HendecagramPath` objet a un centre à (0, 0), et les points d’onze de l’étoile étendent vers l’extérieur à partir de ce centre de 100 unités dans toutes les directions. Cela signifie que le chemin d’accès a des coordonnées positifs et négatifs. Le **transformer du chemin d’accès** page préfère fonctionnent avec une étoile trois fois en tant que grand et toutes les coordonnées positives. En outre, elle ne veut pas un point de l’étoile pour pointer vers le haut. Il souhaite à la place pour un point de l’étoile pour pointer directement vers le bas. (L’étoile ayant onze points, il ne peut pas avoir les deux.) Cela nécessite de faire pivoter l’étoile de 360 degrés divisé en 22.

Le constructeur génère une `SKMatrix` objet à partir de trois transformations distinctes à l’aide de la `PostConcat` méthode avec le modèle suivant, où A, B et C sont des instances de `SKMatrix`:

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

Il s’agit d’une série de multiplications successives, par conséquent, le résultat est comme suit :

UN KIT DE DÉVELOPPEMENT × B × C

L’aide de multiplications consécutifs à comprendre ce que fait chaque transformation. La transformation d’échelle augmente la taille des coordonnées de chemin d’accès par un facteur de 3, afin des coordonnées comprise entre – 300 et 300. La transformation de rotation fait pivoter l’étoile autour de son origine. La transformation de traduction déplace ensuite 300 pixels vers la droite et vers le bas, par conséquent, toutes les coordonnées deviennent positives.

Voici les autres séquences qui produisent la même matrice. Voici un autre :

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

Cela fait pivoter le chemin d’accès autour de son centre tout d’abord et le traduit 100 pixels vers la droite et vers le bas de toutes les coordonnées sont positives. L’étoile est ensuite augmenté la taille par rapport à son angle supérieur gauche nouveaux, qui est le point (0, 0).

Le `PaintSurface` gestionnaire peut rendre simplement ce chemin d’accès :

```csharp
public class PathTransformPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Magenta;
            paint.StrokeWidth = 5;

            canvas.DrawPath(transformedPath, paint);
        }
    }
}

```

Il s’affiche dans le coin supérieur gauche de la zone de dessin :

[![](matrix-images/pathtransform-small.png "Capture d’écran de triple de la page de la transformation de chemin d’accès")](matrix-images/pathtransform-large.png#lightbox "Triple capture d’écran de la page de la transformation de chemin d’accès")

Le constructeur de ce programme s’applique à la matrice pour le chemin d’accès avec l’appel suivant :

```csharp
transformedPath.Transform(matrix);
```

Le chemin d’accès est *pas* conserver cette matrice en tant que propriété. Au lieu de cela, il applique la transformation à toutes les coordonnées du chemin d’accès. Si `Transform` est appelée, la transformation est appliquée à nouveau, et vous pouvez revenir en arrière impérativement en appliquant une autre matrice qui annule la transformation. Heureusement, le `SKMatrix` structure définit un [ `TryInverse` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.TryInvert/p/SkiaSharp.SKMatrix/) méthode qui obtient la matrice qui inverse d’une matrice donnée :

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

La méthode est appelée `TryInverse` car pas toutes les matrices sont réversible, mais une matrice non réversible n’est pas susceptible d’être utilisé pour une transformation de graphique.

Vous pouvez également appliquer une transformation de matrice à un `SKPoint` valeur, un tableau de points, une `SKRect`, ou simplement un nombre unique au sein de votre programme. Le `SKMatrix` structure prend en charge ces opérations avec une collection de méthodes qui commencent par le mot `Map`, telles que celles-ci :

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

Si vous utilisez cette dernière méthode, n’oubliez pas que le `SKRect` structure n’est pas capable de représenter un rectangle avec une rotation. La méthode convient uniquement un `SKMatrix` valeur représentant la traduction et de mise à l’échelle.

### <a name="interactive-experimentation"></a>Expérimentation interactive

Une façon de faire une idée de la transformation affine est en déplacement trois angles d’une image bitmap autour de l’écran et voir quel transformation génère de manière interactive. C’est l’idée derrière le **afficher une matrice Affine** page. Cette page nécessite deux autres classes sont également utilisées dans les autres démonstrations :

Le [ `TouchPoint` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchPoint.cs) classe affiche un cercle translucide qui peut être déplacé dans l’écran. `TouchPoint` requiert qu’une `SKCanvasView` ou un élément qui est un parent d’un `SKCanvasView` ont le [ `TouchEffect` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchEffect.cs) attaché. Affectez à la propriété `Capture` la valeur `true`. Dans le `TouchAction` Gestionnaire d’événements, le programme doit appeler la `ProcessTouchEvent` méthode dans `TouchPoint` pour chaque `TouchPoint` instance. La méthode retourne `true` si l’événement tactile a provoqué le déplacement de point de contact. En outre, le `PaintSurface` gestionnaire doit appeler le `Paint` méthode dans chaque `TouchPoint` instance, en lui passant le `SKCanvas` objet.

`TouchPoint` montre la même façon qu’un élément visuel SkiaSharp pouvant être encapsulé dans une classe distincte. La classe peut définir des propriétés permettant de spécifier les caractéristiques de l’élément visuel et une méthode nommée `Paint` avec un `SKCanvas` argument peut l’afficher.

Le `Center` propriété du `TouchPoint` indique l’emplacement de l’objet. Cette propriété peut être définie pour initialiser l’emplacement ; la propriété change lorsque l’utilisateur fait glisser le cercle autour de la zone de dessin.

Le **afficher la Page de matrice Affine** requiert également le [ `MatrixDisplay` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/MatrixDisplay.cs) classe. Cette classe affiche les cellules d’un `SKMatrix` objet. Il a deux méthodes publiques : `Measure` pour obtenir les dimensions de la matrice rendue, et `Paint` pour l’afficher. La classe contient un `MatrixPaint` propriété de type `SKPaint` qui peuvent être remplacés pour une autre taille de police ou de couleur.

Le [ **ShowAffineMatrixPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) fichier instancie le `SKCanvasView` et attache un `TouchEffect`. Le [ **ShowAffineMatrixPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) fichier code-behind crée trois `TouchPoint` des objets et leur affecte les positions correspondant aux trois angles d’une image bitmap qu’il charge à partir d’un ressource :

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    SKMatrix matrix;
    SKBitmap bitmap;
    SKSize bitmapSize;

    TouchPoint[] touchPoints = new TouchPoint[3];

    MatrixDisplay matrixDisplay = new MatrixDisplay();

    public ShowAffineMatrixPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }

        touchPoints[0] = new TouchPoint(100, 100);                  // upper-left corner
        touchPoints[1] = new TouchPoint(bitmap.Width + 100, 100);   // upper-right corner
        touchPoints[2] = new TouchPoint(100, bitmap.Height + 100);  // lower-left corner

        bitmapSize = new SKSize(bitmap.Width, bitmap.Height);
        matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                           touchPoints[1].Center,
                                           touchPoints[2].Center);
    }
    ...
}
```

Une matrice affine est définie de manière unique par trois points. Les trois `TouchPoint` objets correspondent aux angles supérieur gauche, supérieur droit et inférieur gauche de l’image bitmap. Comme une matrice affine n’est capable de transformer un rectangle dans un parallélogramme, le quatrième point est défini par les trois autres. Le constructeur se termine par un appel à `ComputeMatrix`, qui calcule les cellules d’un `SKMatrix` objet à partir de ces trois points.

Le `TouchAction` appels du Gestionnaire du `ProcessTouchEvent` méthode de chaque `TouchPoint`. Le `scale` valeur convertit Xamarin.Forms coordonnées en pixels :

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = canvasView.CanvasSize.Width / (float)canvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                               touchPoints[1].Center,
                                               touchPoints[2].Center);
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

Le cas échéant `TouchPoint` a été déplacé, la méthode appelle `ComputeMatrix` à nouveau et invalide la surface.

Le `ComputeMatrix` méthode détermine la matrice impliquée par ces trois points. La matrice appelé `A` transformations un rectangle carré un pixel dans un parallélogramme basé sur les trois points, tandis que la transformation d’échelle appelée `S` met à l’échelle l’image bitmap à un rectangle carré un pixel. La matrice composite est `S` × `A`:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL)
    {
        // Scale transform
        SKMatrix S = SKMatrix.MakeScale(1 / size.Width, 1 / size.Height);

        // Affine transform
        SKMatrix A = new SKMatrix
        {
            ScaleX = ptUR.X - ptUL.X,
            SkewY = ptUR.Y - ptUL.Y,
            SkewX = ptLL.X - ptUL.X,
            ScaleY = ptLL.Y - ptUL.Y,
            TransX = ptUL.X,
            TransY = ptUL.Y,
            Persp2 = 1
        };

        SKMatrix result = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

Enfin, le `PaintSurface` méthode restitue la bitmap en fonction de cette matrice, la matrice s’affiche au bas de l’écran et restitue les points tactiles dans les trois angles de l’image bitmap :

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap using the matrix
        canvas.Save();
        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(matrix);

        matrixDisplay.Paint(canvas, matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));

        // Display the touchpoints
        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
        }
    }
  }
```

L’écran d’iOS ci-dessous montre l’image bitmap lorsque la page est chargée, tandis que les deux autres écrans de l’afficher après quelques manipulation :

[![](matrix-images/showaffinematrix-small.png "Capture d’écran de triple de la page Afficher la matrice Affine")](matrix-images/showaffinematrix-large.png#lightbox "Triple capture d’écran de la page Afficher la matrice Affine")

Bien qu’il semble que si les points tactiles faites glisser les angles de l’image bitmap, qui est uniquement une impression. La matrice calculée à partir des points tactiles transforme l’image bitmap afin que les angles coïncident avec les points tactiles.

Il est plus naturel pour les utilisateurs à déplacer, redimensionner et faire pivoter des images bitmap ne pas en faisant glisser les angles, mais en utilisant un ou deux doigts directement sur l’objet à faire glisser, pincement et faire pivoter. Ce sujet est abordé dans l’article suivant [Touch une Manipulation](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md).

### <a name="the-reason-for-the-3-by-3-matrix"></a>La raison de la matrice 3 x 3

Il se peut qu’un système de graphiques à deux dimensions nécessite uniquement une matrice de transformation de 2 x 2 :

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

Cela fonctionne pour la mise à l’échelle, de rotation et l’inclinaison de même, mais il n’est pas capable des plus élémentaires des transformations, sont la traduction.

Le problème est que la matrice 2 x 2 représente un *linéaire* transformer en deux dimensions. Une transformation linéaire conserve des opérations arithmétiques de base, mais l’une des implications est qu’une transformation linéaire ne modifie jamais le point (0, 0). Une transformation linéaire rendant la traduction.

En trois dimensions, une matrice de transformation linéaire ressemble à ceci :

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

La cellule marquée `SkewXY` signifie que la valeur inclinaisons de la coordonnée X en fonction des valeurs y ; la cellule `SkewXZ` signifie que la valeur inclinaisons de la coordonnée X en fonction des valeurs de Z ; et valeurs d’inclinaison de la même façon pour les autres `Skew` cellules.

Il est possible de limiter cette matrice de transformation 3D à un plan à deux dimensions en définissant `SkewZX` et `SkewZY` à 0, et `ScaleZ` 1 :

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

Si les graphiques à deux dimensions sont dessinés entièrement sur le plan dans un espace 3D où Z est égal à 1, la multiplication de transformation ressemble à ceci :

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Rien sur le plan à deux dimensions où Z est égal à 1, mais la `SkewXZ` et `SkewYZ` cellules deviennent efficacement des facteurs de translation à deux dimensions.

Il s’agit d’une transformation linéaire à trois dimensions comment sert d’une transformation non linéaire à deux dimensions. (Par analogie, les transformations dans les graphiques 3D sont basées sur une matrice 4 x 4).

Le `SKMatrix` structure dans SkiaSharp définit des propriétés pour cette troisième ligne :

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

Valeurs non nul de `Persp0` et `Persp1` résultat dans transformations qui déplacent des objets en dehors du plan à deux dimensions où Z est égal à 1. Que se passe-t-il lorsque ces objets sont déplacés vers ce plan est traitée dans l’article sur [transformations affines Non](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md).


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (exemple)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
