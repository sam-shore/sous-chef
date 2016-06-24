# Menus To Go

Menus To Go is our private API for polling menu information for a particular venue.

## Architecture

TouchBistro is unique in the mobile point-of-sale space. We've taken what some of us call a "cloud last" approach to our architecture. Our client (iPad-based) apps have been built to function entirely without the cloud. The apps will of course upload data to the cloud when possible for reports and other features, but our core app doesn't rely on the internet. That's proven very appealing to our customer.

To achieve this, we have a small packaged server running inside a desktop application we deploy to hardware within a venue which runs on a local network. This server acts as a central hub for data between iPads across the venue. To that end, there are a number of end-points which this server exposes that can be leveraged in some novel ways.

![Napkin Architecture](https://github.com/TouchBistro/sous-chef/blob/master/assets/arch.png?raw=true)

One such way is for our self-serve app, which leverages one of the server's end-points to fetch menus from the POS on a network shared by the self-serve app.

## Request

```GET http://<ip>:<port>/getmenuformenuapp```

### Params

* `<ip>`: The local network IP for your TouchBistro server
* `<port>`: 1337. Yea, we know.

## Response

```
{
  "ipAddress": "172.16.4.230",
  "isDetailedMenuItemCard": 0,
  "isLockEnabled": 0,
  "isOrderModeEnabled": 0,
  "maximumItemsInCart": 0,
  "menuCategories": [{
    "menuItems": [{
      "imagePath": "",
      "itemDescription": "",
      "name": "Bud Light",
      "price": "7.08",
      "uuid": "150101000000~C2B28120-C4AC-40BA-91C7-504BBC94FA8A"
    }],
    "name": "Beer",
    "uuid": "150101000000~06949941-AD33-4A3C-BFFB-E2F84B36DAAF"
  }]
}
```

### Params

#### Root

Attribute | Description | Type
----- | ----- | -----
ipAddress | An echo back of the server's IP address | string
isDetailedMenuItemCard | Indicates whether or not the menu item requested is available for the detailed view | bool
isLockEnabled | Indicates whether or not the menu item is locked | bool
isOrderModeEnabled | Indicates whether or not the POS has been configured to allow self ordering | bool
maximumItemsInCart | Limit of how many items can be added to a self-order cart | int
menuCategories | An collection of categories, each containing n menuItem objects | array
menuItems | A collection of menu item objects | array

#### Menu Item

Attribute | Description | Type
----- | -----  | -----
imagePath | The absolute path to the image associated with the menu item. | string
itemDescription | The description of the menu item | string
itemAllergens | Comma-seperated allergens | string
itemIngredients | Comma-seperated ingredients | string
name | The item name | string
price | The item price in dollars. 0 if not priced. | double
uuid | The unique identifier for the menu item in the client-side data model | string

## Usage

In order to use this API, you need only write a small web or desktop app that can connect on the local network to the TouchBistro server app. That app can then poll the server directly via the above end-point. There is no encryption and no authentication required.

# Index

Check out our other documentation in the [index](https://github.com/TouchBistro/sous-chef/blob/master/README.md).
