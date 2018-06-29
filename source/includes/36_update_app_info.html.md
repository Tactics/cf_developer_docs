## Creating or updating the customization of an app

```graphql
mutation UpdateAppInfo
    ($vatnumber: String!, $imageUrl : String, $emailAddress: String, $badgeText: String, $badgeTextColor: String, $badgeColor: String, $iconType: String, $iconColor: String)
    { updateAppInfo
        (vatnumber: $vatnumber, image_url: $imageUrl, emailaddress: $emailAddress, badge: {text: $badgeText, text_color: $badgeTextColor, color: $badgeColor}, icon: {type: $iconType, color: $iconColor})
            {vatnumber, imageUrl, emailaddress, icon{type, color},badge{text,text_color,color}}
    }


```
```json
{ 
    "vatnumber": "0123123123",
    "imageUrl": "XXX", 
    "emailAddress": "test@clearfacts.be", 
    "badgeText": "NEW", 
    "badgeTextColor": "#d5a855", 
    "badgeColor": "red", 
    "iconType": "award", 
    "iconColor":"yellow"}
```

> will result in

```json
{
    "data": {
        "updateAppInfo": {
            "vatnumber": "0123123123",
            "imageUrl" : "XXX",
            "emailaddress": "test@clearfacts.be",
            "icon": {
                "type": "award",
                "color": "yellow"
            },
            "badge": {
                "text": "NEW",
                "textColor": "#d5a855",
                "color": "red"
            }
        }
    }
}
```