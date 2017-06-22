**Signer son code PowerShell**

Que veut dire signer un script PowerShell ?  Certainement pas comme certains peuvent le croire pour encrypter le fichier. Cela c’est confondre ce qui arrive lorsque l’on envoie des données à travers une connexion TLS, comme lorsque l’on regarde son compte en banque sur Internet. Signer un script, un module ou en logiciel c’est indiquer et prouver son identité. Pour reprendre l’analogie avec une connexion web avec un site bancaire, dans la barre d’adresse je peux voir le nom de ma banque à côté d’un petit cadenas. 

Cela est possible car un tiers de confiance, l’entité d’où est issus le certificat, à valider l’identité du propriétaire du site. 

Pour ce qui de nos scripts PowerShell. Signer équivaut à avoir un tiers de confiance qui valide l’identité de l’auteur. Ainsi le script peut être exécuter sans se poser de question. 

Ainsi un certificat auto signé, n’est pas une solution adaptée. Il n’y pas de tiers de confiance permettant la vérification de l’identité. 

Un tiers de confiance peut être soit une autorité externe, autrement dit un certificat commercial soit une autorité interne accessible par tous les serveurs et postes de travail. 

Si vous avez une autorité de certification basée sur Microsoft. La procédure pour permettre la signature est assez simple. Il faut avoir un accès au à l’autorité de certification de l’entreprise. 

Faire un clic droit sur "Certificate Template"

![Console de l'autorité de certification](https://raw.githubusercontent.com/omiossec/powershell-french/master/powershellCodeSigning/img/image001.png)

Cliquer sur "Manage" pour ouvrir le gestionnaire de template


![gestionnaire de template](https://raw.githubusercontent.com/omiossec/powershell-french/master/powershellCodeSigning/img/image002.jpg)

Faire un clic droit sur "Code Signing" puis "Duplicate template".
Dans la nouvelle fenêtre sélectionnez l’onglet General 

![Onglet Général](https://raw.githubusercontent.com/omiossec/powershell-french/master/powershellCodeSigning/img/image003.png)

Entrer un nom, sélectionner une période de validé (1 an par défaut).

Dans "Security", on peut permettre aux utilisateurs (ou un groupe d’utilisateurs) de s’inscrire

![Onglet sécurité](https://raw.githubusercontent.com/omiossec/powershell-french/master/powershellCodeSigning/img/image004.jpg)

D’autre possibilité sont offertes comme le changement dans la taille de la clé ou l’approbation par un tiers. 


Fermer "Certificat Template console". 

De retour dans la console de l’autorité de certification, faire un clic droit sur "Certificate Template" et sélectionner New puis "Certificate Template to Issue".

Sélectionner le template et cliquer sur OK 

![Enable Certificate Template](https://raw.githubusercontent.com/omiossec/powershell-french/master/powershellCodeSigning/img/image005.png)

Le template est disponible pour les utilisateurs. 

Pour obtenir un certificat il suffit de se placer dans le magasin en PowerShell. 

```powershell
# Les certificats étant délivré à des utilisateurs il faut se rendre en PS dans la pseudi dire cert:\currentuser\my

Set-Location -Path cert:\CurrentUser\My
Get-Certificate -Template PowerShellSign

# On peut lister les certificats 
dir

# On peut aussi sélectionner le certificat en utilisant son thumbprint 
# et stocker un pointeur vers ce certificat dans une variable
$cert =  dir Cert:\CurrentUser\My\7DEBE3605DE375F6F1051C9D6B39C4A76FBD349D

Set-Location -Path c:\

Set-AuthenticodeSignature -Certificate $cert -FilePath C:\scripts\monScript.ps1
```

La signature est ajouter à la fin en commentaire. En faisant un clic droit sur le script, un onglet signature est présent. 





```