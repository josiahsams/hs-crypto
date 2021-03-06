---

copyright:
  years: 2020
lastupdated: "2020-07-17"

keywords: frequently asked questions, cryptographic algorithm, regions, pricing, security compliance, key ceremony, critical security parameters, cryptographic module, security Level, fips, HA, DR, high availability, disaster recovery

subcollection: hs-crypto

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}
{:external: target="_blank" .external}
{:support: data-reuse='support'}
{:term: .term}

# FAQs: High availability and disaster recovery
{: #faq-ha-dr}

This topic can help you with high-availability and disaster-recovery related questions about {{site.data.keyword.cloud}} {{site.data.keyword.hscrypto}}.
{: shortdesc}

## How do I set up a high availability configuration?
{: #faq-ha-configuration}
{: faq}

It is recommended that you provision at least two crypto units for high availability, so that there is always at least one additional crypto unit operating in case of a crypto unit failure. {{site.data.keyword.hscrypto}} is built to provide hight availability by default.

For additional details, see [High availability and disaster recovery](/docs/hs-crypto?topic=hs-crypto-ha-dr).

## Can I back up my service instance manually?
{: #faq-backup-manually}
{: faq}

You need to only back up your master key parts and signature keys for service initialization. Your data in {{site.data.keyword.hscrypto}} is backed up automatically by {{site.data.keyword.cloud_notm}} daily.

## What happens if my service instance fails?
{: #faq-instance-fail}
{: faq}

{{site.data.keyword.cloud_notm}} has automatic in-region failover plan in place. Currently, your data is backed up daily by the service and you don't need to do anything to enable it. For [cross-region data restores](/docs/hs-crypto?topic=hs-crypto-ha-dr), you need to open an IBM support ticket so that IBM can restore the service instance for you.

## How can I restore the content from backups?
{: #faq-store-backup}
{: faq}

For cross-region data restores, you need to open an IBM support ticket so that IBM can restore the service instance for you. For more information, see [Restoring your data from another region](/docs/hs-crypto?topic=hs-crypto-restore-data).

## What happens if I delete my service instances?
{: #faq-delete-instance}
{: faq}

If you delete your service instance, your keys that are managed are not accessible.

## Can I back up the keys before I delete a service instance?
{: #faq-backup-keys}
{: faq}

Backing up the keys manually is currently not supported.

## What happens when I delete a key?
{: #faq-delete-a-key}
{: faq}
{: support}

When you delete a key, the key is no longer recoverable and the cloud services that use the key can no longer decrypt data that is associated with the key. Your data remains in those services in its encrypted form. Before you delete a key, ensure that you no longer require access to any data that is associated with the key. This action currently cannot be reversed.

## What happens if I lose the signature key or the master key parts?
{: #faq-lose-signature-key}
{: faq}

If your signature key or master key part is lost, you are not able to initialize your service instance, and your service instance is not accessible. You need to back up you key files on your workstation or back up your smart cards that hold your signature key or master key parts depending on how to store your keys. 
