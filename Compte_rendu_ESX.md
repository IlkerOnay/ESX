# Comtpe rendu ESX ONAY Ilker 09/12/22




> Les IDRAC à notre disposition sont le 4 et le 8 leur adresse IP est respectivement 10.202.0.66 et 10.202.0.67 <br>
Les logins attribués sont `root` et les mots de passes `torototo`


<br><br>

> <ins>Les serveurs ESXi :</ins><br>
10.202.0.154 (le mien Idrac 4) & 10.202.0.82 (noa) <br>
Leur mots de passes est : iutiut34!

<br><br>

## <ins> Branchement du serveur </ins><br>
<br>
Dans un premier temps il faut brancher le serveur sur le switch 10G : <br> <br>

En vert, branchement internet à la salle depuis le switch 10G
<br>En rouge, branchement écran pc
<br>En bleu, branchement clavier/souris

<img src= "Screen\1.png">
<img src= "Screen\2.png">

<br><ins>Schéma :</ins><br> 

<img src= "Screen\3.png">
<br><br><br><br>

## <ins> 3.2 Installation des ESXi en "standalone"</ins><br>
<br>

>Pour l'installation de L'ESXi munissez vous d'un iso<br> 
http://store.iutbeziers.fr/vmware/esxi-702.iso
<br>
Puis booter le sur le serveur.

Une fois le boot réussis vous devez proccéder à l'installation : <br>
- Choisir votre disque dur

<img src="Screen\4.png">
<br><br>

- Si une installation existe `overwrite` permet de l'écrasser
<br><br><img src="Screen\5.png" ><br><br>

- Choisir la "langue" de votre clavier
<br><br><img src="Screen\6.png"><br>

- Choisir votre mot de passe ( Le mien est : `iutiut34!`)

> Une fois fini il nous vous reste plus que à modifier les paramètre réseaux dans mon cas les suivants était appliquer :
<br>Ip : 10.202.0.154/16
<br>Gateway : 10.202.255.254
<br>DNS : 10.255.255.200
<br>Biensur celle-ci dépend de votre configuration réseaux.

Ne pas oublier le mode promiscuité pour la suite de ce qu'on veut faire :
<br><br><img src="Screen\8.png"><br>


## <ins> 3.3 Installer un ESXi 7 sur un de vos serveurs physiques à partir d'une clef USB</ins><br>
<br>

L'installation est la même il suffit de reboot après avoir branchée la clé usb est appuyer sur F2 puis choisir de booter sur la clé USB


## <ins> 4. Installation du VMWareVcenter </ins><br>
Je n'ai pas de screen car aucun concorde car j'ai refait l'installation plusieur fois comme vous le savez très-bien ^^ 
> Lien pour l'ova : <br> http://store.iutbeziers.fr/vmware/vCenter7.0.3.ova
<br> L'installation est très complexe pour ce faire j'ai utiliser les paramètres suivants<br>
Une fois que j'ai cliquer sur crée une machine virtuelle est choisis mon fichier en OVA : 
<br><ins>Configuration :</ins>
<br>ipv4
<br>dhcp
<br>vcsa7.loc
<br>SSO password : VMware!S1
<br>Root password : rftgy#123
<br>Ne pas oublier de décocher lancement à la fin de l'installation !!!

<br><br>
<img src= Screen\10.png>

Par la suite a la fin de la création de la VM, avant de le lancer vous pouvez augementer depuis les paramètres de la VM le CPU à 8 et la RAM à 24 GO pour une meilleur rapidité cependant en abuser peut faire l'effet inverse est ralentir votre système.
<br><br>

<img src= Screen\11.png>
<br><br>

Une fois fini vous pouvez lancer votre VM, puis attendre la fin de l'installation du VCENTER.<br>
Une fois fini vous pouvez appuyer sur `F2` pour configurer le réseaux encore une fois de votre VM (Dans mon cas j'ai laisser le DHCP, car j'avais des problèmes lors de l'installation final).
<br>
<br>
C'est étape fini il vous reste la finalisation, pour celà rendez-vous dans le fichier `/etc/hosts` pour le modifier est rentrer `IP` + `vcsa7.loc`

<img src = Screen\12.png>

Une fois fini il faut se connecter depuis google en écrivant l'adresse IP (10.202.0.180) suivi de `:5480`

Vous vous retrouvez sur la page d'installation très simple il vous suffit juste de bien mettre un serveur ntp exemple time.google.com(pareil pour l'ESXi) et de laisser encore le dhcp  
<br>Le nom de domaine SSO que j'ai choisis est celui par défaut : vsphere.local
<br>Le nom d'utilisateur aussi
<br>Petit rappel mettre le même mots de passe pour le SSO que avant soit : VMware!S1   

<br>
<br>
<img src= Screen\13.png>
<br>
<br>Désactiver le CEIP !!
<br>Vous pouvez lancer l'installation final ^^.

<br><br><br>



## <ins> 4.2 Exploration de l’interface graphique du Vcenter </ins><br>

