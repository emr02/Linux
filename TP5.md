# TP 5 - Systèmes de fichiers, partitions et disques

<h1>Exercice 1. Disques et partitions</h1>

<ol>

*1. Dans l’interface de configuration de votre VM, créez un second disque dur, de 5 Go dynamiquement
alloués ; puis démarrez la VM*

*2. Vérifiez que ce nouveau disque dur est bien détecté par le système*

*3. Partitionnez ce disque en utilisant fdisk : créez une première partition de 2 Go de type Linux (n°83),
et une seconde partition de 3 Go en NTFS (n°7)*

*4. A ce stade, les partitions ont été créées, mais elles n’ont pas été formatées avec leur système de fichiers.
A l’aide de la commande mkfs, formatez vos deux partitions ( pensez à consulter le manuel !)*

*. Pourquoi la commande df -T, qui affiche le type de système de fichier des partitions, ne fonctionne-telle
pas sur notre disque ?*

*6. Faites en sorte que les deux partitions créées soient montées automatiquement au démarrage de la
machine, respectivement dans les points de montage /data et /win (vous pourrez vous passer des
UUID en raison de l’impossibilité d’effectuer des copier-coller)*

*7. Utilisez la commande mount puis redémarrez votre VM pour valider la configuration*

*8. Créez un dossier partagé entre votre VM et votre système hôte (rem. il peut être nécessaire d’installer
les Additions invité de VirtualBox).*

*9. Optionnel : Montez une clé USB dans la VM*

*En*

```bash
```

</ol>

<h1>Exercice 2. Partitionnement LVM</h1>

<ol>
  
<li><h3>Dans cet exercice, nous allons aborder le partitionnement LVM, beaucoup plus flexible pour manipuler
les disques et les partitions.<h3></li

*En*

```bash
```

</ol>



