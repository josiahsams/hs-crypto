---

copyright:
  years: 2020
lastupdated: "2020-07-09"

keywords: restore key, restore a deleted key, re-import a key

subcollection: hs-crypto

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{: preview: .preview}
{:term: .term}

# Restoring keys
{: #restore-keys}

You can use {{site.data.keyword.cloud}} {{site.data.keyword.hscrypto}} to restore a previously deleted root key and access its associated data on the cloud.
{: shortdesc}

As an admin, you might need to restore a root key that you import to {{site.data.keyword.hscrypto}} so that you can access data that the key previously protect. When you restore a key, you move the key from the _Destroyed_ to the _Active_ key state, and you restore access to any data that is previously encrypted with the key.

You can restore a deleted key within 30 days of its deletion. This capability is available only for root keys that are previously imported to the service. You can't restore root keys that are generated by {{site.data.keyword.hscrypto}}.
{: note}

## Restoring a deleted key with the API
{: #restore-api}

Restore a previously imported root key by making a `POST` call to the following endpoint.

```
https://api.<region>.hs-crypto.cloud.ibm.com:<port>/api/v2/keys/<key_ID>?action=restore
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/hs-crypto?topic=hs-crypto-set-up-kms-api#retrieve-kms-credentials).

    To restore a key, you must be assigned a _Manager_ access policy for the instance or key. To learn how IAM roles map to {{site.data.keyword.hscrypto}} service actions, check out [Service access roles](/docs/hs-crypto?topic=hs-crypto-manage-access#service-access-roles).
    {: note}

2. Retrieve the key management API endpoint URL.

    You can get the API endpoint from your provisioned service instance dashboard by clicking **Manage** &gt; **Key management endpoint URL**, or you can dynamically [retrieve the API endpoint URL](https://{DomainName}/apidocs/hs-crypto#retrieve-the-api-endpoint-url){: external} with an API call. Select the public or private key manage endpoint URL based on your needs.

3. Retrieve the ID of the key that you want to restore.

    You can retrieve the ID for a specified key by making a [list keys API request](https://{DomainName}/apidocs/hs-crypto#list-keys){: external}, or by viewing your keys in the {{site.data.keyword.hscrypto}} dashboard.

4. Make the following API call to restore the key and regain access to its associated data.

   You cannot restore a key that has an expiration date that is current or in the past.
   {: important}

   You must wait 30 seconds after deleting a key before you are able to restore it.
   {: note}

    ```cURL
    curl -X POST \
      https://api.<region>.hs-crypto.cloud.ibm.com:<port>/api/v2/keys/<key_ID>?action=restore \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -d '{
        "metadata": {
          "collectionType": "application/vnd.ibm.kms.key+json",
          "collectionTotal": 1
        },
        "resources": [
          {
            "payload": "<key_material>"
          }
        ]
      }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>

      <tr>
        <td>
          <varname>region</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-de</code>, that represents the geographic area where your {{site.data.keyword.hscrypto}} service instance resides.
          </p>
          <p>
            For more information, see [Regional service endpoints](/docs/hs-crypto?topic=hs-crypto-regions#service-endpoints).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_ID</varname>
        </td>
        <td>
          <strong>Required.</strong> The unique identifier for the key that you want to restore.
        </td>
      </tr>

      <tr>
        <td>
          <varname>IAM_token</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request.
          </p>
          <p>
            For more information, see [Retrieving an access token](/docs/hs-crypto?topic=hs-crypto-retrieve-access-token).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>instance_ID</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.hscrypto}} service instance.
          </p>
          <p>
            For more information, see [Retrieving an instance ID](/docs/hs-crypto?topic=hs-crypto-retrieve-instance-ID).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_material</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The base64 encoded key material that you want to store and manage in the service. It should be a root key material that you previously import.
          </p>
          <p>
            Ensure that the key material meets the following requirements:
          </p>
          <p>
            <ul>
              <li>
                The key must be 128, 192, or 256 bits.
              </li>
              <li>
                The bytes of data, for example 32 bytes for 256 bits, must be encoded by using base64 encoding.
              </li>
            </ul>
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to restore keys with the {{site.data.keyword.hscrypto}} API.
      </caption>
    </table>

    A successful restore request returns an HTTP `201 Created` response, which indicates that the key was restored to the _Active_ key state and is now available for encrypt and decrypt operations. All attributes and policies that were previously associated with the key are also restored.

    You will have access to data associated with the key as soon as the key is restored.
    {: note}

