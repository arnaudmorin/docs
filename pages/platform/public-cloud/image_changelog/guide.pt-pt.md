---
title: 'Muelog das imagens Public Cloud & VPS'
slug: trocelog-imagens
excerpt:  Descubra as modificações introduzidas nas imagens fornecidas nas soluções Public Cloud e VPS
section: Introdução
order: 8
updated: 2021-02-09
---

> [!primary]
> Esta tradução foi automaticamente gerada pelo nosso parceiro SYSTRAN. Em certos casos, poderão ocorrer formulações imprecisas, como por exemplo nomes de botões ou detalhes técnicos. Recomendamos que consulte a versão inglesa ou francesa do manual, caso tenha alguma dúvida. Se nos quiser ajudar a melhorar esta tradução, clique em "Contribuir" nesta página.
>

**Última atualização: 18/01/2021**

## Objetivo

A OVHcloud oferece uma variedade de sistemas operativos e aplicações pré-instaladas nos produtos VPS e Public Cloud. Enquanto fornecedor, certificamo-nos de que as imagens que oferecemos aos nossos clientes estão atualizadas, o que significa que publicamos regularmente novas imagens. A transparência é essencial para a OVHcloud e temos a intenção de informar os nossos clientes sobre as modificações efetuadas em cada imagem no seio de qualquer nova versão. Esta página permite-lhe seguir o diário de alterações (changelog) das nossas imagens.

## Requisitos

