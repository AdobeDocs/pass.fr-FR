---
title: SSO via l’authentification passive
description: SSO via l’authentification passive
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 0%

---

# Authentification unique (héritée) via l’authentification passive

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Introduction

Le but de ce document est de décrire la mise en œuvre du flux d’authentification passive et son fonctionnement avec notre approche d’authentification unique standard.

## Cas d’utilisation

L’authentification Adobe Pass active l’authentification unique (SSO) entre les applications/sites. Une fois qu’un utilisateur se connecte avec ses informations d’identification MVPD, l’authentification Adobe Pass génère un jeton sécurisé qui représente la session d’authentification MVPD et lie ce jeton à l’appareil de l’utilisateur à l’aide d’un identifiant d’appareil. L’authentification Adobe Pass stocke le jeton/l’ID d’appareil sur un serveur ou sur l’appareil.

Tant que le jeton est toujours valide, les utilisateurs apparaissent directement comme authentifiés. Cela permet aux utilisateurs de saisir leurs informations d’identification moins fréquemment tout en maintenant la sécurité des transactions.



Le cas d’utilisation commercial détaillé ici est une exigence très spécifique : l’utilisateur DOIT être authentifié au moins une fois pour chaque site visité. Cela permet au MVPD d’appliquer des règles métier associées à la session d’authentification qui peuvent varier en fonction du réseau. Cela est en conflit avec la promesse TVE actuelle selon laquelle un utilisateur ne doit se connecter qu’une seule fois et sera authentifié sur tous les sites qui font partie de l’écosystème d’authentification Adobe Pass.



Afin de conserver la règle métier, mais aussi une bonne expérience utilisateur, le MVPD n’exige PAS qu’un utilisateur fournisse manuellement les informations d’identification. Nous pouvons bénéficier du cookie de session précédemment défini et tenter d’effectuer une réauthentification automatique à l’aide du flux passif ; du point de vue de l’utilisateur, il apparaîtra comme étant connecté automatiquement.



Pour résoudre ces problèmes, nous avons mis en œuvre 2 fonctionnalités distinctes : l’authentification par réseau et la prise en charge de l’authentification passive. Les fichiers MVPD prennent en charge l’authentification passive SAML lorsqu’ils authentifient simplement à nouveau un utilisateur ou une utilisatrice s’il existe une session d’authentification sur l’IdP, quel que soit le site sur lequel cette session a été créée.



## Authentification par réseau

Cette fonctionnalité permet de s’assurer que le MVPD reçoit une demande d’authentification une fois pour chaque site visité. Cette fonctionnalité signifie qu’un jeton d’authentification d’Adobe Pass sera lié à l’ID de demandeur, n’étant ainsi valide que pour le réseau qui l’a demandé. Par conséquent, une fois que l’utilisateur s’authentifie sur le site « A » et qu’il visite ensuite le site « B », il doit s’authentifier.



Notez qu’en raison du fait que l’utilisateur est déjà authentifié sur l’IdP MVPD, il n’est pas tenu de fournir des informations de connexion, mais le navigateur est simplement redirigé du site « B » vers l’IdP MVPD, puis de retour. Le même utilisateur sera toujours authentifié sur le site « A » s’il revient.



Le flux suivant illustre la fonctionnalité d’authentification de base par réseau :





## Authentification passive

L’objectif est de rendre le processus de réauthentification possible en arrière-plan sans avoir à effectuer une redirection complète du navigateur et sans afficher le sélecteur. Par conséquent, un utilisateur qui passe du site A au site B sera automatiquement authentifié.



Du point de vue de l’expérience utilisateur, il n’y aura aucune différence entre ce flux et un flux exécuté avec un MVPD standard. L’utilisateur voit qu’après avoir saisi les informations d’identification suite à sa visite du site A, il sera automatiquement authentifié sur le site B.



Pour rendre ce flux viable, le MVPD doit prendre en charge l’authentification passive afin que l’iframe masqué ne soit pas « bloqué » sur la page de connexion au cas où le MVPD n’aurait plus de session. Cela s’effectue via l’attribut standard « isPassive ».



Le diagramme suivant illustre le flux amélioré et l’authentification passive « en coulisses » :





Exemple de requête SAML
Voici un exemple de requête SAML pour le flux d’authentification passif :


```xml
<saml2p:AuthnRequest xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol"
                     AssertionConsumerServiceURL="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                     Destination="https://mvpd_idp_url"
                     ForceAuthn="false"
                     ID="_15056686-399c-4528-b21a-4a9542cfc8ec"
                     IsPassive="true"
                     IssueInstant="2014-11-03T14:18:12.394Z"
                     ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
                     Version="2.0"
                     >
    <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth.adobe.com </saml2:Issuer>
    <saml2p:Extensions>
        <thrpty:RespondTo xmlns:thrpty="urn:oasis:names:tc:SAML:protocol:ext:third-party">https://saml.sp.auth.adobe.com</thrpty:RespondTo>
    </saml2p:Extensions>
    <saml2p:NameIDPolicy AllowCreate="true"
                         Format="urn:oasis:names:tc:SAML:2.0:nameid-format:transient"
                         SPNameQualifier="https://saml.sp.auth.adobe.com"
                         />
</saml2p:AuthnRequest>
```

## Règles commerciales

Les fichiers MVPD comportent des restrictions de domaine de définition de la portée SSO spécifiques. Par exemple, seuls certains domaines peuvent être autorisés par certains MVPD (SSO pour la même société de médias, mais pas entre les sociétés).
Certaines MVPD peuvent nécessiter l’application de règles d’authentification différentes. Par exemple, les MVPD peuvent avoir des TTL d’authentification différentes pour différents réseaux. En outre, les MVPD peuvent permettre l’authentification à domicile pour certains réseaux, mais pas pour d’autres (les cas d’utilisation du contrôle parental sont fortement représentés ici).


Ces exigences commerciales doivent garder à l’esprit que le cas d’utilisation principal est que l’utilisateur ne devrait pas avoir à se reconnecter après s’être connecté avec son MVPD.

Pour ce faire, utilisez l’authentification par réseau avec l’indicateur authN passif.



## Limites connues

iOS - En raison de la nature du stockage local d’iOS, les flux de connexion unique ne fonctionnent pas sur iOS pour les applications développées par différents fournisseurs. Pour plus d’informations sur l’authentification unique dans iOS 8 et les versions ultérieures, reportez-vous à cette note technique.


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
