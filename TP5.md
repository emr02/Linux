# TP 5 - Systèmes de fichiers, partitions et disques

<h1>Exercice 1. Disques et partitions</h1>

<ol>

*1. Dans l’interface de configuration de votre VM, créez un second disque dur, de 5 Go dynamiquement
alloués ; puis démarrez la VM*


```bash
1. Ouvrez VirtualBox.
2. Sélectionnez la machine virtuelle que vous souhaitez configurer.
3. Cliquez sur le bouton Paramètres.
4. Dans l'onglet Stockage, cliquez sur le bouton Créer un nouveau disque dur.
5. Sélectionnez le type de disque dur VDI (VirtualBox Disk Image).
6. Sélectionnez le mode de création Dynamiquement alloué.
7. Dans le champ Taille, saisissez 5.
8. Cliquez sur le bouton Créer.

```

*2. Vérifiez que ce nouveau disque dur est bien détecté par le système*


```bash
fdisk -l
```

```bash
Disk /dev/sdb: 5 GiB, 5368709120 bytes, 10485760 sectors
Disk model: Virtual disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

*3. Partitionnez ce disque en utilisant fdisk : créez une première partition de 2 Go de type Linux (n°83), et une seconde partition de 3 Go en NTFS (n°7)*

*Pour faire ceci, il faut utiliser la commande : fdisk /dev/"nom du disque" par exemple fdisk
/dev/sdb Puis on suit les commandes et il ne faut pas oublier de faire w afin de sauvegarder les modifications
Pour créer la partitions en NTFS ça se passe ainsi :*

```bash
fdisk /dev/sdb
Commands:
to create the partition: n, p, [enter], [enter]
to give a type to the partition: t, 7 (don't select 86 or 87, those are for volume sets)
if you want to make it bootable: a
to see the changes: p
to write the changes: w
mkfs.ntfs -f /dev/sdb2
mkdir /mnt/ntfsvolume
mount /dev/sdb2 /mnt/ntfsvolume
```

*4. A ce stade, les partitions ont été créées, mais elles n’ont pas été formatées avec leur système de fichiers. A l’aide de la commande mkfs, formatez vos deux partitions (pensez à consulter le manuel !)*

*La partition en NTFS a été formatée à la question précédente, pour ce qui est de celle qui est de type Linux il faut la faire de la manière suivante :
`mkfs.ext4 /dev/sdb1`*

*5. Pourquoi la commande df -T, qui affiche le type de système de fichier des partitions, ne fonctionne-telle pas sur notre disque ?*

*La commande df -T ne fonctionne pas car nos partitions ne sont pas montées. Il faut absolument avoir un système de fichiers et que la partition / disque soit monté pour que cela fonctionne.*

*6. Faites en sorte que les deux partitions créées soient montées automatiquement au démarrage de la machine, respectivement dans les points de montage /data et /win (vous pourrez vous passer des UUID en raison de l’impossibilité d’effectuer des copier-coller)*

*Les commandes pour que les deux partitions soient montés sont :*

```bash
mkdir /data
[09:52]-[root]@client-/home/julien: mkdir /win
[09:52]-[root]@client-/home/julien: umount /dev/sdb2
[09:52]-[root]@client-/home/julien: mount /dev/sdb2 /win
[09:52]-[root]@client-/home/julien: mount /dev/sdb1 /data
```

*Pour que les fichiers soient montés automatiquement au démarrage : il faut modifier le fichier `/etc/fstab`*

*7. Utilisez la commande mount puis redémarrez votre VM pour valider la configuration*

*Les disques sont correctement montés :
Résultat de la commande : `df -T ` :*

```bash
Filesystem Type 1K-blocks Used Available Use% Mounted on
udev devtmpfs 202128 0 202128 0% /dev
tmpfs tmpfs 48852 1116 47736 3% /run
/dev/sda2 ext4 16445308 5953616 9636604 39% /
tmpfs tmpfs 244244 0 244244 0% /dev/shm
tmpfs tmpfs 5120 0 5120 0% /run/lock
tmpfs tmpfs 244244 0 244244 0% /sys/fs/cgroup
/dev/loop0 squashfs 96256 96256 0 100% /snap/core/9066
/dev/loop1 squashfs 69376 69376 0 100% /snap/lxd/14503
/dev/loop2 squashfs 56320 56320 0 100% /snap/core18/1754
/dev/loop3 squashfs 70912 70912 0 100% /snap/lxd/14954
/dev/loop4 squashfs 96128 96128 0 100% /snap/core/8935
/dev/sdb1 ext4 1998672 6144 1871288 1% /data
/dev/sdb2 fuseblk 3144700 16264 3128436 1% /win
tmpfs tmpfs 48848 0 48848 0% /run/user/1000
```

*8. Créez un dossier partagé entre votre VM et votre système hôte (rem. il peut être nécessaire d’installer les Additions invité de VirtualBox).*

*9. Optionnel : Montez une clé USB dans la VM*

</ol>

<h1>Exercice 2. Partitionnement LVM</h1>

<ol>
  
<li><h3>Dans cet exercice, nous allons aborder le partitionnement LVM, beaucoup plus flexible pour manipuler les disques et les partitions.<h3></li

1. On va réutiliser le disque de 5 Gio de l’exercice précédent. Commencez par démonter les systèmes de fichiers montés dans /data et /win s’ils sont encore montés, et supprimez les lignes correspondantes du fichier /etc/fstab

*commande pour démonter les disques : `umount /dev/sdb1`
Suppression des lignes effectuées*

2. Supprimez les deux partitions du disque, et créez une partition unique de type LVM
`La création d’une partition LVM n’est pas indispensable, mais vivement recommandée quand
on utilise LVM sur un disque entier. En effet, elle permet d’indiquer à d’autres OS ou logiciels de gestion de disques (qui ne reconnaissent pas forcément le format LVM) qu’il y a des données sur ce disque.`
`Attention à ne pas supprimer la partition système !`

*Pour mettre en place le LVM : *

```bash
appuyer sur n pour créé une nouvelle partition de disque,
appuyer sur p pour créer une partition primaire,
appuyer sur 1 pour denote it as 1st disk partition,
appuyer sur ENTER deux fois,
appuyer sur t pour changer le type de partition par défaut Linux (0x83) en LVM partition
(0x8e),
appuyer sur L pour lister toutes les types de partitions supportés,
appuyer sur 8e pour changer la partition de 1 à 8e, i.e. Linux LVM partition type,
appuyer sur p pour afficher le groupe du 2ème disque.
appuyer sur w pour écrire les partitions dans la table.
```

4. A l’aide de la commande pvcreate, créez un volume physique LVM. Validez qu’il est bien créé, en utilisant la commande pvdisplay.
`Toutes les commandes concernant les volumes physiques commencent par pv. Celles concernant
les groupes de volumes commencent par vg, et celles concernant les volumes logiques par lv.`

*Création à l'aide de pvcreate :*

```bash
[10:11]-[root]@client-/home/julien: pvcreate /dev/sdb1
Physical volume "/dev/sdb1" successfully created.
[10:12]-[root]@client-/home/julien: pvdisplay
"/dev/sdb1" is a new physical volume of "<5,00 GiB"
--- NEW Physical volume ---
PV Name /dev/sdb1
VG Name
PV Size <5,00 GiB
Allocatable NO
PE Size 0
Total PE 0
Free PE 0
Allocated PE 0
PV UUID S47WxV-IN8K-q1JF-wH9E-qRsW-Dk1j-BiKVfJ
```

5. A l’aide de la commande vgcreate, créez un groupe de volumes, qui pour l’instant ne contiendra que le volume physique créé à l’étape précédente. Vérifiez à l’aide de la commande vgdisplay.
`Par convention, on nomme généralement les groupes de volumes vgxx (où xx représente l’indice
du groupe de volume, en commençant par 00, puis 01...)`

*Voici ce que l'on à fait ainsi que les résultats des commandes `vgcreate et vgdisplay` :*

```bash
[10:12]-[root]@client-/home/julien: vgcreate vg00 /dev/sdb1
Volume group "vg00" successfully created
[10:14]-[root]@client-/home/julien: vgdisplay
--- Volume group ---
VG Name vg00
System ID
Format lvm2
Metadata Areas 1
Metadata Sequence No 1
VG Access read/write
VG Status resizable
MAX LV 0
Cur LV 0
Open LV 0
Max PV 0
Cur PV 1
Act PV 1
VG Size <5,00 GiB
PE Size 4,00 MiB
Total PE 1279
Alloc PE / Size 0 / 0
Free PE / Size 1279 / <5,00 GiB
VG UUID FOpN73-4aQz-4xxJ-e0Cj-KVce-k0Jv-AlzS1q
```

6. Créez un volume logique appelé lvData occupant l’intégralité de l’espace disque disponible.
`On peut renseigner la taille d’un volume logique soit de manière absolue avec l’option -L (par exemple -L 10G pour créer un volume de 10 Gio), soit de manière relative avec l’option -l : -l 60%VG pour utiliser 60% de l’espace total du groupe de volumes, ou encore -l 100%FREE pour utiliser la totalité de l’espace libre.`

*Création du volume :*

```bash
lvcreate -l 100%FREE vg00
Logical volume "lvol0" created.
lvrename vg00 lvol0 lvdata
Renamed "lvol0" to "lvdata" in volume group "vg00"
```


6. Dans ce volume logique, créez une partition que vous formaterez en ext4, puis procédez comme dans l’exercice 1 pour qu’elle soit montée automatiquement, au démarrage de la machine, dans /data.
`A ce stade, l’utilité de LVM peut paraître limitée. Il trouve tout son intérêt quand on veut par exemple agrandir une partition à l’aide d’un nouveau disque.`

7. Eteignez la VM pour ajouter un second disque (peu importe la taille pour cet exercice). Redémarrez la VM, vérifiez que le disque est bien présent. Puis, répétez les questions 2 et 3 sur ce nouveau disque.

*Nous avons rajouté un disque de 3 Go, et il est bien visible*

8. Utilisez la commande `vgextend <nom_vg> <nom_pv>` pour ajouter le nouveau disque au groupe de volumes

*commande entrée*
```bash
vgextend vg00 /dev/sdc
```

9. Utilisez la commande `lvresize (ou lvextend)` pour agrandir le volume logique. Enfin, il ne faut pas oublier de redimensionner le système de fichiers à l’aide de la commande resize2fs.
`Il est possible d’aller beaucoup plus loin avec LVM, par exemple en créant des volumes par
bandes (l’équivalent du RAID 0) ou du mirroring (RAID 1). Le but de cet exercice n’était que de présenter les fonctionnalités de base.`

*Il faut entrer les commandes dans l'ordre suivant :*

```bash
lvextend -L +2.9G /dev/mapper/vg00-lvdata
resize2fs /dev/mapper/vg00-lvdata
[10:43]-[root]@client-/home/julien: lvdisplay
--- Logical volume ---
LV Path /dev/vg00/lvdata
LV Name lvdata
VG Name vg00
LV UUID vfvNyC-gr9n-VoxH-uqkg-lpC0-zFm0-4r45f4
LV Write Access read/write
LV Creation host, time client, 2020-05-06 10:16:59 +0200
LV Status available
# open 1
LV Size <7,90 GiB
Current LE 2022
Segments 2
Allocation inherit
Read ahead sectors auto
- currently set to 256
Block device 253:0
```

</ol>



