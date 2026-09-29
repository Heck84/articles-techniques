Dans les environnements professionnels basés sur Microsoft Active Directory, la gestion des accès utilisateurs constitue un enjeu majeur de cybersécurité.

Les comptes administrateurs et les comptes disposant de privilèges élevés représentent notamment des cibles importantes en cas de compromission. Une authentification basée uniquement sur un mot de passe ne constitue plus une protection suffisante face aux attaques par hameçonnage, vol d'identifiants, réutilisation de mots de passe ou compromission de postes.

Dans ce contexte, UserLock est une solution permettant de renforcer le contrôle des connexions aux environnements Windows et Active Directory, notamment grâce à la gestion des sessions et à l'authentification multifacteur (MFA).


1. Qu'est-ce que UserLock ?
UserLock est une solution de sécurité destinée aux environnements Windows et Active Directory.

Elle permet notamment aux équipes IT et sécurité de :

• contrôler les connexions des utilisateurs ;

• surveiller les sessions ouvertes ;

• appliquer des politiques d'accès ;

• limiter les connexions simultanées ;

• renforcer l'accès aux serveurs ;

• mettre en place une authentification multifacteur ;

• contrôler les connexions RDP ;

• détecter certaines situations anormales liées aux sessions ;

• centraliser la visibilité sur les connexions utilisateurs.

L'objectif est de disposer d'un contrôle supplémentaire entre l'identité Active Directory et l'accès effectif aux ressources Windows.

2. Pourquoi renforcer Active Directory ?
   
Active Directory constitue généralement un composant central du système d'information.

Une compromission d'un compte privilégié peut permettre à un attaquant d'accéder à plusieurs ressources et favoriser une propagation latérale dans le réseau.

Les comptes administrateurs doivent donc bénéficier de contrôles supplémentaires, particulièrement lorsqu'ils permettent l'accès à distance aux serveurs.

3. UserLock et le MFA

L'une des fonctionnalités intéressantes de UserLock est la possibilité de renforcer certaines authentifications avec un MFA (Multi-Factor Authentication).

Le principe consiste à ajouter un facteur supplémentaire au mot de passe. Même si le mot de passe est compromis, l'attaquant doit également disposer du second facteur pour obtenir l'accès protégé.

4. Protection des connexions RDP

Le protocole Remote Desktop Protocol (RDP) est largement utilisé par les administrateurs pour administrer les serveurs Windows.

Cependant, une interface RDP exposée ou insuffisamment protégée peut constituer une surface d'attaque importante.

Une stratégie possible consiste à imposer un MFA sur les connexions RDP afin de renforcer particulièrement les comptes administrateurs.


5. Attention aux comptes de service

Un point important concerne les comptes utilisés par des applications ou des services.

L'activation d'un MFA interactif sur un compte utilisé par une application peut provoquer une interruption de service si l'application ne sait pas gérer cette authentification.

Il est donc important de distinguer les comptes utilisateurs, administrateurs, de service, techniques et les comptes d'urgence.

Avant d'appliquer une politique MFA, il faut identifier précisément les usages de chaque compte.

6. Demo

Etape à suivre

Installer et configurer UserLock Server (source de téléchargement : https://www.isdecisions.com/en/userlock)

Installer le UserLock Agent sur les postes/serveurs concernés

Configurer les méthodes MFA (Authenticator, Push, etc.)

Créer un groupe AD de test

Créer une stratégie MFA dans UserLock

Associer le groupe AD à la stratégie

Enrôler les utilisateurs dans la MFA

Tester la connexion Windows

Tester les connexions RDP

Vérifier les journaux et événements MFA
   
Captures

-Activation de la méthode MFA
<img width="1468" height="626" alt="image" src="https://github.com/user-attachments/assets/c6fbead6-240b-473b-894f-b9881863e509" />

-Enrôlement des utilisateurs
<img width="1624" height="535" alt="image" src="https://github.com/user-attachments/assets/49a1fd6c-e01a-4240-b1dc-7910980b5d6c" />
-Génération du QR Code

-Validation du code OTP

-Génération des codes de récupération

-Authentification Windows protégée par MFA









