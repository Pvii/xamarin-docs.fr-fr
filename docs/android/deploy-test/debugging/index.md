---
title: Débogage de Xamarin.Android sur des appareils et émulateurs
description: Guide pratique pour tester et déboguer votre application Xamarin.Android
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: a4ce36e28bd5b6dcf78d248b1f2ba951cad9b286
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="debugging"></a>Débogage

Cette section explique comment déboguer une application Xamarin.Android sur des appareils ou émulateurs.
## <a name="debugging-overview"></a>Vue d'ensemble du débogage

Le développement d’applications Android nécessite l’exécution de l’application, que ce soit sur du matériel physique ou un émulateur. L’utilisation de matériel est la meilleure approche, mais elle n’est pas toujours la plus pratique. Il est souvent plus simple et plus rentable de simuler/émuler le matériel Android à l’aide de l’un des émulateurs décrits ci-dessous.


### <a name="google-android-emulatorandroiddeploy-testdebuggingandroid-sdk-emulatorindexmd"></a>[Émulateur Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

Ces articles expliquent comment utiliser l’émulateur par défaut qui est fourni avec le kit Android SDK. Cet émulateur est disponible pour Visual Studio pour Windows et Visual Studio pour Mac.

### <a name="visual-studio-android-emulatorandroiddeploy-testdebuggingvisual-studio-android-emulatormd"></a>[Émulateur Android Visual Studio](~/android/deploy-test/debugging/visual-studio-android-emulator.md)

Cet article explique comment déboguer et tester votre application Xamarin.Android à l’aide de l’émulateur Android qui est intégré à Visual Studio 2015. Il constitue un bon choix si vous utilisez Visual Studio 2015 et n’avez pas besoin de profils d’appareil personnalisés.

### <a name="debugging-on-a-deviceandroiddeploy-testdebuggingdebug-on-devicemd"></a>[Débogage sur un appareil](~/android/deploy-test/debugging/debug-on-device.md)

Cet article explique comment configurer un appareil Android physique afin d’y déployer l’application Xamarin.Android directement depuis Visual Studio ou Visual Studio pour Mac.

### <a name="android-debug-logandroiddeploy-testdebuggingandroid-debug-logmd"></a>[Journal de débogage Android](~/android/deploy-test/debugging/android-debug-log.md)

L’une des astuces couramment utilisées par les développeurs pour déboguer leurs applications est d’utiliser `Console.WriteLine`. Toutefois, sur une plateforme mobile comme Android, il n’y a pas de console. Les appareils Android proposent un journal que vous aurez probablement besoin d’utiliser pendant l’écriture d’applications. On l’appelle parfois **logcat** en raison de la commande à taper pour le récupérer. Cet article décrit comment utiliser **logcat**.

> [!WARNING]
> Notez que **Xamarin Android Player** est maintenant déconseillé. Pour plus d’informations, consultez l’[annonce dans ce billet de blog](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/).
