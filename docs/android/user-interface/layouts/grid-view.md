---
title: GridView
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 82c82de912fd253d45e6343e2dd1c50e389c6371
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="gridview"></a>GridView

[`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) est un [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) qui affiche les éléments dans une grille à deux dimensions, permettant le défilement. Les éléments de grille sont automatiquement insérés à la disposition en utilisant un [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/).

Dans ce didacticiel, vous allez créer une grille d’images miniatures. Lorsqu’un élément est sélectionné, un message de toast affichera la position de l’image.

Démarrer un nouveau projet nommé **HelloGridView**.

Rechercher des photos que vous souhaitez utiliser, ou [télécharger ces exemples d’images](http://developer.android.com/shareables/sample_images.zip). Ajoutez les fichiers image dans le fichier **ressources/Drawable** active. Dans le **propriétés** fenêtre, définir l’Action de génération pour chacune d’elles pour **AndroidResource**.

Ouvrez le **Resources/Layout/Main.axml** de fichier et insérez le code suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gridview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:columnWidth="90dp"
    android:numColumns="auto_fit"
    android:verticalSpacing="10dp"
    android:horizontalSpacing="10dp"
    android:stretchMode="columnWidth"
    android:gravity="center"
/>
```

Cela [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) remplira la totalité de l’écran. Les attributs sont plutôt explicite. Pour plus d’informations sur les attributs valides, consultez la [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) référence.

Ouvrez `HelloGridView.cs` et insérez le code suivant pour le [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) méthode :

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    var gridview = FindViewById<GridView> (Resource.Id.gridview);
    gridview.Adapter = new ImageAdapter (this);

    gridview.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

Après le **Main.axml** mise en page est défini pour l’affichage du contenu, le [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) est capturée à partir de la mise en page avec [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/). Le [ `Adapter` ](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/) propriété est ensuite utilisée pour définir un adaptateur personnalisé (`ImageAdapter`) comme source pour tous les éléments à afficher dans la grille. Le `ImageAdapter` est créé à l’étape suivante.

Pour faire en cas de clic sur un élément dans la grille, un délégué anonyme est abonné à la [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) événement.
Il montre un [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) qui affiche la position d’index (base zéro) de l’élément sélectionné (dans un scénario réel, la position pourrait servir à obtenir l’image de la taille complète pour une autre tâche). Notez que les classes d’écouteur Java-style peuvent être utilisés au lieu des événements .NET.

Créez une classe appelée `ImageAdapter` qui sous-classe [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
        context = c;
    }

    public override int Count {
        get { return thumbIds.Length; }
    }

    public override Java.Lang.Object GetItem (int position)
    {
        return null;
    }

    public override long GetItemId (int position)
    {
        return 0;
    }

    // create a new ImageView for each item referenced by the Adapter
    public override View GetView (int position, View convertView, ViewGroup parent)
    {
        ImageView imageView;

        if (convertView == null) {  // if it's not recycled, initialize some attributes
            imageView = new ImageView (context);
            imageView.LayoutParameters = new GridView.LayoutParams (85, 85);
            imageView.SetScaleType (ImageView.ScaleType.CenterCrop);
            imageView.SetPadding (8, 8, 8, 8);
        } else {
            imageView = (ImageView)convertView;
        }

        imageView.SetImageResource (thumbIds[position]);
        return imageView;
    }

    // references to our images
    int[] thumbIds = {
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7
    };
}
```

Tout d’abord, il implémente certaines méthodes requises héritées [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/). Le constructeur et le [ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/) propriété sont explicites. Normalement, [ `GetItem(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/) doit retourner l’objet réel, à la position spécifiée dans l’adaptateur, mais elle est ignorée pour cet exemple. De même, [ `GetItemId(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/) doit retourner l’id de ligne de l’élément, mais il n’est pas nécessaire ici.

La première méthode nécessaire est [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/).
Cette méthode crée un nouveau [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) pour chaque image ajoutée à la `ImageAdapter`. Lorsqu’elle est appelée, un [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) est passé, qui est normalement un objet recyclé (au moins une fois que cela a été appelée une seule fois), donc une vérification pour voir si l’objet est null. Si elle *est* null, une [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) est instancié et configuré avec les propriétés souhaitées pour la présentation de l’image :

- [`LayoutParams`](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) définit la hauteur et la largeur de la vue&mdash;Cela garantit que, quel que soit la taille de la drawable, chaque image est redimensionnée et rognée pour s’ajuster à ces dimensions, comme il convient.

- [`SetScaleType()`](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetScaleType/) déclare que les images doivent être rognées vers le centre (si nécessaire).

- [`SetPadding(int, int, int, int)`](https://developer.xamarin.com/api/member/Android.Views.View.SetPadding/) définit le remplissage pour tous les côtés. (Notez que, si les images ont des proportions différentes, puis moins remplissage provoqueront de rognage plus de l’image si elle ne correspond pas aux dimensions indiquées à la ImageView).

Si le [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) passé à [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/) est *pas* null, alors que l’ordinateur local [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) est initialisé avec le recyclage [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) objet.

À la fin de la [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/) (méthode), la `position` entier passé dans la méthode est utilisée pour sélectionner une image à partir la `thumbIds` tableau, qui est défini en tant que la ressource d’image pour le [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).

Tout ce qui reste consiste à définir le `thumbIds` tableau des ressources drawable.

Exécutez l'application. La mise en page de grille doit ressembler à ceci :

[![Capture d’écran de l’exemple de GridView qui affiche des 15 images](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png#lightbox)

Tenter d’expérimenter avec les comportements de la [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) et [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) éléments en modifiant leurs propriétés. Par exemple, au lieu d’utiliser [ `LayoutParams` ](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) essayer d’utiliser [ `SetAdjustViewBounds()` ](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetAdjustViewBounds/).


## <a name="references"></a>Références

-   [`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).

*Des parties de cette page sont des modifications en fonction de travail créés et partagés par projet Android Open Source utilisés en fonction des conditions décrites dans le*
[*Creative Commons 2.5 Attribution de licence* ](http://creativecommons.org/licenses/by/2.5/).
