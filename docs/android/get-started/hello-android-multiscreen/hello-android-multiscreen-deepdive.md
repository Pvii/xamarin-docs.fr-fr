---
title: Hello, Android multi-écran - En profondeur
description: Dans ce guide en deux parties, l’application Phoneword de base (créée dans le guide Hello, Android) est étendue de manière à gérer un second écran. Vous y découvrirez également les composants d’une application Android. Vous allez plonger au cœur de l’architecture Android afin de mieux comprendre la structure et les fonctionnalités des applications Android.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E4150036-7760-4023-BD33-B7BDE7B7AF5B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: f1c19d43aa1f9010307df3fb954ac1029221ccd4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="hello-android-multiscreen-deep-dive"></a>Hello, Android multi-écran - En profondeur

_Dans ce guide en deux parties, l’application Phoneword de base (créée dans le guide Hello, Android) est étendue de manière à gérer un second écran. Vous y découvrirez également les composants d’une application Android. Vous allez plonger au cœur de l’architecture Android afin de mieux comprendre la structure et les fonctionnalités des applications Android._

## <a name="hello-android-multiscreen-deep-dive"></a>Hello, Android multi-écran - En profondeur

Dans [Hello, Android multi-écran - Démarrage rapide](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md), vous avez généré et exécuté votre première application Xamarin.Android multi-écran.
Il est temps maintenant d’approfondir votre connaissance de la navigation et de l’architecture Android afin de pouvoir générer des applications plus complexes.

Dans ce guide, vous allez explorer l’architecture Android plus avancée avec la présentation des *composants d’une application* Android. Vous allez découvrir la navigation Android avec des *intentions* et explorer les options de navigation matérielle. Les éléments ajoutés à l’application Phoneword sont examinés afin de vous donner une vue plus globale des relations de l’application avec le système d’exploitation et d’autres applications.


## <a name="android-architecture-basics"></a>Principes de base de l’architecture Android

Dans [Hello, Android - En profondeur](~/android/get-started/hello-android/hello-android-deepdive.md), vous avez appris que les applications Android sont des programmes uniques, car ils ne disposent pas d’un point d’entrée unique. Le système d’exploitation (ou une autre application) démarre toute activité enregistrée de l’application qui, à son tour, démarre le processus de l’application. Cette plongée au cœur de l’architecture Android va vous permettre de mieux comprendre la manière dont les applications Android sont construites avec la présentation des composants d’une application Android et de leurs fonctions.


### <a name="android-application-blocks"></a>Blocs d’application Android

Une application Android se compose d’une collection de classes Android spéciales appelées *blocs d’application*, regroupées avec des ressources d’application - images, thèmes, classes d’assistance, etc. Elles sont coordonnées par un fichier XML appelé le *manifeste Android*.

Les blocs d’application constituent l’armature des applications Android, car ils permettent de faire ce qui est normalement impossible de faire avec une classe normale. Les deux plus importants sont _Activités_ et _Services_ :

-   **Activité** &ndash; Une activité correspond à un écran avec une interface utilisateur. D’un point de vue conceptuel, une activité est semblable à une page web dans une application web. Par exemple, dans une application de flux d’actualités, l’écran de connexion serait la première activité, la liste déroulante des articles serait une autre activité et la page de détails de chaque article en serait une troisième. Plus d’informations sur les activités, consultez le guide [Cycle de vie des activités](~/android/app-fundamentals/activity-lifecycle/index.md).

-   **Service** &ndash; Les services Android prennent en charge les activités en prenant le contrôle des tâches de longue durée et en les exécutant en arrière-plan. Les services ne disposent pas d’une interface utilisateur et sont utilisés pour traiter les tâches qui ne sont pas liées à des écrans, par exemple écouter une chanson en arrière-plan ou charger des photos sur un serveur. Pour plus d’informations sur les services, consultez les guides [Création de Services](~/android/app-fundamentals/services/index.md) et [Services Android](~/android/app-fundamentals/services/index.md).


Une application Android peut ne pas utiliser tous les types de blocs et en utilise souvent plusieurs du même type. Par exemple, l’application Phoneword développée dans [Hello, Android - Démarrage rapide](~/android/get-started/hello-android/hello-android-quickstart.md) est composée d’une seule activité (écran) et de fichiers de ressources. Une application de lecteur de musique simple peut avoir plusieurs activités et un service pour la lecture de musique lorsque l’application est en arrière-plan.

### <a name="intents"></a>Intentions

Autre concept fondamental des applications Android, l’*intention*.
Android est conçu autour du *principe des privilèges minimum*. Les applications ont accès uniquement aux blocs dont elles ont besoin pour fonctionner et elles ont un accès limité aux blocs qui composent le système d’exploitation ou les autres applications. De même, les blocs sont *sommairement couplés*. Ils sont conçus pour n’avoir que peu de connaissance des autres blocs et un accès limité à ceux-ci (même les blocs faisant partie de la même application).

