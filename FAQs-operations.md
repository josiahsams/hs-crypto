---

copyright:
  years: 2020
lastupdated: "2020-07-17"

keywords: frequently asked questions, cryptographic algorithm, regions, pricing, security compliance, key ceremony, critical security parameters, cryptographic module, security Level, fips, provisioning, operations

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

# FAQs: Provisioning and operations
{: #faq-provisioning-operations}

This topic can help you with questions about provisioning an {{site.data.keyword.cloud}} {{site.data.keyword.hscrypto}} instance and related operations.
{: shortdesc}

## Are there any prerequisites for using {{site.data.keyword.hscrypto}}?
{: #faq-hpcs-prerequisites}
{: faq}

To use {{site.data.keyword.hscrypto}}, you need to have a Pay-As-You-Go or Subscription {{site.data.keyword.cloud_notm}} account. Other than that, there are no prerequisites.

If you don't have an {{site.data.keyword.cloud_notm}} account, create a Lite account first by going to [{{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/login){: external} and clicking **Create an {{site.data.keyword.Bluemix_notm}} account**. And then [upgrade it to a Pay-As-You-Go or Subscription {{site.data.keyword.cloud_notm}} account](/docs/account?topic=account-upgrading-account). You can also [apply your promo code](/docs/billing-usage?topic=billing-usage-applying-promo-codes) if you have one. For more information about {{site.data.keyword.cloud_notm}} accounts, see [FAQs for accounts](/docs/account?topic=account-accountfaqs).

The service can be provisioned quickly by following instructions in [Provisioning service instances](/docs/hs-crypto?topic=hs-crypto-provision). However, in order to perform key management and cryptographic operations, you need to initialize service instances first by using [{{site.data.keyword.cloud_notm}} TKE CLI plug-in](/docs/hs-crypto?topic=hs-crypto-initialize-hsm) or the [Management Utilities](/docs/hs-crypto?topic=hs-crypto-initialize-hsm-management-utilities) by using smart cards.

## How to initialize {{site.data.keyword.hscrypto}} service instances?
{: #faq-how-to-initialize}
{: faq}
{: support}

To initialize the service instance, you need to create administrator signature keys, exit the imprint mode, and load the master key to the instance. To meet various security requirements of your enterprises, IBM offers you the following options to load the master key: 

- Using the [IBM {{site.data.keyword.hscrypto}} Management Utilities](/docs/hs-crypto?topic=hs-crypto-introduce-service#understand-management-utilities) for the highest level of security. This solution uses smart cards to store signature keys and master key parts. Signature keys and master key parts never appear in the clear outside the smart card.

- Using the [{{site.data.keyword.cloud_notm}} TKE CLI plug-in](/docs/hs-crypto?topic=hs-crypto-introduce-service#understand-tke-plugin) for a solution that does not require the procurement of smart card readers and smart cards. This solution uses workstation files encrypted with a key that is derived from a file password to store signature keys and master key parts. When the keys are used, file contents are decrypted and appear temporarily in the clear in workstation memory.

## Are there any recommendations on how to set up smart cards?
{: #faq-smart-card-setup}
{: faq}
{: support}

It is recommended to procure eight or 10 smart cards and two smart card readers. The smart cards can be set up to contain a primary set and a backup set. For details of the recommendations, see [Smart card setup recommendations](/docs/hs-crypto?topic=hs-crypto-introduce-service#smart-card-considerations). To find out details on how to procure and set up smart cards and other Management Utilities components, see [Setting up the Management Utilities](/docs/hs-crypto?topic=hs-crypto-prepare-management-utilities).

## How can I procure smart cards and smart card readers?
{: #faq-procure-smart-card}
{: faq}

If you are in the United States, you can order smart cards and smart card readers online. For details, see [Order smart cards and smart card readers](/docs/hs-crypto?topic=hs-crypto-prepare-management-utilities#order-smart-card-and-reader).

For procurement from other countries, contact IBM Part Sales through the following email addresses. For countries that are not listed, contact `ppricing@de.ibm.com`. In the email, provide the Field Replaceable Unit (FRU) number, 00RY790, for the smart card.

|Country| Email address |
|--------------|-----------------------|
|Belgium  | parts@be.ibm.com|
|Denmark  | reservedele@dk.ibm.com|
|Germany | partsale@de.ibm.com|
|France | vtepce@fr.ibm.com|
|Ireland | emeapart@ie.ibm.com|
|Israel | psale@il.ibm.com|
|Poland | parts@pl.ibm.com|
|Portugal | ptsales@pt.ibm.com|
|South Africa    |autoship@za.ibm.com |
|Spain    | ventadepiezas@es.ibm.com|
|Switzerland | pasa@ch.ibm.com|
|UK | parts_sales@uk.ibm.com|
{: caption="Table 2. IBM Part Sales contacts" caption-side="bottom"}

## How many crypto units should I set up in my service instance?
{: #faq-crypto-units-number}
{: faq}

You need to set up at least two crypto units for high availability. {{site.data.keyword.hscrypto}} currently sets the upper limit of crypto unit to three.

## Can I use {{site.data.keyword.hscrypto}} along with other {{site.data.keyword.cloud_notm}} services?
{: #faq-hpcs-with-cloud-services}
{: faq}

Yes. {{site.data.keyword.hscrypto}} can be integrated with many {{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.cos_full_notm}}, {{site.data.keyword.vmwaresolutions_short}}, {{site.data.keyword.containerlong_notm}}, and {{site.data.keyword.openshiftlong_notm}}. For a complete list of services and instructions on integrations, see [Integrating services](/docs/hs-crypto?topic=hs-crypto-integrate-services). 

## How does my application connect to a {{site.data.keyword.hscrypto}} service instance?
{: #faq-application-connection}
{: faq}

{{site.data.keyword.hscrypto}} provides the standard APIs for users to access. Your applications can connect to a {{site.data.keyword.hscrypto}} service instance by using the APIs directly over the public internet. If a more secured and isolated connection is needed, you can also use [private endpoints](/docs/hs-crypto?topic=hs-crypto-private-endpoints). You can connect your service instance through {{site.data.keyword.cloud_notm}} service endpoints over the {{site.data.keyword.cloud_notm}} private network.

<!--
## Can I use language characters as part of the key name?
{: #faq-key-name-rules}
{: faq}
{: support}

Language characters, such as Chinese characters, cannot be used as part of the key name.
-->

## Can I generate master key on-premises and store the master key parts in the smart cards?
{: #faq-generate-master-key-on-premises}

Generating master key on-premises is currently not supported.

## Can I import root keys from an on-premises HSM?
{: #faq-import-key-on-premises}
{: faq}

Importing root keys from an on-premises HSM is currently not supported.

## Can I use {{site.data.keyword.hscrypto}} only for cryptographic operations, but use other {{site.data.keyword.cloud_notm}} services such as {{site.data.keyword.keymanagementserviceshort}} for key management?
{: #faq-hpcs-with-key-protect}
{: faq}

Yes. {{site.data.keyword.hscrypto}} can be used with {{site.data.keyword.keymanagementserviceshort}} for key management. In this way, {{site.data.keyword.hscrypto}} is responsible for only cryptographic operations, while {{site.data.keyword.keymanagementserviceshort}} provides key management service secured by multi-tenant FIPS 140-2 Level 3 certified cloud-based HSM.

<!--
## Can I use {{site.data.keyword.hscrypto}} only for cryptographic operations, but use my existing on-premises key management system for key storage?
{: #faq-hpcs-cryptography-only}
{: faq}

Integration with on-premises key management system is currently not supported.
-->

## Can I use {{site.data.keyword.hscrypto}} in conjunction with other cloud provider services such as AWS and Azure?
{: #faq-hpcs-other-cloud-vendor}
{: faq}

Yes. Any application can connect to {{site.data.keyword.hscrypto}} and use our APIs from anywhere on the internet.
