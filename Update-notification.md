When an update is available for any plugin, the system will notify the user that it is available for at least one of the modules that is currently installed. As part of this experience, the user can choose to see the details of the release. These details should display a summarized aggregated list the release notes. 

At first this may seem like a trial task, but the plugin model provided by Glimpse means each module may be versioned independently, originate from different sources and the user may be many releases behind. Hence, the system needs to deal with these complexities.

## Notification Models
The following models are what the web client expects to receive when making requests against the Update Notification Endpoints. This is designed to be a formal definition and from this point on, data type described by [] will be defined throughout this section. Other "simple" data type will assume that the reader is able to derive the values via the names and examples.

### [Update Notification]

Definition:
```js 
{
    ("name" : alphanumeric)?,
    ("channel" : alphanumeric)?,
    ("version" : versionNumber),
    ("description" : alphanumeric)?, 
    ("items" : [
        [Update Item]
    ])?,
    ("features" : [
        [Update Feature]
    ])?
}

/* [Update Feature] */
{
    ("name" : alphanumeric),
    ("icon" : url),
    ("description" : alphanumeric),
    ("items" : [
        [Update Item]
    ])
}

/* [Update Item] */
{
    ("category" : ("New" | "Changed" | "Fixed" | "Developer"),
    ("summary" : alphanumeric),
    ("taskId" : alphanumeric),
    ("taskLink" : url),
    ("importance" : (1 | 2 | 3)
}
```

Remarks:
 - `name` - Short text associated with the release
 - `channel` - Defines the channel that the release might belong to (i.e. Alpha, Beta, Production)
 - `version` - Actual version number of this release
 - `description` - General text that describes the release (supports basic MarkDown syntax - i.e. bold, italic, links)
 - `items` - General items that are loosely associated with the release, or not a part of any defined feature 
     - `category` - What class of entry the item is
         - `New` - Represents a new feature or functionality
         - `Changed` - Represents an update to existing functionality
         - `Fixed` - Represents an update that fixes a 
         - `Developer` - Represents an API change that plguin/provider developers need to know about
     - `summary` - Description of item (supports basic MarkDown syntax - i.e. bold, italic, links)
     - `taskId` - If there is a id that is associated with the item
     - `taskLink` - Link that provides more details of the fix
     - `importance` - How important the release is (1 being most important and 3 least important)
 - `features` - Groups that items can be associated with
     - `name` - Name of feature group
     - `icon` - Url of the icon that represents the group
     - `description` - Description of feature (supports basic MarkDown syntax - i.e. bold, italic, links)
     - `items` - Update items that are associated with the feature in this release

Sample:
```js
{
    name : "Glimpse Core",
    channel : "Beta",
    version : "1.1.2",
    features : [
        { 
            name : "System",
            icon : "http://getglimpse.com/release/icon/core.png",
            items : [
                {
                    category : "New",
                    summary : "*Release Checker*: Now gives you a breakdown of exactly what you are missing from the latest version.",
                },
                {
                    category : "New",
                    summary : "*Structured Layout*: An alternative layout engine that allows developers to control the layout of the data.",
                }
            ]
        },
        { 
            name : "Plugin",
            icon : "http://getglimpse.com/release/icon/mvc.png",
            items : [
                {
                    category : "Changed",
                    summary : "*Timeline*: Comes with an additional grid view to show the same data.",
                },
                {
                    category : "Fixed",
                    summary : "*Ajax*: Fix that crashed poll in Chrome and IE due to log/trace statement.",
                }
            ]
        },
    ]
}
```