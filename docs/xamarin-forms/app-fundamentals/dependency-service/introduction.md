---
title: Introduction aux DependencyService
description: "Comprendre le fonctionne de DependencyService pour accéder aux fonctionnalités de plateforme natifs"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 74b22f31fabf70885eca732ef021232124df71bb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-dependencyservice"></a>Introduction aux DependencyService

## <a name="overview"></a>Vue d'ensemble

[`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) permet aux applications d’appeler dans les fonctionnalités spécifiques à la plateforme à partir de code partagé. Cette fonctionnalité permet à des applications de Xamarin.Forms faire tout ce qu’une application native peut faire.

`DependencyService` est un résolveur de dépendance. Dans la pratique, une interface est définie et `DependencyService` recherche l’implémentation correcte de cette interface à partir de divers projets de plateforme.

## <a name="how-dependencyservice-works"></a>Fonctionne de DependencyService

Les applications de Xamarin.Forms besoin de trois composants à utiliser `DependencyService`:

- **Interface** &ndash; la fonctionnalité requise est définie par une interface dans le code partagé.
- **Mise en œuvre par la plateforme** &ndash; les Classes qui implémentent l’interface doivent être ajoutés à chaque projet de plateforme.
- **L’enregistrement** &ndash; chaque classe d’implémentation doit être inscrite avec `DependencyService` via un attribut de métadonnées. L’inscription active `DependencyService` pour rechercher la classe d’implémentation et de fournir à la place de l’interface au moment de l’exécution.
- **L’appel à DependencyService** &ndash; partagé code doit appeler explicitement `DependencyService` à poser pour les implémentations de l’interface.

Notez que les implémentations doivent être fournies pour chaque projet de plateforme dans votre solution. Les projets de plateforme sans les implémentations échoue lors de l’exécution.

La structure de l’application est expliquée dans le schéma suivant :

![](introduction-images/overview-diagram.png "Structure de l’Application DependencyService")

### <a name="interface"></a>Interface

L’interface que vous concevez allez définir la façon dont vous interagissez avec des fonctionnalités spécifiques à la plateforme. Soyez prudent si vous développez un composant à être partagé comme un composant ou un package Nuget. Conception de l’API peut ou l’échec d’un package. L’exemple ci-dessous spécifie une interface simple pour parler de texte qui autorise une grande souplesse pour spécifier les mots à énoncer mais laisse l’implémentation pour être personnalisées pour chaque plateforme :

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>Mise en œuvre par la plateforme

Une fois qu’une interface appropriée a été conçue, cette interface doit être implémentée dans le projet pour chaque plateforme que vous ciblez. Par exemple, les classes suivantes implémentent la `ITextToSpeech` interface sur Windows Phone :

```csharp
namespace TextToSpeech.WinPhone
{
  public class TextToSpeechImplementation : ITextToSpeech
  {
      public TextToSpeechImplementation() {}

      public async void Speak(string text)
      {
          SpeechSynthesizer synth = new SpeechSynthesizer();
          await synth.SpeakTextAsync(text);
      }
  }
}
```

Notez que toutes les implémentations doivent avoir un constructeur (sans paramètre) par défaut par ordre de `DependencyService` pour être en mesure de l’instancier. Constructeurs sans paramètre ne peut pas être définies par l’interface.

### <a name="registration"></a>Inscription

Chaque implémentation de l’interface doit être inscrite avec `DependencyService` avec un attribut de métadonnées. Le code suivant inscrit l’implémentation pour Windows Phone :

```csharp
using TextToSpeech.WinPhone;

[assembly: Xamarin.Forms.Dependency (typeof (TextToSpeechImplementation))]
namespace TextToSpeech.WinPhone {
  ...
```

Vue d’ensemble, l’implémentation spécifique à la plateforme ressemble à ceci :

```csharp
[assembly: Xamarin.Forms.Dependency (typeof (TextToSpeechImplementation))]
namespace TextToSpeech.WinPhone {
  public class TextToSpeechImplementation : ITextToSpeech
  {
      public TextToSpeechImplementation() {}

      public async void Speak(string text)
      {
          SpeechSynthesizer synth = new SpeechSynthesizer();
          await synth.SpeakTextAsync(text);
      }
  }
}
```

Remarque : l’inscription effectuée au niveau de l’espace de noms, pas au niveau de la classe.

#### <a name="universal-windows-platform-net-native-compilation"></a>Plateforme Windows universelle .NET la Compilation Native

Les projets UWP qui utilisent l’option de compilation .NET Native doivent suivre un [configuration légèrement différentes](~/xamarin-forms/platform/windows/installation/universal.md#target-invocation-exception) lors de l’initialisation de Xamarin.Forms. Compilation .NET native nécessite également d’inscription légèrement différente pour les services de dépendance.

Dans le **App.xaml.cs** inscrire manuellement chaque service de dépendance définie dans le projet UWP à l’aide de fichiers, le `Register<T>` (méthode), comme indiqué ci-dessous :

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

Remarque : à l’aide de l’inscription manuelle `Register<T>` est uniquement efficace dans version builds à l’aide de la compilation .NET Native. Si vous omettez cette ligne, les versions Debug continueront de fonctionner, mais les versions Release échoue charger le service de dépendance.

### <a name="call-to-dependencyservice"></a>L’appel à DependencyService

Une fois que le projet a été configuré avec une interface commune et des implémentations pour chaque plateforme, utilisez `DependencyService` pour obtenir de l’implémentation appropriée lors de l’exécution :

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` trouve l’implémentation correcte de l’interface `T`.

### <a name="solution-structure"></a>Structure de la solution

Le [UsingDependencyService solution échantillon](https://developer.xamarin.com/samples/UsingDependencyService/) est indiqué ci-dessous pour iOS et Android, avec les modifications de code ci-dessus mis en surbrillance.

 [![iOS et Android solution](introduction-images/solution-sml.png "DependencyService exemple de Structure de Solution")](introduction-images/solution.png#lightbox "DependencyService exemple de Structure de Solution")

> [!NOTE]
> Vous **doit** fournir une implémentation dans chaque projet de plateforme. Si aucune implémentation de l’Interface n’est enregistrée, puis le `DependencyService` sera impossible de résoudre la `Get<T>()` méthode lors de l’exécution.


## <a name="related-links"></a>Liens associés

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Exemples Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)