Esta documentação aplica-se apenas às soluções [VPS](https://www.ovhcloud.com/pt/vps/compare/) e às instâncias [Public Cloud](https://www.ovhcloud.com/pt/public-cloud/compute/).

## Changelog 2021

Esta secção apresenta a alteração em cada mês de 2021.

### June

```
--- 01 June 2021 ---
Image: All
Product: Public Cloud / VPS
Task: <https://bare-metal-servers.status-ovhcloud.com/incidents/920sf4j7d8lq>
Changelog:
1. Adding new image: Fedora 34
2. Adding new image: Ubuntu 21.04
3. New image property "distro_family" added to each image to help group images into distributions.
4. New image property "hypervisor_type" added to each image which will be used by Nova to filter suitable hosts. This is to prevent spawning images made for QEMU to be spawned on Ironic hosts as they are not compatible.
5. Patch issue with Centos 7 - Plesk and Debian 10 - Plesk image with Lets Encrypt SSL. Plesk will now generate unique hostname for each installation unless there is a valid hostname detected on first boot.
6. General system updates applied to all images.

```

### February

```
--- 03 February 2021 ---
Image: All
Product: Public Cloud / VPS
Task: <https://bare-metal-servers.status-ovhcloud.com/incidents/58k8v7rm4crk>
Changelog:
1. All images contain new package / security updates from vendors since our last image updates
2. Centos 7 - cPanel image: we disabled QEMU Guest Agent on this image due to issues caused by creation of a virtfs when allowing Jailed Shell access. This virtfs cannot be frozen by QEMU Guest Agent and therefore causes a kernel panic. Customers with previous release of this image should check [this guide for fix](https://docs.ovh.com/gb/en/vps/cpanel_auto_backup/)

```

### January

```
--- 18 January 2021 ---
Image: All
Product: Public Cloud / VPS
Task: <https://bare-metal-servers.status-ovhcloud.com/incidents/961mgl7qbt35>
Changelog:
1. Release of new Debian 10 - Plesk image
2. All images contain new package / security updates from vendors since our last image updates

```

## Changelog 2020

Esta secção apresenta a alteração em cada mês de 2020.

### November

```
--- 09 November 2020 ---
Image: NVIDIA GPU Cloud (NGC)
Product: Public Cloud
Changelog:
1. We switch from Ubuntu 16.04 to Ubuntu 20.04 base
2. From now on, we will place into the images a file (/etc/cloud/ovhcloud.manifest) which will show packages we installed / ensure is installed in the image
3. Installation of qemu-guest-agent, curl & gpg

--- 17 November 2020 ---
Image: Fedora 33 & Ubuntu 20.10
Product: Public Cloud
Changelog:
The new Fedora 33 and Ubuntu 20.10 images are now available for all public cloud regions. VPS2020 will soon offer the images also.
```

### October

```
--- 07 October 2020 ---
Image: Ubuntu 20.04, Ubuntu 18.04, Ubuntu 16.04, Debian 10, Debian 9, rescue-ovh
Product: VPS 2016 - 2020 & Public Cloud
Task: <https://bare-metal-servers.status-ovhcloud.com/incidents/2m5smflk2706>
Changelog:
1. Installation of qemu-guest-agent, curl & gpg
2. From now on, we will place into the images a file (/etc/cloud/ovhcloud.manifest) which will show packages we installed / ensure is installed in the image
3. Latest patches applied
4. We are moving the rescue-ovh image from Debian 9 to Debian 10 - we install more packages such as zfs utils and other useful diagnostics tools
5. We switch to cloud images with Debian 10 & Debian 9 which are optimized for cloud use. Customers with automated deployment using Debian 9 should expect interface naming have changed. Only change we make on these images is just to install packages mentioned above.

--- 08 October 2020 ---
Image: All Debian 8 images
Product: VPS 2016 - 2020 & Public Cloud
Task: <https://bare-metal-servers.status-ovhcloud.com/incidents/2m5smflk2706>
Changelog:
We have removed the Debian 8 images from our catalogue as it has reached end of life (https://wiki.debian.org/DebianReleases). Pre-installed application images has been replaced with new ones and guides has been made available.

--- 14 October 2020 ---
Image: All Centos images
Product: VPS 2016 - 2020 & Public Cloud
Task: <https://bare-metal-servers.status-ovhcloud.com/incidents/ffvm20r8cwc7>
Changelog:
1. Installation of qemu-guest-agent, curl & gpg
2. From now on, we will place into the images a file (/etc/cloud/ovhcloud.manifest) which will show packages we installed / ensure is installed in the image. For now with pre-installed apps the manifest does not included packages installed by/for the application
3. Latest patches applied

--- 20 October 2020 ---
Image: Fedora 31
Product: VPS 2016 - 2020 & Public Cloud
Task: <https://bare-metal-servers.status-ovhcloud.com/incidents/ypzbqc575dvy>
Changelog:
1. Installation of qemu-guest-agent, curl & gpg
2. From now on, we will place into the images a file (/etc/cloud/ovhcloud.manifest) which will show packages we installed / ensure is installed in the image.
3. Latest patches applied

--- 22 October 2020 ---
Image: Fedora 32
Product: VPS 2016 - 2020 & Public Cloud
Task: <https://bare-metal-servers.status-ovhcloud.com/incidents/1n1fv1xhd9rn>
Changelog:
1. Installation of qemu-guest-agent, curl & gpg
2. From now on, we will place into the images a file (/etc/cloud/ovhcloud.manifest) which will show packages we installed / ensure is installed in the image.
3. Latest patches applied

--- 27 October 2020 ---
Image: Archlinux
Product: VPS 2016 - 2020 & Public Cloud
Task: <https://bare-metal-servers.status-ovhcloud.com/incidents/6jfb3f0f2sjz>
Changelog:
1. Installation of qemu-guest-agent, curl & gpg
2. From now on, we will place into the images a file (/etc/cloud/ovhcloud.manifest) which will show packages we installed / ensure is installed in the image
3. Latest patches applied and using release 2020.10.01
```

### September

```
--- 02 September 2020 ---
Image: Centos 8
Product: VPS 2016 - 2020 & Public Cloud
Task: <https://bare-metal-servers.status-ovhcloud.com/incidents/99m3kb3tzgh3>
Changelog:
1. Qemu Guest Agent installed to improve snapshot experience
2. Installation of pending system updates compared to previous image. Image is up-to-date as of the creation of the image.

--- 21 September 2020 ---
Image: Debian 10
Product: VPS 2016 - 2020 & Public Cloud
Task: <https://bare-metal-servers.status-ovhcloud.com/incidents/5m96knyn78l9>
Changelog:
1. Qemu Guest Agent is now installed and enabled - to improve snapshot experience
2. curl & gpg packages are now installed
3. Using the Debian Cloud image provided by Debian
4. Applying recent system update - up to the date of image build
```


## Quer saber mais?

Fale com a nossa comunidade de utilizadores em <https://community.ovh.com/en/>.
