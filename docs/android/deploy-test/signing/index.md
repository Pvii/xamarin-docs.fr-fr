---
title: "Signature du paquet d’application Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8E3EFBB2-F8AD-C126-5F32-7FD140791E53
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/26/2018
ms.openlocfilehash: 20a28d475e58a58a98abe21203e9841b7824fe48
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="signing-the-android-application-package"></a>Signature du paquet d’application Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Cette section décrit le workflow de publication intégré pour la signature de l’APK fourni par Visual Studio. Dans [Préparation d’une application pour sa mise en production](~/android/deploy-test/release-prep/index.md), le **Gestionnaire d’archives** a été utilisé pour générer l’application et la placer dans une archive en vue de sa signature et de sa publication. Cette section explique comment créer une identité de signature Android et un nouveau certificat de signature pour les applications Android et comment publier l’application archivée *ad hoc* sur disque.
Le fichier APK ainsi créé peut être chargé de façon indépendante (sideloaded) sur les appareils Android sans passer par un App Store.

Dans [Archiver pour publication](~/android/deploy-test/release-prep/index.md#archive), la boîte de dialogue **Canal de distribution** proposait deux options pour la distribution. Sélectionnez **Ad-Hoc** :

[ ![Boîte de dialogue Canal de distribution](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Dans cette section, nous allons utiliser le workflow de publication intégré de Visual Studio pour Mac afin de signer l’APK. Dans [Préparation d’une application pour sa mise en production](~/android/deploy-test/release-prep/index.md), nous avons utilisé le **Gestionnaire d’archives** pour générer l’application et la placer dans une archive en vue de sa signature et de sa publication. Dans cette section, nous allons apprendre à créer une identité de signature Android, à créer un nouveau certificat de signature pour les applications Android et à publier l’application archivée *ad hoc* sur disque. Le fichier APK ainsi créé peut être chargé de façon indépendante (sideloaded) sur les appareils Android sans passer par un App Store.

Dans [Archiver pour publication](~/android/deploy-test/release-prep/index.md#archive), la boîte de dialogue **Signer et distribuer...** proposait deux options pour la distribution. Sélectionnez **Ad-Hoc** et cliquez sur **Suivant** :

[ ![Boîte de dialogue Signer et distribuer](images/xs/01-select-ad-hoc-sml.png)](images/xs/01-select-ad-hoc.png)

-----

<a name="newcertvs" />
<a name="newcert" />
<a name="newcertxs" />

## <a name="create-a-new-certificate"></a>Créer un nouveau certificat

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Une fois **Ad-Hoc** sélectionné, Visual Studio ouvre la page **Identité de signature** de la boîte de dialogue comme illustré dans la capture d’écran suivante. Avant de pouvoir être publié, le fichier .APK doit être signé avec une clé de signature (également appelée un certificat).

Un certificat existant peut être utilisé en cliquant sur le bouton **Importer**, puis en poursuivant par [Signer le fichier APK](#signapkvs). Sinon, cliquez sur le bouton **+** pour créer un nouveau certificat :

[ ![Identité de signature Ad Hoc](images/vs/02-ad-hoc-signing-identity-vs-sml.png)](images/vs/02-ad-hoc-signing-identity-vs.png)

La boîte de dialogue **Créer un magasin de clés Android** s’affiche. Utilisez cette boîte de dialogue pour créer un nouveau certificat de signature qui sera utilisé pour la signature des applications Android. Entrez les informations requises (indiquées en rouge), comme illustré dans cette boîte de dialogue :

[![Boîte de dialogue Créer un magasin de clés Android](images/vs/03-create-android-key-store-vs-sml.png)](images/vs/03-create-android-key-store-vs.png)

L’exemple suivant illustre le type d’informations qui doivent être fournies. Cliquez sur **Créer** pour créer le nouveau certificat :

[ ![Création d’un nouveau certificat](images/vs/04-key-store-example-vs-sml.png)](images/vs/04-key-store-example-vs.png)

Le magasin de clés ainsi créé se trouve à l’emplacement suivant :

**C:\\Utilisateurs\\*NOM_UTILISATEUR*\\AppData\\Local\\Xamarin\\Mono for Android\\alias\\alias.keystore**

Par exemple, la procédure ci-dessus crée une nouvelle clé de signature à l’emplacement suivant :

**C:\\Utilisateurs\\*NOM_UTILISATEUR*\\AppData\\Local\\Xamarin\\Mono for Android\\chimp\\chimp.keystore**

> [!NOTE]
> **Remarque :** Veillez à sauvegarder le fichier de magasin de clés créé dans un endroit sûr &ndash; il n’est pas inclus dans la solution. Si vous perdez votre fichier de magasin de clés (par exemple, suite à un changement d’ordinateur ou à la réinstallation de Windows), vous ne pourrez pas signer votre application avec le même certificat que les versions précédentes.

Pour plus d’informations sur le magasin de clés, consultez [Recherche de la signature MD5 ou SHA1 de votre magasin de clés](~/android/deploy-test/signing/keystore-signature.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Après avoir cliqué sur **Ad-Hoc**, Visual Studio pour Mac ouvre la boîte de dialogue **Identité de signature Android** comme illustré dans la capture d’écran suivante. Avant de pouvoir être publié, le fichier .APK doit être signé avec une clé de signature (également appelée un certificat). Si un certificat existe déjà, cliquez sur le bouton **Importer une clé** pour l’importer, puis poursuivez avec [Signer le fichier APK](#signapkxs). Sinon, cliquez sur le bouton **Créer une clé** pour créer un nouveau certificat : 

[ ![Boîte de dialogue Identité de signature Android](images/xs/02-android-signing-identity-sml.png)](images/xs/02-android-signing-identity.png)

La boîte de dialogue **Créer un autre certificat** est utilisée pour créer un nouveau certificat de signature qui peut être utilisé pour la signature des applications Android. Cliquez sur **OK** après avoir entré les informations nécessaires :

[ ![Boîte de dialogue Créer un autre certificat](images/xs/03-create-new-certificate-sml.png)](images/xs/03-create-new-certificate.png)

Le magasin de clés ainsi créé se trouve à l’emplacement suivant :

**~/Library/Developer/Xamarin/Keystore/alias/alias.keystore**

Par exemple, la procédure ci-dessus crée une nouvelle clé de signature à l’emplacement suivant :

**~/Library/Developer/Xamarin/Keystore/chimp/chimp.keystore**


> [!NOTE]
> **Remarque :** Veillez à sauvegarder le fichier de magasin de clés créé dans un endroit sûr &ndash; il n’est pas inclus dans la solution. Si vous perdez votre fichier de magasin de clés (par exemple, suite à un changement d’ordinateur ou à la réinstallation de Windows), vous ne pourrez pas signer votre application avec le même certificat que les versions précédentes.

Pour plus d’informations sur le magasin de clés, consultez [Recherche de la signature MD5 ou SHA1 de votre magasin de clés](~/android/deploy-test/signing/keystore-signature.md).

-----

<a name="signapkvs" />
<a name="signingxs" />

## <a name="sign-the-apk"></a>Signer l’APK

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Lorsque vous cliquez sur **Créer**, un nouveau magasin de clés (contenant un nouveau certificat) est enregistré et répertorié sous **Identité de signature** comme illustré dans la capture d’écran suivante. Pour publier une application sur Google Play, cliquez sur **Annuler** et accédez à la rubrique [Publication sur Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
Pour une publication *ad-hoc*, sélectionnez l’identité de signature à utiliser pour la signature, puis cliquez sur **Enregistrer sous** pour publier l’application en vue de sa distribution indépendante. Par exemple, l’identité de signature **chimp** (créée précédemment) est sélectionnée dans cette capture d’écran :

[![Identité de signature - Exemple](images/vs/05-save-as-vs-sml.png)](images/vs/05-save-as-vs.png)

Ensuite, le **Gestionnaire d’archives** affiche la progression de la publication. Une fois le processus de publication terminé, la boîte de dialogue **Enregistrer sous** s’ouvre. Vous devez indiquer l’emplacement de stockage du fichier .APK généré :

[ ![Boîte de dialogue Enregistrer sous](images/vs/06-save-as-dialog-vs-sml.png)](images/vs/06-save-as-dialog-vs.png)

Accédez à l’emplacement souhaité et cliquez sur **Enregistrer**. Si le mot de passe de la clé est inconnu, la boîte de dialogue **Mot de passe de signature** s’affiche et vous invite à entrer le mot de passe pour le certificat sélectionné :

[ ![Boîte de dialogue Mot de passe de signature](images/vs/07-signing-password-vs-sml.png)](images/vs/07-signing-password-vs.png)

Une fois le processus de signature terminé, cliquez sur **Ouvrir le dossier** :

[ ![Bouton Ouvrir le dossier](images/vs/08-open-folder-vs-sml.png)](images/vs/08-open-folder-vs.png)

L’Explorateur Windows ouvre alors le dossier contenant le fichier APK généré. À ce stade, Visual Studio a compilé l’application Xamarin.Android dans un APK qui est prêt pour la distribution.
La capture d’écran suivante affiche un exemple de l’application prête à être publier, **MyApp.MyApp.apk** :

[ ![APK dans l’Explorateur Windows](images/vs/09-generated-app-vs-sml.png)](images/vs/09-generated-app-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)


Comme nous le voyons ici, un nouveau certificat a été ajouté au magasin de clés. Pour publier une application sur Google Play, cliquez sur **Annuler** et accédez à la rubrique [Publication sur Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
Sinon, cliquez sur **Suivant** pour publier l’application *ad-hoc* (en vue de sa distribution indépendante) comme illustré dans cet exemple :

[ ![Boîte de dialogue Signer et distribuer](images/xs/04-select-identity-sml.png)](images/xs/04-select-identity.png)

La boîte de dialogue **Publier en tant qu’Ad Hoc** affiche un résumé de l’application signée avant sa publication. Si ces informations sont correctes, cliquez sur **Publier**.

[ ![Boîte de dialogue Publier en tant qu’Ad Hoc](images/xs/05-publish-ad-hoc-sml.png)](images/xs/05-publish-ad-hoc.png)

La boîte de dialogue **Fichier APK de sortie** enregistre l’APK dans le chemin spécifié. Cliquez sur **Enregistrer**.

![Boîte de dialogue Fichier APK de sortie](images/xs/06-output-apk-file.png)

Ensuite, entrez le mot de passe du certificat (celui qui a été utilisé dans la boîte de dialogue **Créer un autre certificat**) et cliquez sur **OK** : 

![Entrer le mot de passe du certificat](images/xs/07-signing-certificate.png)

L’APK est signé avec le certificat et enregistré à l’emplacement spécifié. Cliquez sur **Afficher dans le Finder** :

[![Boîte de dialogue Publication réussie](images/xs/08-app-is-ready-sml.png)](images/xs/08-app-is-ready.png)

Le Finder s’ouvre alors à l’emplacement du fichier APK signé :

[ ![APK affiché dans le Finder](images/xs/09-show-in-finder-sml.png)](images/xs/09-show-in-finder.png)

L’APK est prêt à être copié à partir du Finder et à être envoyé vers sa destination finale. Il est conseillé d’installer l’APK sur un appareil Android et de le tester avant sa distribution. Voir [Publication indépendante](~/android/deploy-test/publishing/publishing-independently.md) pour plus d’informations sur la publication d’un APK *ad-hoc*.

-----


<a name="nextsteps" />

## <a name="next-steps"></a>Étapes suivantes

Une fois le paquet d’application signé pour sa mise en production, il doit être publié. Les sections suivantes décrivent différentes façons de publier une application.