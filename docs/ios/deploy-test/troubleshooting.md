---
title: Résolution des problèmes
description: Conseils et astuces pour un déploiement sans problème
ms.prod: xamarin
ms.assetid: 65286D09-F74D-4F22-B6CD-D1BCD7FC7992
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/23/2017
ms.openlocfilehash: fe7425bbf6440317cc856d2c727874298f66bc33
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>Résolution des problèmes

## <a name="code-signing--provisioning"></a>Signature de code et provisionnement

La signature de code et le provisionnement avec iOS sont des processus assez délicats. Il est donc important de vous assurer que les certificats de signature de code et les profils de provisionnement sont correctement organisés.

* Les grandes équipes doivent éviter d’utiliser le bouton « Fix issue » dans Xcode, illustré ici :

    [![](troubleshooting-images/fixissue.png "Boîte de dialogue Fix issue")](troubleshooting-images/fixissue.png#lightbox)

    Cette action entraîne la création de certificats et profils de provisionnement supplémentaires. Dans le meilleur des cas, elle crée un profil de provisionnement chaque fois qu’un membre de l’équipe clique sur le bouton, ce qui nuit à la bonne organisation des profils. Dans le pire des cas, cette action révoque les certificats des autres utilisateurs dans l’entreprise, lesquels ne peuvent alors plus utiliser leurs applications.

* Gérez bien l’accès au trousseau, et supprimez les certificats et profils qui ont expiré. Les certificats d’entreprise ont une durée de validité de trois ans, alors que les autres restent valides pendant un an uniquement. Les certificats ne pouvant pas être renouvelés, il faut créer d’autres certificats juste avant l’expiration des anciens. Assurez-vous de révoquer et de supprimer les anciens certificats, puis de resigner les applications avec les nouveaux certificats.

* Supprimez les anciens profils de provisionnement quand les nouveaux profils sont installés. Ainsi, Visual Studio pour Mac n’a pas à choisir quel profil utiliser. Pour cela, supprimez d’abord le profil dans le centre pour développeurs d’Apple, puis accédez à *Préférences > Votre compte > Afficher les détails...*. Sélectionnez le profil de provisionnement, puis cliquez sur **Afficher dans le Finder**. L’emplacement du profil dans le système de fichiers Mac s’affiche. Vous pouvez ensuite supprimer le profil à l’aide du Finder.

* Assurez-vous que tous les certificats requis et les clés privées correspondantes sont disponibles. Chaque membre de l’équipe doit avoir un certificat de développeur (pour installer des applications sur son propre appareil) et un certificat de distribution (pour installer des applications sur les autres appareils)

* Relancez Xcode et Visual Studio pour Mac ou Visual Studio après l’installation d’un nouveau certificat ou profil de provisionnement.


## <a name="testflight"></a>TestFlight

Parfois, les tests ne se déroulent pas de la manière prévue.  Les étapes suivantes peuvent vous aider à résoudre les problèmes rencontrés avec TestFlight :

- TestFlight est disponible uniquement pour les applications ciblant iOS 8 et les versions ultérieures.

- L’application doit avoir un *profil de distribution sur l’App Store* avec le droit bêta.

- Le formulaire de **soumission de la nouvelle application iOS** doit contenir exactement les mêmes informations que le fichier **Info.plist** de l’application, et avoir toutes ses sections renseignées. Les icônes doivent être spécifiées pour l’application avant le chargement dans TestFlight.

- Après le chargement d’une nouvelle build, il faut attendre entre 1 et 5 minutes pour voir la build s’afficher dans iTunes Connect.

- Le [bouton TestFlight Beta Testing](~/ios/deploy-test/testflight.md#beta-testing) doit être activé pour chaque version de votre application.

- Le bouton **Testeur interne** doit être activé pour chaque membre de l’équipe de développement qui a aussi été ajouté comme testeur interne.

- Les utilisateurs qui ont un compte iTunes Connect, ou qui y sont associés, ne peuvent pas être testeurs internes. Ils peuvent uniquement être ajoutés comme testeurs externes.

- Les utilisateurs internes et externes sont ajoutés, sélectionnés et invités de manière individuelle. Les listes doivent être gérées séparément.

- Apple doit approuver chaque build qui va être distribuée aux testeurs externes. Si la version d’une build change, Apple doit effectuer une nouvelle revue de la bêta. Si le numéro de build change, la revue est facultative.

- Les métadonnées doivent être ajoutées aux builds qui sont distribuées aux testeurs externes. Pour ce faire, cliquez sur le numéro de build dans **Mes apps > Prerelease**.

- Deux builds au maximum peuvent être soumises pour revue chaque jour. Étant donné qu’un changement de version donne obligatoirement lieu à une revue, les numéros de version peuvent uniquement être changés deux fois par jour.

<a name="Automatically_copy_app_bundles_back_to_Windows" />

## <a name="automatically-copy-app-bundles-back-to-windows"></a>Copier automatiquement les bundles .app sur Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]
