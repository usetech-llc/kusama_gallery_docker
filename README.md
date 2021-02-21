# Dockerfiles for Chiba gallery backend

## Configuring

1. Create .env file with database username and password. This user and password will be used to initialize postreSQL DB that stores indexed data

2. Put the same user name and password in app.config.kusama. Also, put there the wss URL to the (Kusama, Westend, etc.) chain node. This file is used to initialize the indexer service so that it can connect to the database and to the Substrate node that hosts Chiba gallery pallet.

3. Put the same user name and password in appsettings.json file. This file is used to initialize Web API that provides REST endpoints to the Chiba frontend so that it can connect to the DB and read the indexed data.

## Starting

Start the backend services:

```
docker-compose up -d
```

## Using 

The following RESTful endpoints will be available on port 5000:

`GET /nft/{collection_id}/{token_id}`

Get NFT information. Includes the owner address, the URL or IPFS pin for images/files to be displayed, NFT status (display, hide, report_pending, or reported) and report reason (none, illegal, plagiarism, duplicate).

`GET /display`

Return the list of NFTs displayed in the gallery. Includes the URLs for images to display for each NFT.

Does not include NFTs with “Hidden”, “Reported” or “burned” status.

`GET /wallet/collections/{address}`

Return the list of collection IDs for collections created by this address. (Initial requirement - there will only be one collection per address).

`GET /wallet/owned_nfts/{address}`

Return the list of NFTs owned by the address. Includes the URLs for images to display for each NFT.

Includes NFTs with “Hidden” and “Reported” status, does not include NFTs with “Burned” status.

`GET /wallet/created_nfts/{address}`

Return the list of NFTs created by the address. Includes the URLs for images to display for each NFT.

`GET /wallet/appreciation/{address}`

Return the list of appreciations given or received by an address.

`GET /offers/{address}`

Returns the offers made by or to the address, if address is specified. If not, returns all offers. Includes collection id, token id, date of creation, seller address, buyer address, price, and offer status (pending, completed, or canceled) 

`GET /report?status={status}`

Returns the list of all currently reported NFTs with the given status (`pending` or `reported`). Each entry of the list includes all fields from the token table (collection_id, token_id, etc.).
