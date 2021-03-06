---
title: Les pilotes USB ai-je besoin pour déboguer Android sur Windows ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2018
ms.openlocfilehash: 7b3a4c2f807839897a099959fe3a6ea9ec25df78
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732747"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Les pilotes USB ai-je besoin pour déboguer Android sur Windows ?

## <a name="finding-usb-drivers"></a>Recherche de pilotes USB

Pour déboguer sur un appareil Android lors du développement de Windows ; Vous devez installer un pilote USB compatible. Le Gestionnaire du Kit de développement logiciel Android inclut le « pilote USB Google » par défaut, ce qui ajoute la prise en charge pour les appareils Nexus comme décrit ici : [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

Autres périphériques nécessitent des pilotes USB spécifiquement publiés par le fabricant du périphérique. Certains liens correspondant aux fabricants courants sont inclus dans ce guide : [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>Alternatives

Selon le manfacturer, il peut être difficile à localiser le pilote USB exact nécessité. Autres possibilités pour le test des applications Android développement dans Windows, y compris à l’aide d’un émulateur Android ou à l’aide des services de tests externes. Certaines interfaces incluent :

- [Application Center Test](https://docs.microsoft.com/appcenter/test-cloud/) - Cloud services s’exécutent sur des centaines d’appareils Android réel de test.

- [Émulateur Visual Studio pour Android](https://www.visualstudio.com/en-us/features/msft-android-emulator-vs.aspx)

- [Débogage avec l’émulateur Android de Google](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