5. Optional: Verify that the key was restored by retrieving details about the key.

    ```cURL
    curl -X GET \
      https://api.<region>.hs-crypto.cloud.ibm.com:<port>/api/v2/keys/<key_id>/metadata \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'accept: application/vnd.ibm.kms.key+json'
    ```
    {: codeblock}

    Review the `state` field in the response body to verify that the key transitioned to the _Active_ key state. The following JSON output shows the metadata details for an _Active_ key.

    The integer mapping for the _Active_ key state is 1. Key States are based on NIST SP 800-57.
    {: note}

    ```json
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 1
      },
      "resources": [
        {
          "type": "application/vnd.ibm.kms.key+json",
          "id": "02fd6835-6001-4482-a892-13bd2085f75d",
          "name": "...",
          "description": "...",
          "tags": [
            "..."
          ],
          "state": 1,
          "extractable": false,
          "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
          "imported": true,
          "creationDate": "2020-03-10T20:41:27Z",
          "createdBy": "...",
          "algorithmType": "AES",
          "algorithmMetadata": {
            "bitLength": "128",
            "mode": "CBC_PAD"
          },
          "algorithmBitSize": 128,
          "algorithmMode": "CBC_PAD",
          "lastUpdateDate": "2020-03-16T20:41:27Z",
          "keyVersion": {
            "id": "30372f20-d9f1-40b3-b486-a709e1932c9c",
            "creationDate": "2020-03-12T03:37:32Z"
          },
          "dualAuthDelete": {
            "enabled": false
          },
          "deleted": false
        }
      ]
    }
    ```
    {: screen}

