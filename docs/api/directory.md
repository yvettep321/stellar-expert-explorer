# Address Directory API

REST API interface of the service that maintains a list of well-known Stellar account addresses. It is publicly available for developers, free of charge. 

Each directory entry contains the following set of fields:

- `address` - Stellar address of the account
- `name` - short display name
- `domain` - domain of the public entity operating the account
- `tags` - array of account tags (categories)

## REST API specification

#### `GET /directory/tags` - list all available categories

Example:

```
curl https://api.stellar.expert/explorer/directory/tags
```

```json
[
  {
    "name": "exchange",
    "description": "Centralized exchange account"
  },
  {
    "name": "anchor",
    "description": "Operational account of an anchor"
  },
  {
    "name": "issuer",
    "description": "Well known asset issuer account"
  },
  {
    "name": "wallet",
    "description": "Shared account that belongs to a wallet"
  },
  {
    "name": "custodian",
    "description": "Reserved, custodian account, or cold wallet"
  },
  {
    "name": "malicious",
    "description": "Account involved in theft/scam/spam/phishing"
  },
  {
    "name": "unsafe",
    "description": "Obsolete or potentially dangerous account"
  },
  {
    "name": "personal",
    "description": "Personal signing key or account address"
  },
  {
    "name": "sdf",
    "description": "Account under the custody of SDF"
  },
  {
    "name": "memo-required",
    "description": "Destination requires transaction memo"
  },
  {
    "name": "airdrop",
    "description": "Airdrop distribution account"
  },
  {
    "name": "obsolete-inflation-pool",
    "description": "Inflation pool distribution account (obsolete)"
  }
]
```

#### `GET /directory` - list all entries

This API endpoint follows Stellar Horizon API response format convention.
A response result contains records and navigation links.

Optional query parameters:

- `address` *{Array\<String\>}* - filters results by addresses list (up to 50 per request)
- `tag` *{Array\<String\>}* - applies a filter by tags
- `search` *{String}* - applies a full-test search filter by entry address, name, or domain
- `cursor` *{String}* - address from which to continue search
(`paging_token` value from a results set)
- `order` *{String}* - `"asc"` or `"desc"`, controls results order
- `limit` *{Number}* - data page size (10 entries per page by default)

Example:

```
curl https://api.stellar.expert/explorer/directory?limit=2
```

```json
{
  "_links": {
    "self": {
      "href": "/explorer/directory?sort=address&order=asc&limit=2"
    },
    "prev": {
      "href": "/explorer/directory?sort=address&order=desc&cursor=GA272U6UMNKYOBDCLH5CDAPZVDG4WAGJWFY2S2RV2DN6YSSOGYECEVOF&limit=2"
    },
    "next": {
      "href": "/explorer/directory?sort=address&order=asc&cursor=GA2VRL65L3ZFEDDJ357RGI3MAOKPJZ2Z3IJTPSC24I4KDTNFSVEQURRA&limit=2"
    }
  },
  "_embedded": {
    "records": [
      {
        "address": "GA272U6UMNKYOBDCLH5CDAPZVDG4WAGJWFY2S2RV2DN6YSSOGYECEVOF",
        "paging_token": "GA272U6UMNKYOBDCLH5CDAPZVDG4WAGJWFY2S2RV2DN6YSSOGYECEVOF",
        "domain": "",
        "name": "SCAM ACCOUNT",
        "tags": [
          "malicious"
        ]
      },
      {
        "address": "GA2VRL65L3ZFEDDJ357RGI3MAOKPJZ2Z3IJTPSC24I4KDTNFSVEQURRA",
        "paging_token": "GA2VRL65L3ZFEDDJ357RGI3MAOKPJZ2Z3IJTPSC24I4KDTNFSVEQURRA",
        "domain": "stellar.org",
        "name": "SDF Escrow Jan 1 2023",
        "tags": [
          "sdf",
          "custodian"
        ]
      }
    ]
  }
}
```

More query examples:

- Fetch information about multiple accounts in one call:
  ```
  curl -g "https://api.stellar.expert/explorer/directory?address[]=GA6HCMBLTZS5VYYBCATRBRZ3BZJMAFUDKYYF6AH6MVCMGWMRDNSWJPIH&address[]=GAP5LETOV6YIE62YAM56STDANPRDO7ZFDBGSNHJQIYGGKSMOZAHOOS2S"
  ```
- Search for Kraken's deposit account  
  ```
  curl "https://api.stellar.expert/explorer/directory?search=kraken"
  ```
- Find accounts tagged as `malicious` or `unsafe`:
  ```
  curl -g "https://api.stellar.expert/explorer/directory?tag[]=malicious&tag[]=unsafe"
  ```
- Lookup addresses reported for "staking"-related scams:
  ```
  curl -g "https://api.stellar.expert/explorer/directory?tag[]=malicious&search=stacking"
  ```

#### `GET /directory/{address}` - lookup specific Stellar account

Parameters:
- `address` *{String}* - the address of the account to check

Example:

```
curl https://api.stellar.expert/explorer/directory/GA5XIGA5C7QTPTWXQHY6MCJRMTRZDOSHR6EFIBNDQTCQHG262N4GGKTM
```

```json
{
  "address": "GA5XIGA5C7QTPTWXQHY6MCJRMTRZDOSHR6EFIBNDQTCQHG262N4GGKTM",
  "name": "Kraken",
  "domain": "kraken.com",
  "tags": [
    "exchange",
    "memo-required"
  ]
}
```

### CORS, Rate Limiting and Caching Considerations

All described API endpoints have [CORS headers](https://enable-cors.org/index.html)
enabled, so they can be used directly from any website.

The effective API request rate is limited to 5 requests per second for each
individual IP. Please consider response caching or group queries on your side
in case of heavy utilization.

---

Explore all [open API endpoints](./index.md).
