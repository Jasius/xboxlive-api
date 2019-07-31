# Xbox Live - API

Simple Xbox Live API wrapper.

### Installation
```
$ npm install @xboxreplay/xboxlive-api
```

### Clone
```
$ git clone https://github.com/XboxReplay/xboxlive-api.git
```

### Build
```
$ npm run build
```

### Test
```
$ npm run test
```

### Example usage

```
import XboxLiveAPI from '@xboxreplay/xboxlive-api';

XboxLiveAPI.getPlayerSettings('Zeny IC', {
    userHash: 'YOUR_OWN_USER_HASH',
    XSTSToken: 'YOUR_OWN_XSTS_TOKEN'
}, ['GameDisplayPicRaw', 'Gamerscore', 'Location'])
    .then(console.info)
    .catch(console.error);
```

**Sample response:**

```
[
    {
        "id": "GameDisplayPicRaw",
        "value": "http://images-eds.xboxlive.com/image?url=wHwbXKif8cus8csoZ03RWwcxuUQ9WVT6xh5XaeeZD02wEfGZeuD.XMoGFVYkwHDq4Ch7pcu9E3UwDqy.fzrTaviUvY1c8gvrWRzLTqFKUVap_Nvh0.Em2IsAWtHcMFeVpY2boMYiy03w887.tSGAT62Na2z3k33eMWnP12mY2x0-&format=png"
    }
    {
        "id": "Gamerscore",
        "value": "5610"
    },
    {
        "id": "Location",
        "value": "Paris, France"
    }
]
```

### How to generate a Xbox Live authorization?
The fastest way to generate a valid authorization is to use our [XboxLive-Auth](https://github.com/XboxReplay/xboxlive-auth) module which returns a **userHash** and a **XSTSToken** for a specified account.

### Available methods
**getPlayerXUID** - Returns targeted player's XUID:
```
getPlayerXUID(
    gamertag: string;
    authorization: {
        userHash: string;
        XSTSToken: string;
    };
): Promise<string>
```

**getPlayerSettings** - Returns targeted player's settings:
```
getPlayerSettings(
    gamertag: string;
    authorization: {
        userHash: string;
        XSTSToken: string;
    };
    settings?: [
        'GameDisplayPicRaw' |
        'Gamerscore' |
        'Gamertag' |
        'AccountTier' |
        'XboxOneRep' |
        'PreferredColor' |
        'RealName' |
        'Bio' |
        'Location'
    ];
): Promise<{
    id: string;
    value: string;
}[]>
```

**getPlayerScreenshots** - Returns targeted player's screenshots:
```
getPlayerScreenshots(
    gamertag: string, // Or a valid XUID
    authorization: {
        userHash: string;
        XSTSToken: string;
    },
    qs: {
        maxItems?: number = 25
        continuationToken?: string
    }
): Promise<{
    screenshots: {
        screenshotId: string;
    	resolutionHeight: number;
    	resolutionWidth: number;
    	state: string;
    	datePublished: string;
    	dateTaken: string;
    	lastModified: string;
    	userCaption: string;
    	type: 'UserGenerated' | 'AutoGenerated';
    	scid: string;
    	titleId: number;
    	rating: number;
    	ratingCount: number;
    	views: number;
    	titleData: string;
    	systemProperties: string;
    	savedByUser: boolean;
    	achievementId: string;
    	greatestMomentId: string | null;
    	thumbnails: {
            uri: string;
            fileSize: 0;
            thumbnailType: 'Small' | 'Large';
	    }[];
    	screenshotUris: {
            uri: string;
            fileSize: number;
            uriType: 'Download';
            expiration: string;
	    }[];
    	xuid: string;
    	screenshotName: string;
    	titleName: string;
    	screenshotLocale: string;
    	screenshotContentAttributes: string;
    	deviceType: string;
    }[];
    pagingInfo: {
        continuationToken: string | null
    };
}>
```

**getPlayerGameclips** - Returns targeted player's gameclips:
```
getPlayerGameclips(
    gamertag: string, // Or a valid XUID
    authorization: {
        userHash: string,
        XSTSToken: string
    },
    qs: {
        maxItems?: number = 25
        continuationToken?: string
    }
): Promise<{
    gameClips: {
        gameClipId: string;
        state: string;
        datePublished: string;
        dateRecorded: string;
        lastModified: string;
        userCaption: string;
        type: 'UserGenerated' | 'AutoGenerated';
        durationInSeconds: number;
        scid: string;
        titleId: number;
        rating: number;
        ratingCount: number;
        views: number;
        titleData: string;
        systemProperties: string;
        savedByUser: boolean;
        achievementId: string;
        greatestMomentId: string | null;
        thumbnails: {
            uri: string;
            fileSize: 0;
            thumbnailType: 'Small' | 'Large';
	    }[];
    	gameClipUris: {
            uri: string;
            fileSize: number;
            uriType: 'Download';
            expiration: string;
	    }[];
        xuid: string;
        clipName: string;
        titleName: string;
        gameClipLocale: string;
        clipContentAttributes: string;
        deviceType: string;
        commentCount: number;
        likeCount: number;
        shareCount: number;
        partialViews: number;
    }[];
    pagingInfo: {
        continuationToken: string | null
    };
}>
```

**call** - Generic method to call the API with a custom URI:
```
call(
    uri: string;
    authorization: {
        userHash: string;
        XSTSToken: string;
    };
    options?: {
        method?: string; // HTTP method
        payload?: any; // Body payload for PATCH | POST | PUT requests
        qs?: any; // Query String
    }
): Promise<any>
```

### Should I use XUIDs instead of Gamertags?
`getPlayerGameclips` and `getPlayerScreenshots` methods resolve player's XUID thanks to the specified gamertag which requires an additional request to be made internally (`getPlayerXUID`). If a valid XUID is used instead (during pagination for instance), this may speed up your request.

### Where can I find additional Xbox Live API URIs?
Please refer to https://docs.microsoft.com/en-us/windows/uwp/xbox-live/xbox-live-rest/uri/atoc-xboxlivews-reference-uris.
