# Polygon DID Method Specification

## Preface

The polygon-did method specification is complaint with the [DID requirements](https://www.w3.org/TR/did-core/#ref-for-dfn-did-documents-3) specified by W3C Creredentials Community Group. For a detailed read on DID and other DID method specifications, one can refer [this](https://github.com/WebOfTrustInfo/rwot5-boston/blob/master/topics-and-advance-readings/did-primer.md)

## Abstract

The Polygon DID method allows any Ethereum key pair account to become a valid identity. For registration of the DID Document, a smart contract has been deployed on polygon testnet and main net on addresses specified at [registry-contract](https://github.com/ayanworks/polygon-did-registry-contract)

## Target System

The polygon-register-smart-contract is deployed on
1. Polygon Mainnet
2. Polygon Testnet

## DID Method Name

The DID uri for Polygon specific DID method is: 'polygon' .
A DID compatible with Polygon network will entail a prefix "did:polygon". This is fixed, and always in lower case based upon the DID specifications. 
A DID uri on Polygon testnet will entail a prefix of order "did:polygon:testnet".

## DID Method Specific Identifier

For the polygon DID representation, the MSI (Method Specific Identifier) is an ethereum address, which can also be called as a Hex encoded secp256k1 compressed public key. 

For Polygon main net
```js
did:polygon:0x1CE7AB4d4Aee57Cac9FA09e951BB4452ED856367
```
For Polyon testnet
```js
did:polygon:testnet:0x1CE7AB4d4Aee57Cac9FA09e951BB4452ED856367
```
To be noted:
The address corresponding to the user's private key or generated private key, is supposed to be used to carry out functions like Create, Resolve, Update and Delete. The account should also hold a minimum balance of Matic tokens to pay as gas. 

## CRUD

### Create

To create a polygon DID, the user is required to either hold a private key, of a Ethereum walllet, or the user can opt to generate one. For both cases the user summons the 'createDID' function with a network type(mainnet/ testnet) and a private key(if available).It should be noted that in cases where a private key is not provided, the createDID function will itself create a new wallet for the user on chosen network. The above said function returns:

```js
<address, publicKeyBase58, privateKey, DID_uri>
```

Next the user will initiate a call to the registerDID function  with his generated DID uri and private Key and other parameters as contract address and RPC url(for chain identification). The function will create a corresponding DID Document of format given below and it will be logged on chain.

```json
{
	"@context": "https://w3id.org/did/v1",
	"id": "did:polygon:0x2C020b0112A1B98C1BB5FC490C44772357E73803",
	"verificationMethod": [{
		"id": "did:polygon:0x2C020b0112A1B98C1BB5FC490C44772357E73803",
		"type": "EcdsaSecp256k1VerificationKey2019",
		"controller": "did:polygon:0x2C020b0112A1B98C1BB5FC490C44772357E73803",
		"publicKeyBase58": "7Lnm1frErwLwwZB1x2XbweLauYJpAZBjGxAXk55u248DEGGKF62apu9QuekaE3d7jMUUeHjk2F4sSYqKF3oeQ6b3ZLuMb"
	}]
}
```

#### Transaction Fee

To register a DID on the Polygon chain, a minimal fee in form of gas will be required. This gas fee is supposed to be paid in Polygon's native token, the MATIC token. Create, Update and Delete are the operations which will require a transaction fee.

### Resolve

Resolving a DID implies the act of fetching the DID doc registered on chain. The resolver when queried with a DID returns the associated DID doc. A query is sent out to fetch the registered DID Doc from the chain. This Doc is then used for signing or verification purposes.

#### Universal Resolver

A Universal Resolver is an identifier resolver that works with any decentralized identifier system, including Decentralized Identifiers (DIDs). Refer [here](https://github.com/decentralized-identity/universal-resolver). A driver for Polygon DID is added to universal resolver configurations to make it publicly accessible.

### Update

There is a set of rules pre-defined for how a DID document is supposed to be defined, but at times a need might arise to update the DID doc. So to help users update their DID doc, the Polygon DID method, has been enhanced with update DID functionality. This function is accessible only to the controller/owner of DID. 

### Delete

The owner of DID doc holds the authority to his instance of DID doc on chain, and to provide him with true ownership, the network facilitates the user with ability to delete his DID doc instance from the chain at any point of time. It should be noted that only the owner/controller of the DID Doc, will be allowed to delete the instance.

## Security Considerations

To mitigate attacks all transactions to register, update and delete DID on Polygon chain are signed using key pairs generated by secp256k1 algorithm. Any vulnerabilities in that algorithm will reflect onto the Polygon DID method protocol as well.
To enhance security, the Polygon DID method implementation stores only DID document on chain, with valid time stamps.

## Privacy

In view of privacy, a DID is pseudonymous. Though the user, using the implementation should take not, that since DID Polygon is registered on chain in a decentralized fashion, it is not fully revocable. It is also worth noting, that a DID document registered once, can only be updated and revoked by the owner of DID as a part of privacy consideration.

## Reference Implementation

The users who wish to have a DID on polygon, are expected to use the refrence implementation of ['polygon-did-registrar'](https://github.com/ayanworks/polygon-did-registrar) and ['polygon-did-resolver'](https://github.com/ayanworks/polygon-did-resolver) to register and resolve polygon based DID's on chain.