<br>

1.Créez un datacenter et rattachez-lui les hôtes ESXi.<br>
<img src = Screen\14.png>
<br><br>
2.Depuis la page d’accueil créez une bibliothèque de contenu.La création est rapide mais la bibliothèque
peut mettre du temps à apparaître dans la console du vcenter.<br>

<img src = Screen\15.png>
<br><br>
4.Stocker l’iso dans la bibliothèque de contenu que vous venez de créer.<br>
<img src =Screen\16.png>
<br><br>
5.Installez la distribution Linux Alpine depuis une iso. Créez une machine avec 512M de RAM et
un disque d’1 Go. Choisissez linux, other 3.x kernel 64 bit. Laissez vous guider par le setup afin
d’installer Alpine sur le disque de la VM:<br>
<img src =Screen\17.png><img src= Screen\18.png>
<br><br>

6.Transformez la VM Alpine en modèle.<br><img src=Screen\19.png><br><br>

7.Créez une machine Alpine depuis le modèle<br><img src=Screen\20.png><img src=Screen\21.png><br> <br>


8.Migrez à froid une Alpine d’un ESX à un autre. (il faut configurer Vmotion sur le port "VM Kernel".)<br><img src=Screen\22.png><img src=Screen\23.png><br><br>


9.Retrouvez le graphique du "Virtual Switch" ou sont connectées les VMs depuis le menu hôte.<br><img src=Screen\24.png><br><br>


10.Combien d’adaptateur réseaux sont visibles et à quoi correspondent-ils ?<br>`Un seul adapatateur réseaux, il est physique et correspond au port internet `<br><br>


11.Limitez la bande passante pour une VM.<br>Il faut crée un groupe de port, dans la deuxième image dans l'onglet Configurer les paramètres la vitesse bande passante est bridable (j'ai perdu le screen lors du rendu pdf il a été écrasser par une autre image, donc irrécupérable) <img src=Screen\25.png><img src=Screen\26.png><img src=Screen\27.png><br><br>


12.Quelle est l’unité de mesure du CPU dans Vsphere?<br> L'unité de mesure du CPU est en GHz (donc Hz)<br><img src=Screen\28.png><br><br>

13.Quelle est la consommation électrique (en watts) de votre Hôte? <br>La consommation en Watt max est de 122 et minimum de 120<br> <img src=Screen\29.png><br><br>


14.Quelles sont les latences de lecture et décriture de votre datastore?<br>Latence de lecture 0,028 ms (maximum 1 ms)<br>
Latence d’écriture 0ms<br><img src=Screen\30.png><br> <br>


