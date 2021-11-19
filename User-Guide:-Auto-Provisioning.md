OpenRemote offers functionality for automatic client and asset provisioning. If you build and distribute your own hardware devices, you can use this mechanism to have your devices automatically register and connect to OpenRemote.

# Terminology

Client: Refers to the initiator in communication with OpenRemote; the same meaning as in authentication terminology
Asset: Any asset within the OpenRemote system
Provisioning: Creation within the OpenRemote system

# Provisioning Type

There are two basic mechanisms for client provisioning.

## X.509 Client Certificate

Supports the industry standard X.509 certificate authentication mechanism where each client has a unique client certificate that is signed by a CA certificate that must also be registered within OpenRemote, the certificate must contain a unique ID within the CN attribute of the certificate. OpenRemote then verifies the certificate and CN attribute presented during provisioning. 

This is the most secure authentication mechanism but adds complexity to the manufacturing/flashing process.

## Symmetric Key (HMAC-SHA256)

**NOT YET IMPLEMENTED**

Uses a shared secret for authentication; HMAC protects the integrity and authenticity of the encrypted message; the HMAC is generated from a unique client ID and the shared secret; OpenRemote then verifies the HMAC based on the unique ID presented during provisioning.

This mechanism is less secure especially if the shared secret is stored in an accessible way on the client device; using a HSM (Hardware Security Module) protects against this; also pre-generating the client HMAC and loading this onto the client device before provisioning means the secret does not need to be shared, but once again complicates the manufacturing process.

# Connect flow

The following illustrates the connect process which clients can use to auto provision a service user and optionally an asset whose ID is generated using a UNIQUE_ID provided by the client; the client is then authenticated and the asset is then returned to the client.

<kbd>![Auto provisioning Connect flow](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Auto%20provisioning%20Connect%20flow.png)</kbd>

## X.509 Client Certificate Validation

1. Find X.509 realm config whose CA cert subject matches the client cert issuer
2. Check client certificate has been signed by the CA
3. Extract client certificate subject ‘CN’ value
4. Check it matches UNIQUE_ID

## Symmetric Key Validation

**NOT YET IMPLEMENTED**

1. Regenerate HMAC using shared secret and UNIQUE_ID and check it matches
2. Find realm config match where HMAC generated using secret matches

# Message Schema

## X.509 Provisioning Request Message
The provisioning message format for X.509 is as follows:

{
  “type”: “x509”,
  “cert”: “...”
}


The cert field should be in PEM format and must contain the certificate chain up to and including the CA certificate registered within OpenRemote.

## Symmetric Key Provisioning Request Message

**NOT YET IMPLEMENTED**

The provisioning message format for HMAC is as follows:

    {
      “type”: “hmac-sha256”,
      “code”: “...”
    }

The code field should be the base64 encoded HMAC specific to this client.

## Success Response Message

    {
      “type”: “success”,
      “realm”: “REALM_NAME”,
      “asset”: {...}
    }

## Error Response Message

    {
      “type”: “error”,
      “error”: “ERROR_TYPE”
    }

### Error Type

* MESSAGE_INVALID - Failed to parse the request message
* CERTIFICATE_INVALID - The X.509 certificate is not valid
* UNAUTHORIZED - No matching config found
* FORBIDDEN - Unique ID fails whitelist/blacklist
* UNIQUE_ID_MISMATCH - Unique ID used in subscription does not match credentials
* CONFIG_DISABLED - Matched realm config is marked as disabled
* USER_DISABLED - Service user previously provisioned has been disabled
* SERVER_ERROR - Unknown exception occurred during processing
* ASSET_ERROR - Asset previously provisioned is not in the same realm as the matched realm config

## Certificate Generation

Client certificate generation is done using standard tooling e.g. openssl:

1. A unique client private key and X.509 certificate should be generated with the client’s unique ID stored in the CN attribute of the certificate.
1. The certificate should then be signed by an intermediate CA.
1. The intermediate CA certificate is then uploaded into OpenRemote within a Realm config instance

When the client publishes its’ certificate to OpenRemote it must be the certificate chain including the intermediate CA certificate that was uploaded to OpenRemote and this must be in the PEM format. Client certificates can take place within the manufacturing environment without any external dependencies. 

**NOTE: The security of the CA private key(s) is essential, if compromised then the certificate can be marked as revoked within OpenRemote and this will require all client certificates signed by this compromised CA certificate to be replaced at considerable effort.**

# See Also

* [Git GitHub, Self Signed Certificate with Custom Root CA](https://gist.github.com/fntlnz/cf14feb5a46b2eda428e000157447309)
* [Whitepaper Device Manufacturing and Provisioning with X.509](https://d1.awsstatic.com/whitepapers/device-manufacturing-provisioning.pdf)
