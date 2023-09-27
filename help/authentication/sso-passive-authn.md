---
title: SSO via l’authentification passive
description: SSO via l’authentification passive
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 0%

---

# SSO via l’authentification passive

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.


## Introduction

Ce document décrit la mise en oeuvre du flux d’authentification passive et son fonctionnement avec notre approche standard de l’authentification unique.

## Usectes

L’authentification Adobe Pass active l’authentification unique (SSO) entre les applications et les sites. Une fois qu’un utilisateur se connecte avec ses informations d’identification MVPD, l’authentification Adobe Pass génère un jeton sécurisé qui représente la session d’authentification du MVPD et lie ce jeton à l’appareil de l’utilisateur à l’aide d’un identifiant de périphérique. L’authentification Adobe Pass stocke le jeton/ID de périphérique sur un serveur ou sur l’appareil.

Tant que le jeton est toujours valide, les utilisateurs s’affichent directement comme étant authentifiés. Cela permet aux utilisateurs de saisir leurs informations d’identification moins fréquemment tout en préservant la sécurité des transactions.



Le cas d’utilisation commerciale présenté ici est une exigence très spécifique : l’utilisateur DOIT être authentifié au moins une fois pour chaque site visité. Cela permet au MVPD d’appliquer des règles de fonctionnement associées à la session authN qui peuvent varier selon le réseau. Elle entre en conflit avec la promesse TVE actuelle qu’un utilisateur ne doit se connecter qu’une seule fois et sera authentifié sur tous les sites qui font partie de l’écosystème d’authentification Adobe Pass.



Pour conserver la règle de fonctionnement, mais aussi une bonne expérience utilisateur, le MVPD n’exige PAS qu’un utilisateur fournisse manuellement des informations d’identification. Nous pouvons tirer parti du cookie de session précédemment défini et essayer d’effectuer une réauthentification automatique à l’aide du flux passif ; du point de vue de l’utilisateur, il apparaîtra comme étant connecté automatiquement.



Pour résoudre ces problèmes, nous avons mis en place 2 fonctionnalités distinctes : authentification par réseau et prise en charge de l’authentification passive. Les MVPD prennent en charge l’authN passif SAML lorsqu’ils se contentent de réauthentifier un utilisateur s’il existe une session authN sur l’IdP, quel que soit le site sur lequel cette session a été créée.



## Authentification par réseau

Cette fonctionnalité garantit que le MVPD reçoit une demande d’authentification une fois pour chaque site visité. Cette fonctionnalité signifie qu’un jeton d’authentification d’authentification Adobe Pass sera limité au requestorID, ce qui sera valide uniquement pour le réseau qui l’a demandé. Par conséquent, une fois que l’utilisateur s’authentifie sur le site &quot;A&quot; et qu’il consulte ensuite le site &quot;B&quot;, il est nécessaire de s’authentifier.



Notez qu’en raison du fait que sur le MVPD IdP l’utilisateur est déjà authentifié, il n’est pas nécessaire de fournir des informations de connexion, mais le navigateur est simplement redirigé du site &quot;B&quot; vers MVPD IdP, puis de nouveau. Le même utilisateur sera toujours authentifié sur le site &quot;A&quot; s’il revient.



Le flux suivant illustre la fonctionnalité d’authentification de base par réseau :





## Authentification passive

L’objectif est de faire en sorte que le processus de réauthentification se produise en arrière-plan sans qu’il faille rediriger entièrement le navigateur et que le sélecteur s’affiche. Par conséquent, un utilisateur qui passe du site A au site B est automatiquement authentifié.



Du point de vue de l’expérience utilisateur, il n’y a aucune différence entre ce flux et un flux exécuté avec un MVPD normal. Ce que l’utilisateur voit, c’est qu’après avoir entré les informations d’identification suite à sa visite sur le site A, il sera automatiquement authentifié sur le site B.



Pour que ce flux soit viable, le MVPD doit prendre en charge l’authentification passive de sorte que l’iframe masqué ne soit pas &quot;bloqué&quot; sur la page de connexion au cas où le MVPD n’ait plus de session. Pour ce faire, utilisez l’attribut &quot;isPassive&quot; standard.



Le diagramme suivant illustre le flux amélioré et l’authentification passive &quot;en coulisses&quot; :





Exemple de requête SAML Voici un exemple de requête SAML pour le flux authN passif :


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

## Règles de fonctionnement

Les MVPD ont des restrictions de domaine de portée d’authentification unique spécifiques. Par exemple, seuls certains domaines peuvent être autorisés par certains MVPD (SSO pour la même société de médias, mais pas entre plusieurs entreprises).
Certains MVPD peuvent nécessiter l’application de règles d’authentification différentes. Par exemple, les MVPD peuvent avoir des TTL d’authentification différents par réseau. En outre, les MVPD peuvent activer l’authentification à domicile pour certains réseaux, mais pas pour d’autres (les cas d’utilisation du contrôle parental sont fortement représentés ici).


Ces exigences commerciales doivent garder à l’esprit que le cas d’utilisation principal est que l’utilisateur ne doit pas être tenu de se reconnecter après s’être connecté avec son MVPD.

Pour ce faire, utilisez l’authentification par réseau avec l’indicateur authN passif.



## Limites connues

iOS : en raison de la nature du stockage local dans iOS, les flux d’authentification unique ne fonctionnent pas sur iOS pour les applications développées par différents fournisseurs. Pour plus d’informations sur la connexion unique dans iOS 8 et versions ultérieures, reportez-vous à cette note technique.


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
