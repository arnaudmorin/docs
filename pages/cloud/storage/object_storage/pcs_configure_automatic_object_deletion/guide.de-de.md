---
title: Object Storage Swift - Automatisches Löschen von Objekten konfigurieren (EN)
excerpt: Automatisches Löschen von Objekten konfigurieren
slug: pcs/configure-automatic-object-deletion
routes:
    canonical: 'https://docs.ovh.com/gb/en/storage/object-storage/pcs/configure-automatic-object-deletion/'
section: OpenStack Swift Storage Class Specifics
legacy_guide_number: g1950
order: 070
updated: 2021-10-27
---

**Last updated 27th October 2021**

## Objective

To easily manage your Object Storage, you may need to define the lifespan of some of your files. This allows you, for example, to keep some backups only for a specific period.

**This guide shows you how to set up automatic file deletion after a specified period or on a specific date.**

## Requirements

- [Prepare the environment to use the OpenStack API](https://docs.ovh.com/de/public-cloud/prepare_the_environment_for_using_the_openstack_api/)
- [Set OpenStack environment variables](https://docs.ovh.com/de/public-cloud/set-openstack-environment-variables/)

## Instructions

There are two ways to delete your objects/files

- After a certain number of seconds
- On a specific date

### After a certain number of seconds

To do this, configure your request's X-Delete-After header

```bash
root@server:~$ swift copy --header "X-Delete-After: 3600" container test.txt
```

The test.txt file will be deleted in an hour.

### On a specific date

First, you need to know what the deletion date is in epoch format.
To help you find the value you need to insert, use a [converter](http://www.epochconverter.com/){.external}.

Then you can enter this date in the X-Delete-At header:

```bash
root@server:~$ swift copy --header "X-Delete-At: 1668877261000" container test.txt
```

The file will therefore be deleted on the 19th November 2022.

## Go further

If you need training or technical assistance to implement our solutions, contact your sales representative or click on [this link](https://www.ovhcloud.com/de/professional-services/) to get a quote and ask our Professional Services experts for assisting you on your specific use case of your project.

Join our community of users on [https://community.ovh.com/en/](https://community.ovh.com/en/){.external}.