<!-- ### Using an import token to restore a key
{: #restore-keys-secure-api}

If you initially used an import token to import the root key, you can restore the key by making a `POST` call to the following endpoint.

```
https://api.<region>.hs-crypto.cloud.ibm.com:<port>/api/v2/keys/<key_ID>?action=restore
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/hs-crypto?topic=hs-crypto-set-up-kms-api#retrieve-kms-credentials).

    To restore a key, you must be assigned a _Manager_ service access role for the instance or key. To learn how IAM roles map to {{site.data.keyword.hscrypto}} service actions, check out [Service access roles](/docs/hs-crypto?topic=hs-crypto-manage-access#service-access-roles).
    {: note}

2. Retrieve the key management API endpoint URL.

    You can get the API endpoint from your provisioned service instance dashboard by clicking **Manage** &gt; **Key management endpoint URL**, or you can dynamically [retrieve the API endpoint URL](https://{DomainName}/apidocs/hs-crypto#retrieve-the-api-endpoint-url){: external} with an API call. Select the public or private key manage endpoint URL based on your needs.

3. Retrieve the ID of the key that you want to restore.

    You can retrieve the ID for a specified key by making a [list keys API request](https://{DomainName}/apidocs/hs-crypto#list-keys){: external}, or by viewing your keys in the {{site.data.keyword.hscrypto}} dashboard.

4. [Create and retrieve an import token](/docs/hs-crypto?topic=hs-crypto-create-import-tokens).

5. Use the import token to encrypt the key that you want to restore.

    To learn how to use an import token, check out
    [Tutorial: Creating and importing encryption keys](/docs/hs-crypto?topic=hs-crypto-tutorial-import-keys).

6. Restore the key and regain access to its associated data by running the following cURL command.

    ```cURL
    curl -X POST \
      https://api.<region>.hs-crypto.cloud.ibm.com:<port>/api/v2/keys/<key_ID>?action=restore \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -d '{
        "metadata": {
          "collectionType": "application/vnd.ibm.kms.key+json",
          "collectionTotal": 1
        },
        "resources": [
          {
            "payload": "<encrypted_key>",
            "encryptionAlgorithm": "RSAES_OAEP_SHA_256",
            "encryptedNonce": "<encrypted_nonce>",
            "iv": "<iv>"
          }
        ]
      }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>

      <tr>
        <td>
          <varname>region</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-de</code>, that represents the geographic area where your {{site.data.keyword.hscrypto}} service instance resides.
          </p>
          <p>
            For more information, see [Regional service endpoints](/docs/hs-crypto?topic=hs-crypto-regions#service-endpoints).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_ID</varname>
        </td>
        <td>
          <strong>Required.</strong> The unique identifier for the root key that you want to enable.
        </td>
      </tr>

      <tr>
        <td>
          <varname>IAM_token</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request.
          <p>
          <p>
            For more information, see [Retrieving an access token](/docs/hs-crypto?topic=hs-crypto-retrieve-access-token).
          <p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>instance_ID</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.hscrypto}} service instance.
          </p>
          <p>
            For more information, see [Retrieving an instance ID](/docs/hs-crypto?topic=hs-crypto-retrieve-instance-ID).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>encrypted_key</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The base64 encoded key material that you want to store and manage in the service. It should be a root key material that you previously import.
          </p>
          <p>
            Ensure that the key material meets the following requirements:
          </p>
          <p>
            <ul>
              <li>
                The key must be 128, 192, or 256 bits.
              </li>
              <li>
                The bytes of data, for example 32 bytes for 256 bits, must be encoded by using base64 encoding.
              </li>
            </ul>
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>encrypted_nonce</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The AES-CBC encrypted nonce that ensures that the bits you send as part of a request are exactly the same as what we receive. The nonce validates the key that you are restoring.
          </p>
          <p>
            To learn more, see
            [Tutorial: Creating and importing encryption keys](/docs/hs-crypto?topic=hs-crypto-tutorial-import-keys#tutorial-import-encrypt-nonce).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>iv</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The initialization vector (IV) that is generated by the AES-CBC algorithm when you encrypt a nonce. This value is used to decode the key for storage in {{site.data.keyword.hscrypto}}.
          </p>
          <p>
            To learn more, see
            [Tutorial: Creating and importing encryption keys](/docs/hs-crypto?topic=hs-crypto-tutorial-import-keys#tutorial-import-encrypt-nonce).
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 2. Describes the variables that are needed to restore keys via
        Import Token with the {{site.data.keyword.hscrypto}}
        API.
      </caption>
    </table>

    A successful restore request returns an HTTP `201 Created` response, which indicates that the key was restored to the _Active_ key state and is now available for encrypt and decrypt operations. All attributes and policies that were previously associated with the key are also restored.

    You will have access to data associated with the key as soon as the key is restored.
    {: note}

7. Optional: Verify that the key was restored by retrieving details about the key.

    ```cURL
    curl -X GET \
      https://api.<region>.hs-crypto.cloud.ibm.com:<port>/api/v2/keys/<key_id>/metadata \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'accept: application/vnd.ibm.kms.key+json'
    ```
    {: codeblock}

    Review the `state` field in the response body to verify that the key transitioned to the _Active_ key state. The following JSON output shows the metadata details for an _Active_ key.

    The integer mapping for the _Active_ key state is 1. Key States are based on NIST SP 800-57.
    {: note}

    ```json
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 1
      },
      "resources": [
        {
          "type": "application/vnd.ibm.kms.key+json",
          "id": "02fd6835-6001-4482-a892-13bd2085f75d",
          "name": "...",
          "description": "...",
            "tags": [
              "..."
            ],
          "state": 1,
          "extractable": false,
          "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
          "imported": true,
          "creationDate": "2020-03-10T20:41:27Z",
          "createdBy": "...",
          "algorithmType": "AES",
          "algorithmMetadata": {
            "bitLength": "128",
            "mode": "CBC_PAD"
          },
          "algorithmBitSize": 128,
          "algorithmMode": "CBC_PAD",
          "lastUpdateDate": "2020-03-16T20:41:27Z",
          "keyVersion": {
            "id": "30372f20-d9f1-40b3-b486-a709e1932c9c",
            "creationDate": "2020-03-12T03:37:32Z"
          },
          "dualAuthDelete": {
            "enabled": false
          },
          "deleted": false
        }
      ]
    }
    ```
    {: screen} -->