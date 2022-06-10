# Āto License Generator

Create intellectual property licenses for all your NFTs.

## Overview

The [`ato-api-form.html`](https://github.com/ATO-nft/api-client/blob/main/ato-api-form.html) page includes a form displaying parameters to set. When the form is submitted, the API sends an intellectual property license (pdf file) in response (in English and French). This file can then be stored and added to the NFT metadata.

## Test

Go to the following page and click on the `Download License` button:

[https://ato.works/ato-api-form.html](https://ato.works/ato-api-form.html)

## Integration

There is no access restriction on the demo version.

For production use, please contact us to get your API key ([Discord](https://discord.gg/2sFr3dqvfg) or [email](mailto:julien@ato.network)), and follow our [integration guide](https://ato.works/guide/).

## Change log

#### v0.9.4-beta

- Added base 64 API key
- Added external JS file
- Added multi-user mode
- Added UTF-8 support

#### v0.9.3-beta

- Added API key
- Added payment Solidity contract (Rinkeby)

#### v0.9.0-beta

- Initial version

# Integration guide

## Use the live demo

Go to the following page and click on the `Download License` button:

[https://ato.works/ato-api-form.html](https://ato.works/ato-api-form.html)

## Integrate in demo mode

Add this JS file in your project: [https://github.com/ATO-nft/api-client/blob/main/ato-api-form.js](https://github.com/ATO-nft/api-client/blob/main/ato-api-form.js)

Here's an example of a json object to send to the server (placeholder values):

```json
JSONValues = {
  "ArtworkName": "NFT #1",
  "NftStandard": "ERC-721",
  "NftLicence": "Art",
  "ArtworkDesc": "This is a demo.",
  "FileName": "artwork.png",
  "FileType": "png",
  "FileSize": "24000",
  "CreatorName": "Francis",
  "CreatorAddress": "0x0000000000000000000000000000000000000000",
  "ListNetwork": "Polygon",
  "RightLevel": "3",
  "RightDuration": "70",
  "SupplyNumber": "1",
  "RightExclusive": "on",
  "NonReissuance": "on",
  "ResaleRight": "8",
  "userKey": "",
  "VersionApi": "v1.0"
}
```

#### Expected values

###### Artwork

| Name        | Type   | Description                      | Example        |
| :---------- | :----- | :------------------------------- | :------------- |
| ArtworkName | String | min : 1 <br/> max : 32           | NFT #1         |
| NftStandard | select | ERC-721 <br/> ERC-1155           | ERC-721        |
| NftLicence  | select | Art <br/> Gaming <br/> Redeemale | Art            |
| ArtworkDesc | String | min : 1 <br/> max : 512          | This is a demo |

###### Image

| Name     | Type   | Description            | Example     |
| :------- | :----- | :--------------------- | :---------- |
| FileName | String | min : 1 <br/> max : 32 | artwork.png |
| FileType | String | min : 1 <br/>max : 10  | png         |
| FileSize | Number | max : 40000000         | 24000       |

###### Artist

| Name           | Type   | Description                          | Example                                    |
| :------------- | :----- | :----------------------------------- | :----------------------------------------- |
| CreatorName    | String | min : 1 <br/> max : 32               | Francis                                    |
| CreatorAddress | String | min : 42 <br/> max : 42              | 0x0000000000000000000000000000000000000000 |
| ListNetwork    | select | Polygon <br/> Ethereum <br/> Rinkeby | Rinkeby                                    |

###### Rights

| Name              | Type     | Description             | Example |
| :---------------- | :------- | :---------------------- | :------ |
| RightLevel        | Number   | min : 1 <br/> max : 3   | 3       |
| RightAdapt \*     | checkbox | on <br/> undefined      |         |
| RightLogo \*      | checkbox | on <br/> undefined      |         |
| RightMerch \*     | checkbox | on <br/> undefined      |         |
| RightDuration     | select   | 1...70                  | 70      |
| SupplyNumber      | Number   | min : 1 <br/> max : 150 | 1       |
| RightExclusive \* | checkbox | on <br/> undefined      | on      |
| NonReissuance \*  | checkbox | on <br/> undefined      | on      |
| ResaleRight       | select   | 1...10                  | 8       |

###### Pro version

| Name    | Type   | Description             | Example            |
| :------ | :----- | :---------------------- | :----------------- |
| userKey | String | min : 42 <br/> max : 42 | empty if demo mode |

###### Misc

| Name       | Type   | Description | Example |
| :--------- | :----- | :---------- | :------ |
| VersionApi | select | v1.0        | v1.0    |

\* optional

### JS example

In your app, add the following script:

```javascript
httpRequest.open("POST", "http://ato-api.jcloud-ver-jpe.ik-server.com/"); // or https

let data = new FormData();
data.append("json", JSONValues);
data.append(
  "access",
  JSON.stringify({ user_id: "", pass: "", userNetwork: "Rinkeby", api: "v1.0" })
);
httpRequest.setRequestHeader(
  "Content-Type",
  "multipart/form-data; boundary=" + data._boundary
);
httpRequest.responseType = "arraybuffer";
httpRequest.send(data);
```

You will get a pdf file in response.

You may want to use this script to handle the response:

```javascript
var fileName = httpRequest
  .getResponseHeader("Content-Disposition")
  .split("filename=")[1]
  .replaceAll('"', "");
var blob = new Blob([httpRequest.response], { type: "application/pdf" });
```

Then, save the pdf file on your own machine:

```javascript
let url = window.URL.createObjectURL(blob);
let a = document.getElementById("downloadLink");
a.href = url;
if (fileName === undefined || fileName === "") {
  fileName = "IP_License.pdf";
}
a.download = fileName;
a.click();
window.URL.revokeObjectURL(url);
```

If the pdf file is blank, please check the UTF-8 convertion.

## Integrate in pro mode (multi-user)

- [Contact us directly](https://github.com/ATO-nft/api-client#support), we will send you your API key.
- Send Rinkeby ETH to this address: `0xf1eeb16879a7ecbda10675dcdebeb27db96f1b87` (testnet)
- Add the [api-server](https://github.com/ATO-nft/api-core/blob/main/api-server.js) file in your Nodejs server
- Adjust the server port line 78 (default is 8080)
- Run the server: `node api-server.js`

Here's what your access object should look like:

```javascript
access : {
   "user_id":"0x0000000000000000000000000000000000000000",
   "pass":"jjr0UrM0Sm53qGqJqHQDN/val1FBhXhfFAXUC93rG4bW5MTrmfHVO7FRhv/i8C+HNi/yOhZaaucC+EdLpavYfc9VZMbny/mz5pOwT9Q4njk7WrN9tixLrnK+KB/hn/jjkRgJ3vB0zAKtVUmk2OZb3XxBymOTDwlGoNcV2MHVva/uMEJVeJR6qCgPfIBhbOTTM6dzFlEzjIHyZrrEpTyUcHAihbJSOQbnDDJ8Bp5T8WjLUNkVAEy59sBGr5qYXcUiU9vPVJGZ2/vjelXqAO41DCblOgn55bg4z4868ZA9OVHmFh3yY2N3iVEu7gEKfpFr00AF/vQQEsJrI31A9Tu7OQ==",
   "userNetwork":"Rinkeby",
   "api":"v1.0"
}
```

# Fields

- `ArtworkName`: the name of the NFT
- `FileName`: the name of the media file linked with the NFT
- `FileType`: the extension of the media file
- `FileSize`: the size of of the media file
- `CreatorName`: the name of the NFT creator
- `CreatorAddress`: the Ethereum adddress of the NFT creator
- `ListNetwork`: the network to which the NFT is published
- `RightLevel`: private use, public use limited to resale, or public use for all purposes
- `RightAdapt`: right to adapt (bool)
- `RightLogo`: right to add a logo (bool)
- `RightMerch`: merchandising right (bool)
- `RightDuration`: duration of the rights
- `SupplyNumber`: number of editions of the NFT
- `RightExclusive`: exclusivity of
- `NonReissuance`: non-reissuance guarantee (bool)
- `ResaleRight`: percentage of resale right
- `VersionApi`: version of the API
- `NftLicense`: the type of license (art, gaming or redeemable)
- `NftStandard`: NFT standard (ERC-721 or ERC-1155)

# Edit the docs

[https://retype.com/guides/cli/](https://retype.com/guides/cli/)

- `retype watch`
- `retype init`
- `retype build`
- `retype run`

## Support

- Email: [julien@ato.network](mailto:julien@ato.network)
- Discord: [https://discord.gg/2sFr3dqvfg](https://discord.gg/2sFr3dqvfg)
- Twitter: [https://twitter.com/julienbrg](https://twitter.com/julienbrg)
