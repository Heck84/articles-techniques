Comment résoudre l’erreur « Password Writeback n’est pas activé pour votre organisation » dans Microsoft Entra ID

Lorsque vous utilisez Microsoft Entra ID avec un Active Directory local (on-premises), vous pouvez rencontrer l’erreur suivante lorsqu’un utilisateur tente de réinitialiser son mot de passe :
	
  This user's password can’t be reset because password writeback isn't turned on for your organization.
  
Cette erreur indique généralement que la fonctionnalité Password Writeback n’est pas activée dans Microsoft Entra Connect.

Constat du Problème:

Avec une synchronisation entre un Active Directory local et Microsoft Entra ID, les comptes utilisateurs sont normalement synchronisés de l’AD local vers Entra ID.
Lorsque Password Writeback est activé, le fonctionnement peut également se faire dans l’autre sens pour les mots de passe :
Microsoft Entra ID vers Active Directory local

Ainsi, lorsqu’un utilisateur réinitialise son mot de passe depuis Microsoft Entra ID, le nouveau mot de passe peut être réécrit dans l’Active Directory local.
Si Password Writeback n’est pas activé, la réinitialisation du mot de passe peut échouer avec le message présenté ci-dessus.

Mon cas : 

Active Directory local indisponible
Dans mon cas, la situation était différente.
Mon Active Directory local était down (avait craché, il fallait réfaire une nouvelle installation, backup corompu) et je ne pouvais donc plus accéder au serveur AD afin de modifier la configuration de Microsoft Entra Connect et d’activer Password Writeback.
La solution classique consistant à intervenir directement sur l’environnement local ce quin’était donc pas possible.
J’ai ensuite essayé de modifier la configuration directement depuis Microsoft Entra ID afin de passer l’organisation en mode Cloud-only.
Cependant, l’interface graphique ne me permettait pas d’effectuer cette modification : l’option Source d’identité n’était pas disponible.
Je me suis donc tourné vers Microsoft Graph PowerShell pour effectuer cette opération.

Solution avec Microsoft Graph PowerShell
L’objectif est de vérifier l’état actuel de la synchronisation, puis de désactiver la synchronisation d’annuaire afin que Microsoft Entra ID fonctionne en mode Cloud-only.

Etapes à suivre:


1. Ouvrir PowerShell en tant qu’administrateur
2. Installer Microsoft Graph PowerShell
Exécutez :
Install-Module Microsoft.Graph -Scope CurrentUser

Si PowerShell demande une confirmation concernant l’installation depuis PowerShell Gallery, confirmez l’installation avec Y ou O.
<img width="1060" height="173" alt="image" src="https://github.com/user-attachments/assets/724613bb-97a1-44f1-9426-0e0ac57ed569" />
4. Se connecter à Microsoft Graph
Connectez-vous à votre tenant Microsoft 365 avec un compte disposant des autorisations nécessaires (compte administrateur general) :
Connect-MgGraph -Scopes "Organization.ReadWrite.All"
Une fenêtre d’authentification Microsoft s’ouvre.
Connectez-vous avec votre compte administrateur.
<img width="971" height="356" alt="image" src="https://github.com/user-attachments/assets/8091cbf4-d390-4443-9aa2-fd41e2f69efa" />
5. Vérifier l’état actuel de la synchronisation
Avant de modifier quoi que ce soit, vérifiez l’état actuel de la synchronisation d’annuaire :
Get-MgOrganization | Select-Object DisplayName, OnPremisesSyncEnabled


indique que la synchronisation avec un Active Directory local est actuellement activée.
<img width="938" height="97" alt="image" src="https://github.com/user-attachments/assets/4280e7a2-a287-4f37-a21a-5a50d74715b5" />
5. Désactiver la synchronisation d’annuaire
l'objectif est de passer en Cloud-only.

Exécutez :

Get-MgOrganization | Select-Object Id, DisplayName, OnPremisesSyncEnabled
<img width="850" height="56" alt="image" src="https://github.com/user-attachments/assets/516f71ba-a153-4c2c-ab18-f5ca5670fd66" />

6. Vérifier la modification
Après l’opération, vérifiez à nouveau l’état :
Get-MgOrganization | Select-Object DisplayName, OnPremisesSyncEnabled
La valeur devrait maintenant être :
OnPremisesSyncEnabled
---------------------
False

Microsoft Entra ID considère alors l’organisation comme n’étant plus synchronisée avec un Active Directory local.

Tester la réinitialisation du mot de passe
Une fois la modification effectuée, testez à nouveau la réinitialisation du mot de passe avec l'utilisateur concerné.

L’objectif est de vérifier que l’erreur :

	This user's password can’t be reset because password writeback isn't turned on for your organization.
ne se présente plus.


Dans mon cas ça marché

Conclusion

Microsoft Graph m'a permis de résoudre ce problème.
Les commandes principales utilisées sont :
Connect-MgGraph -Scopes "Organization.ReadWrite.All"
Get-MgOrganization | Select-Object Id, DisplayName, OnPremisesSyncEnabled
Update-MgOrganization -OrganizationId "<TENANT_ID>" -OnPremisesSyncEnabled:$false

Cette méthode est particulièrement utile dans un scénario de dépannage où l'infrastructure Active Directory locale n'est plus synchronisé avec Entra ID.

