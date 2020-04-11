# Xbox Live - API

Simple Xbox Live API wrapper.

### Installation
```shell
$ npm install @xboxreplay/xboxlive-api
```

### Example usage

```javascript
import XboxLiveAPI from '@xboxreplay/xboxlive-api';

XboxLiveAPI.getPlayerSettings('Zeny IC', {
    userHash: 'YOUR_OWN_USER_HASH',
    XSTSToken: 'YOUR_OWN_XSTS_TOKEN'
}, ['UniqueModernGamertag', 'GameDisplayPicRaw', 'Gamerscore', 'Location'])
    .then(console.info)
    .catch(console.error);
```

**Sample response:**

```javascript
[
    {
        "id": "UniqueModernGamertag",
        "value": "Zeny IC"
    },
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
The fastest way to generate a valid authorization is to use our [XboxLive-Auth](https://github.com/XboxReplay/xboxlive-auth) module which returns an **userHash** and a **XSTSToken** for a specified account.

### Available methods
**getPlayerXUID** - Returns targeted player's XUID:

```javascript
getPlayerXUID(
    gamertag: string;
    authorization: {
        userHash: string;
        XSTSToken: string;
    };
): Promise<string>
```

**getPlayerSettings** - Returns targeted player's settings:

```javascript
getPlayerSettings(
    gamertag: string;
    authorization: {
        userHash: string;
        XSTSToken: string;
    };
    settings?: [
        | 'GameDisplayPicRaw'
        | 'Gamerscore'
        | 'Gamertag'
        | 'AccountTier'
        | 'XboxOneRep'
        | 'PreferredColor'
        | 'RealName'
        | 'Bio'
        | 'Location'
        | 'ModernGamertag'
        | 'ModernGamertagSuffix'
        | 'UniqueModernGamertag'
        | 'RealNameOverride'
        | 'TenureLevel'
        | 'Watermarks'
        | 'IsQuarantined'
    ];
): Promise<{
    id: | 'GameDisplayPicRaw'
        | 'Gamerscore'
        | 'Gamertag'
        | 'AccountTier'
        | 'XboxOneRep'
        | 'PreferredColor'
        | 'RealName'
        | 'Bio'
        | 'Location'
        | 'ModernGamertag'
        | 'ModernGamertagSuffix'
        | 'UniqueModernGamertag'
        | 'RealNameOverride'
        | 'TenureLevel'
        | 'Watermarks'
        | 'IsQuarantined';
    value: string;
}[]>
```

**getPlayerScreenshots** - Returns targeted player's screenshots:<br>
<sub>**Warning:** Recent games *(since mid-2019)* will not be returned, please use **getPlayerScreenshotsFromActivityHistory** instead (if required).</sub>

```javascript
getPlayerScreenshots(
    gamertag: string; // Or a valid XUID
    authorization: {
        userHash: string;
        XSTSToken: string;
    };
    qs: {
        maxItems?: number; // Default: 25
        continuationToken?: string
    };
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

**getPlayerScreenshotsFromActivityHistory** - Returns targeted player's screenshots from its activity history:<br>
<sub>**Warning:** Returned items count may not respect the specified `numItems` parameter.</sub>

```javascript
getPlayerScreenshotsFromActivityHistory(
    gamertag: string; // Or a valid XUID
    authorization: {
        userHash: string;
        XSTSToken: string;
    };
    qs: {
        numItems?: number;
        contToken?: string;
        pollingToken?: string;
        startDate?: string;
    };
): Promise<{
    numItems: number;
    activityItems: {
        screenshotId: string;
        screenshotThumbnail: string;
        screenshotScid: string;
        screenshotName: string;
        screenshotUri: string;
        viewCount: number;
        gameMediaContentLocators: [
            {
                Expiration: string;
                FileSize: number;
                LocatorType: 'Download';
                Uri: string;
            },
            {
                Expiration: string;
                FileSize: number;
                LocatorType: 'Thumbnail_Small';
                Uri: string;
            },
            {
                Expiration: string;
                FileSize: number;
                LocatorType: 'Thumbnail_Large';
                Uri: string;
            }
        ];
        contentImageUri: string;
        contentTitle: string;
        platform: string;
        titleId: string;
        uploadTitleId: string;
        activity: {
            screenshotThumbLarge: null;
            screenshotThumbSmall: null;
            screenshotType: null;
            savedByUser: boolean;
            screenshotScid: string;
            screenshotId: string;
            numShares: number;
            numLikes: number;
            numComments: number;
            ugcCaption: string | null;
            authorType: string;
            activityItemType: 'Screenshot';
            userXuid: string;
            date: string;
            contentType: 'Game';
            titleId: string;
            platform: string;
            sandboxid: string;
            userKey: string | null;
            scid: string;
        };
        userImageUriMd: string;
        userImageUriXs: string;
        description: string;
        date: string;
        hasUgc: boolean;
        activityItemType: 'Screenshot';
        contentType: 'Game';
        shortDescription: string;
        itemText: string;
        itemImage: string;
        shareRoot: string;
        feedItemId: string;
        itemRoot: string;
        hasLiked: boolean;
        authorInfo: {
            name: string;
            secondName: string;
            imageUrl: string;
            authorType: string;
            id: string;
        };
        gamertag: string;
        realName: string;
        displayName: string;
        userImageUri: string;
        userXuid: string;
    }[];
    pollingToken: string;
    pollingIntervalSeconds: string | null;
    contToken: string;
}>
```

**getPlayerGameClips** - Returns targeted player's clips:<br>
<sub>**Warning:** Recent games *(since mid-2019)* will not be returned, please use **getPlayerGameClipsFromActivityHistory** instead (if required).</sub>

```javascript
getPlayerGameClips(
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

**getPlayerGameClipsFromActivityHistory** - Returns targeted player's clips from its activity history:<br>
<sub>**Warning:** Returned items count may not respect the specified `numItems` parameter.</sub>

```javascript
getPlayerGameClipsFromActivityHistory(
    gamertag: string; // Or a valid XUID
    authorization: {
        userHash: string;
        XSTSToken: string;
    };
    qs: {
        numItems?: number;
        contToken?: string;
        pollingToken?: string;
        startDate?: string;
    };
): Promise<{
    numItems: number;
    activityItems: {
        clipId: string;
        clipThumbnail: string;
        downloadUri: string;
        clipName: string;
        clipCaption: string;
        clipScid: string;
        dateRecorded: string;
        viewCount: number;
        gameMediaContentLocators: [
            {
                Expiration: string;
                FileSize: number;
                LocatorType: 'Download';
                Uri: string;
            },
            {
                Expiration: string;
                FileSize: number;
                LocatorType: 'Thumbnail_Small';
                Uri: string;
            },
            {
                Expiration: string;
                FileSize: number;
                LocatorType: 'Thumbnail_Large';
                Uri: string;
            }
        ];
        contentImageUri: string;
        contentTitle: string;
        platform: string;
        titleId: string;
        uploadTitleId: string;
        activity: {
            dateRecorded: string;
            numShares: number;
            numLikes: number;
            numComments: number;
            ugcCaption: string | null;
            authorType: string;
            clipId: string;
            clipName: string | null;
            activityItemType: 'GameDVR';
            clipScid: string;
            userXuid: string;
            clipImage: string | null;
            clipType: string | null;
            clipCaption: string | null;
            savedByUser: boolean;
            date: string;
            sharedSourceUser: number;
            contentType: 'Game';
            titleId: string;
            platform: string;
            sandboxid: string;
            userKey: string | null;
            scid: string;
        };
        userImageUriMd: string;
        userImageUriXs: string;
        description: string;
        date: string;
        hasUgc: boolean;
        activityItemType: 'GameDVR';
        contentType: 'Game';
        shortDescription: string;
        itemText: string;
        itemImage: string;
        shareRoot: string;
        feedItemId: string;
        itemRoot: string;
        hasLiked: boolean;
        authorInfo: {
            name: string;
            secondName: string;
            imageUrl: string;
            authorType: string;
            id: string;
        };
        gamertag: string;
        realName: string;
        displayName: string;
        userImageUri: string;
        userXuid: string;
    }[];
    pollingToken: string;
    pollingIntervalSeconds: string | null;
    contToken: string;
}>
```

**getPlayerActivityHistory** - Returns targeted player's activity history:<br>
<sub>**Warning:** Returned items count may not respect the specified `numItems` parameter.</sub>

```javascript
getPlayerActivityHistory(
    gamertag: string;
    authorization: {
        userHash: string;
        XSTSToken: string;
    };
    qs?: {
        numItems?: number;
        contToken?: string;
        pollingToken?: string;
        activityTypes?: 'GameDVR' | 'Screenshot' | 'Achievement' | 'Played';
        excludeTypes?: 'GameDVR' | 'Screenshot' | 'Achievement' | 'Played';
        contentTypes?: 'Game' | 'App';
        startDate?: string;
    };
): Promise<{
    numItems: number;
    activityItems: any[];
    pollingToken: string;
    pollingIntervalSeconds: string | null;
    contToken: string;
}>
```

**call** - Generic method to call the API with a custom configuration:
```javascript
call(
    config: {
        uri: string;
        method: GET | PUT | POST | PATCH | DELETE,
        params: any // querystring
        ... // Please refer to https://github.com/axios/axios#request-config for further information
    };
    authorization: {
        userHash: string;
        XSTSToken: string;
    };
    XBLContractVersion?: number; // Default: 2
): Promise<any>
```

### Should I use XUIDs instead of Gamertags?
Some of exposed methods resolve player's XUID thanks to the specified gamertag which requires an additional request to be made internally (`getPlayerXUID`). If a valid XUID is used instead (during pagination for instance), this may speed up your request.

### Where can I find additional Xbox Live API URIs?
Please refer to https://docs.microsoft.com/en-us/windows/uwp/xbox-live/xbox-live-rest/uri/atoc-xboxlivews-reference-uris.
