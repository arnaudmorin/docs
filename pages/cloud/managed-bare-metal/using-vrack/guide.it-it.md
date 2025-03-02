---
title: Utilizzare Managed Bare Metal all'interno di una vRack
routes:
    canonical: 'https://docs.ovh.com/it/private-cloud/aggiungere_un_dedicated_cloud_alla_vrack_15_e_configurare_una_vm/'
excerpt:  Scopri come utilizzare la vRack con la tua soluzione Managed Bare Metal
slug: vrack-essentials
section: Servizi e opzioni OVHcloud
order: 02
updated: 2020-11-23
---

> [!primary]
> Questa traduzione è stata generata automaticamente dal nostro partner SYSTRAN. I contenuti potrebbero presentare imprecisioni, ad esempio la nomenclatura dei pulsanti o alcuni dettagli tecnici. In caso di dubbi consigliamo di fare riferimento alla versione inglese o francese della guida. Per aiutarci a migliorare questa traduzione, utilizza il pulsante "Contribuisci" di questa pagina.
>

**Ultimo aggiornamento: 05/11/2020**

## Obiettivo

La vRack è la possibilità di connettere diversi servizi Cloud di OVHcloud all'interno di una o più reti private sicure (VLAN).

**Questa guida ti mostra come attivarlo**

## Procedura

### Spazio cliente

Al momento della consegna del servizio [Managed Bare Metal](https://www.ovhcloud.com/it/managed-bare-metal/), la sezione *datacenter* è già all'interno di una vRack.

![Datacenter](images/vRackDatacenter.PNG){.thumbnail}

Per spostare il *datacenter* del tuo Managed Bare Metal in un'altra vRack, clicca sul pulsante `Sposta`{.action}

### Client vSphere

Nel client vSphere, le *VLANs* compatibili con la vRack sono disponibili nello switch virtuale distribuito (vds), a sua volta situato nella cartella **vrack**.

> [!success]
>
> Di default, OVHcloud fornisce un'infrastruttura con 11 VLAN (da VLAN10 a VLAN20).
>

![VLAN](images/vRackVsphere.png){.thumbnail}

È possibile modificare le impostazioni o crearne di nuove seguendo la guida alla [creazione di VLAN](../creazione-vlan/.

Questi *portgroup* possono essere assegnati alle interfacce di rete delle macchine virtuali.

## Per saperne di più

Contatta la nostra Community di utenti all’indirizzo <https://community.ovh.com/en/>.
