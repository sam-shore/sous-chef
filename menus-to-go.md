# Menus To Go

Menus To Go is our private API for polling menu information for a particular venue.

## Architecture

TouchBistro is unique in the mobile point-of-sale space. We've taken what some of us call a "cloud last" approach to our architecture. Our client (iPad-based) apps have been built to function entirely without the cloud. The apps will of course upload data to the cloud when it can for reports and other features, but our core app doesn't rely on the internet. That's proven very appealing to our customer.

To achieve this, we have a small packaged server running inside a desktop application we deploy to hardware within a venue which runs on a local network. This server acts as a central hub for data between iPads across the venue. To that end, there are a number of end-points which this server exposes that can be leveraged in some novel ways.

![Napkin Architecture](https://github.com/TouchBistro/sous-chef/blob/master/assets/arch.png?raw=true)

One such way is for our self-serve app, which leverages one of the server's end-points to fetch menus from the POS on a network shared by the self-serve app.

## Request

```GET http://<ip>:<port>/getmenuformenuapp```

* `<ip>`: The local network IP for your TouchBistro server
* `<port>`: 1337. Yea, we know.

## Response

```
{
  ipAddress = "172.16.4.230";
  isDetailedMenuItemCard = 0;
  isLockEnabled = 0;
  isOrderModeEnabled = 0;
  maximumItemsInCart = 0;
  menuCategories = ({
    menuItems =             (
                                {
                    imagePath = "";
                    itemDescription = "";
                    name = "Bud Light";
                    price = "7.08";
                    uuid = "150101000000~C2B28120-C4AC-40BA-91C7-504BBC94FA8A";
                },...
            );
            name = Beer;
            uuid = "150101000000~06949941-AD33-4A3C-BFFB-E2F84B36DAAF";
        },...
    });
}
```

# Index

Check out our other documentation in the [index](https://github.com/TouchBistro/sous-chef/blob/master/README.md).