Pour communiquer, les blocs d’application s’envoient des messages asynchrones appelés *intentions*. Les intentions contiennent des informations sur le bloc de réception et parfois des données. Une intention envoyée depuis un composant d’application déclenche une action dans un autre composant d’application, liant ainsi les deux composants d’application et leur permettant de communiquer. Par l’échange d’intentions, les blocs peuvent coordonner des actions complexes comme lancer l’application appareil-photo pour prendre des photos et les enregistrer, collecter des informations sur l’emplacement ou naviguer d’un écran à un autre.


### <a name="androidmanifestxml"></a>AndroidManifest.XML

Lorsque vous ajoutez un bloc à l’application, il est enregistré dans un fichier XML spécial appelé le **manifeste Android**. Le manifeste suit tous les blocs d’application dans une application, ainsi que les conditions de version, les autorisations et les bibliothèques liées : tout ce que le système d’exploitation a besoin de connaître pour exécuter votre application. Le **manifeste Android** travaille également avec les activités et les intentions pour contrôler les actions qui sont appropriées pour une activité donnée. Ces fonctionnalités avancées du manifeste Android sont abordées dans le Guide [Utilisation du manifeste Android](~/android/platform/android-manifest.md).

Dans la version à écran unique de l’application Phoneword, seule une activité, une intention et le manifeste `AndroidManifest.xml` étaient utilisés, avec des ressources supplémentaires, comme les icônes. Dans la version multi-écran de Phoneword, une activité supplémentaire a été ajoutée . Elle est lancée à partir de la première activité à l’aide d’une intention. La section suivante explique comment les intentions permettent de créer la navigation dans les applications Android.

## <a name="android-navigation"></a>Navigation Android

Les intentions sont utilisées pour naviguer entre les écrans. Il est temps de plonger dans ce code pour savoir comment fonctionnent les intentions et comprendre leur rôle dans la navigation Android.


### <a name="launching-a-second-activity-with-an-intent"></a>Lancement d’une seconde activité avec une intention

Dans l’application Phoneword, une intention a été utilisée pour lancer un second écran (activité). Commencez par créer une intention, en passant le *Contexte* actuel (`this`, faisant référence au **Contexte** actuel) et le type de bloc d’application que vous recherchez (`TranslationHistoryActivity`) :

```csharp
Intent intent = new Intent(this, typeof(TranslationHistoryActivity));
```

Le **contexte** est une interface vers des informations générales sur l’environnement de l’application. Il informe les nouveaux objets créés de ce qui se passe avec l’application. Si vous envisagez une intention comme un message, vous fournissez le nom du destinataire du message (`TranslationHistoryActivity`) et l’adresse du destinataire (`Context`).

Android fournit une option permettant de joindre des données simples à une intention (les données complexes sont traitées différemment). Dans l’exemple Phoneword, `PutStringArrayExtra` est utilisé pour joindre une liste de numéros de téléphone à l’intention et `StartActivity` est appelé sur le destinataire de l’intention. Le code complet se présente ainsi :

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", _phoneNumbers);
    StartActivity(intent);
};
```


## <a name="additional-concepts-introduced-in-phoneword"></a>Autres concepts introduits dans Phoneword

L’application Phoneword a introduit plusieurs concepts qui ne sont pas traités dans ce guide. Ces concepts sont notamment :

**Ressources de type chaîne** &ndash; Dans l’application Phoneword, le texte de `TranslationHistoryButton` était défini sur `"@string/translationHistory"`. La syntaxe `@string` signifie que la valeur de la chaîne est stockée dans le _fichier de ressources de chaîne_, **Strings.xml**. La valeur suivante a été ajoutée à **Strings.xml** pour la chaîne `translationHistory` :

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Call History</string>
</resources>
```

Pour plus d’informations sur les ressources de chaîne et les autres ressources Android, reportez-vous au guide [Ressources Android](~/android/app-fundamentals/resources-in-android/index.md).

**ListView et ArrayAdapter** &ndash; Un _ListView_ est un composant de l’interface utilisateur qui fournit un moyen simple pour présenter une liste déroulante de lignes. Une instance de `ListView` requiert un _Adapter_ qui le remplit avec des données contenues dans les affichages de ligne. La ligne de code suivante a été utilisée pour remplir l’interface utilisateur de `TranslationHistoryActivity` :

```csharp
this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
```

Les ListViews et les Adapters n’entrent pas dans le cadre de ce document, mais ils sont traités dans le guide très complet [ListViews et Adapters](~/android/user-interface/layouts/list-view/index.md).
[Remplissage d’un affichage de liste avec des données](~/android/user-interface/layouts/list-view/populating.md) traite spécifiquement de l’utilisation des classes `ListActivity` et `ArrayAdapter` intégrées pour créer et remplir un `ListView` sans définir une disposition personnalisée, comme cela a été fait dans l’exemple Phoneword.


## <a name="summary"></a>Récapitulatif

Félicitations, vous avez terminé votre première application Android multi-écran ! Dans ce guide, vous avez découvert les *composants des applications Android* et les *intentions* qui ont ensuite été utilisées pour générer une application Android multi-écran. Vous avez maintenant acquis les bases solides dont vous avez besoin pour commencer à développer vos propres applications Xamarin.Android.

Ensuite, vous allez apprendre à générer des applications multiplateformes avec Xamarin dans les [guides Génération d’applications multiplateformes](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
