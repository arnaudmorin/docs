---
title: Object Storage Swift - Synchroniser un NAS Synology avec l’Object Storage
slug: pcs/pcs-syno
excerpt: Retrouvez ici comment synchroniser un NAS Synology a un conteneur.
section: Spécificités de la classe de stockage OpenStack Swift
order: 150
updated: 2023-05-22
---

**Dernière mise à jour le 22/05/2023**

## Objectif

DiskStation Manager 7.0 de Synology propose un outil de synchronisation avec différentes solutions Cloud.

Il est compatible avec l'Object Storage du Public Cloud OVHcloud et vous permet donc d'effectuer une sauvegarde de vos données et de les rendre accessibles depuis n'importe quel endroit.

**Ce guide vous explique comment configurer DiskStation Manager 7.0 afin de synchroniser les fichiers se trouvant sur votre NAS vers votre Object Storage.**

> [!primary]
>
> DiskStation Manager 6 n'est pas compatible avec l'Object Storage Public Cloud OVHcloud.
>

## Prérequis

- [Créer un conteneur de stockage](https://docs.ovh.com/ca/fr/storage/object-storage/pcs/create-container/)
- [Créer un utlisateur OpenStack](https://docs.ovh.com/ca/fr/public-cloud/creation-et-suppression-dun-utilisateur-openstack/#creation-dun-utilisateur-openstack)

## En pratique

### Configuration de DiskStation Manager 7.0

> [!warning]
>
> Les solutions Synology telles que DiskStation ou Hyperbackup ne sont pas compatible avec l'offre Public Cloud Archive.
>

#### Récupération de vos identifiants Openstack

Afin de configurer la synchronisation de votre NAS Synology, vous devez être en possession des identifiants de votre utilisateur OpenStack.

Vous pouvez les récupérer en téléchargeant le fichier OpenRC à l'aide de la première partie du guide suivant :

- [Charger les variables d'environnement OpenStack](https://docs.ovh.com/ca/fr/public-cloud/set-openstack-environment-variables/#recuperation-des-variables){.ref}

#### Configuration du point de synchronisation avec Cloud Sync

Une fois en possession de vos identifiants, vous pouvez vous connecter sur votre NAS et effectuer ces différentes actions :

- Lancer l'application Cloud Sync 

- Sélectionner OpenStack Swift en tant que Cloud Provider :

![public-cloud](images/DSM7_1.png){.thumbnail}

- Renseigner les informations de votre utilisateur OpenStack :

![public-cloud](images/DSM7_2.png){.thumbnail}

Toutes ces informations sont trouvables dans le fichier OpenRC que vous avez récupéré lors de la précédente étape.

- Configurer la localisation ainsi que le nom de votre conteneur de stockage :

![public-cloud](images/DSM7_3.png){.thumbnail}

- Configurer votre dossier à synchroniser :

![public-cloud](images/DSM7_4.png){.thumbnail}

## Aller plus loin

Si vous avez besoin d'une formation ou d'une assistance technique pour la mise en oeuvre de nos solutions, contactez votre commercial ou cliquez sur [ce lien](https://www.ovhcloud.com/fr-ca/professional-services/) pour obtenir un devis et demander une analyse personnalisée de votre projet à nos experts de l’équipe Professional Services.

Échangez avec notre communauté d’utilisateurs sur <https://community.ovh.com>.