15.Décrivez et expliquez les courbes liées à la mémoire.<br>
>Utilisation de 10.202.0.154 : La ram allouée à l’hôte<br>
La mémoire dite de Gonflage ( Ballonée), c’est le fait que VMTools fake une demande de RAM<br>
pour récupérer de la mémoire sur les serveurs en gonflant la demande de RAM pour libérer ceux dont l’utilisation peut être dit pas nécéssaires.<br>
La mémoire active, quant à elle, est la partie de la mémoire utilisée/consommée qui a été activement (d'où son nom) touchée par des opérations d'E/S au cours des 20 dernières secondes.<br>
La mémoire utilisée (plus communément appelée mémoire consommée) correspond à la quantité totale de mémoire utilisée sur l'ESXi.<br>
La mémoire partagé c’est la mémoire hôte qui assiste la mémoire physique d’invite <br>
La mémoire accordée est la quantité de mémoire physique hôte ou de mémoire physique mappée pour une vm ou un hôte<br>

<img src=Screen\31.png><br><br>


16.Permettez au Vm de se démarrer avec l’Hôte ESX<br><img src=Screen\32.png><br><br>


18.Ouvrez la console d’une VM Nostalgia et jouez mais pas plus de quelques minutes...<br><img src=Screen\33.png><br><br>
<br><br><br>

## **<ins>5 Utilisation de PowerCli pour piloter le datacenter VMware** </ins>
## <ins>5.1 Installation de PowerCli</ins>

<br><ins>1.Depuis une VM Ubuntu :</ins>

<img src= Screen\34.png><img src= Screen\35.png>
<br><br>

3.Choisissez de travailler sous Windows ou Linux.<br> Je choisis Linux<br><br>


4.Listez l’ensemble des commandlets PowerCLI disponibles via: `Get-VICommand`<br><img src=Screen\36.png><br><br>


5.Listez l’ensemble des commandlets PowerCLI disponibles pour les VM<br>`Get-Command -Module vm*` ou `get-help vm`<img src=Screen\37.png><br><br>


6.Obtenez de l’aide sur la commande Connect-Viserver via : `Get-help Connect-Viserver`<br><img src=Screen\38.png><br><br>


7.Obtenez via l’aide Powershell des exemples d’utilisation pour la commande de la question précédente.
<br>`Get-Help Connect-Viserver -Examples`<img src=Screen\39.png><br><br>


8.Listez le datacenter créé au départ du TP ?<br>` Get-Datacenter`<br><br>


9.Créez un nouveau Datacenter IUTBeziers dans le folder des Datacenters.<br>`$folder = Get-Folder -NoRecursion |New-Folder -Name IUT` + `New-Datacenter -Location $folder -Name IUTBeziers | fl`<br>
<img src=Screen\40.png><br><br>


10.Recherchez les dossiers (folders) du Vcenter<br>`Get-Folder  `<img src=Screen\41.png><br><br>


11.Créer un nouveau cluster MonClusterAsur localisé dans le datacenter IUTBéziers.<br>`New-Cluster -Name "MonClusterAsur` -Location "IUTBeziers"<img src=Screen\42.png><br><br>


12.Supprimez MonClusterAsur localisé dans le datacenter IUTBéziers<br>`Remove-Cluster -Cluster MonClusterAsur`<img src=Screen\43.png><br><br>


13.Listez les hôtes VMware.<br>`Get-VMHost`<img src=Screen\44.png><br><br>


14.Listez les datastores. A quoi sert un datastore ?<br>`Get-Datastore`<br>Le datastore permet de stocker les VM<img src=Screen\45.png><br><br>


15.Listez les VM présentes via PowerCli.<br>`Get-VM`<img src=Screen\46.png><br><br>


16.Listez les VM dont le nom commence par "alpine".<br>`Get-VM -NAME ALPINE*`<img src=Screen\47.png><br><br>


17.Listez les VM dont l’état est poweroff.<br>`Get-VM | grep PoweredOff`<img src=Screen\48.png><br><br>


18.Stoppez une VM Alpine, redémarrez-la sans demander de confirmation (suivre en même temps sur
le vi-client infrastructure le résultat de la commande ).<br>`Stop-VM "ALPINE MODELE" -Confirm:$false`<br>`Start-VM "ALPINE MODELE" -Confirm:$false`<br>`Restart-VM "ALPINE MODELE" -Confirm:$false`<br><img src=Screen\49.png><br><br>


19.Créez 3 nouvelles VM Alpine à partir du template.
<br>`New-VM -Name 'alpine1' -Template "ALPINE MODELE" -VMHost "10.202.0.154" -Datastore "datastore1"`<img src=Screen\50.png><br><br>


20.Faites un stop-start de ces VM en n’utilisant qu’une seule ligne de commande.<br>`Get-VM -Name Alpi* |Start-VM | Stop-VM -Confirm:$false`<img src=Screen\51.png><br><br>


21.Créer une nouvelle vm vmtest0x avec 10 Mb de disque et 256 M de mémoire en utilisant la commande
new-vm depuis powercli.<br>`New-VM -Name "vmtest0x" -MemoryMB 256 -DiskMB 10 -Datastore datastore1 -VMHost "10.202.0.154"`<img src=Screen\52.png><br><br>


22.Vérifiez la taille de disque occupée par la VM à l’aide la commande suivante:<br> `get-VM | select Name,@{ Name="DatastoreCapacityGB" ;Expression = { $_.UsedSpaceGB }}}`<br><img src=Screen\53.png><br><br>


23.Déterminez la mémoire utilisée par les VM.<br>`get-VM | select Name,@{ Name="MemoryGB" ;Expression = { $_.MemoryGB }}}`<br><img src=Screen\54.png><br><br>


24.Exportez une liste de VM et leurs propriétés vers un fichier csv : <br>
`get-VM | select Name, Description,PowerState, Num*, Memory*,@{Name="Host" ;
Expression={$_.Host.Name}}| export-csv output.csv`<br><img src=Screen\55.png><br><br>


25.A quoi sert la variable $_ ?<br>Permet de traiter les propriétées de l’objet<br><br>


26.Passez le nombre de vcpu à deux pour cette vm.<br>Permet d'afficher le nombre de cpu utilisées est autre informations :<br>`get-VM | select Name, Description,PowerState, Num*, Memory*,@{Name="Host" ;
Expression={$_.Host.Name}}
`
<br>Permet de changer le nombre de cpu :
<br>`get-VM -name alpine1 | set-VM -NumCpu 2`<br>Voir avant - après<br>

<img src=Screen\56.png><br><br>


27.Détruisez votre VM vmtest0x.<br>`remove-vm vmtest0x`<img src=Screen\57.png><br><br>


28.Lister les deux cartes réseaux physiques de l’hôte ESX.<br>`Get-VMHostNetworkAdapter -Physical`<img src=Screen\58.png><br><br>


29.Lister la liste des virtualswitch de l’ESX . A quoi sert un virtual switch ?<br>`Get-VirtualSwitch`<img src=Screen\59.png><br><br>

## <ins>5.2 Exploration de l’hôte ESXi via la ligne de commande (ESXcli) :</ins>

1.Activer sur l’ESXi le service SSH et le shell ESXi <br>
<img src=Screen\60.png><br><img src=Screen\61.png>