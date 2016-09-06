# REST API

The following documentation is designed to help you connect with the REST API. There are a number of different end-points available internally and one publicly at this time.

## Contents

Endpoint | Description
--- | --- |
[Menu](#menu) | Retrieve menu metadata, including menu item names and images
[Bills](#bills) | Retrieve closed bills metadata, including sales amounts

## <a name="menu"></a>Menu

### Authentication

This API is publically accessible once enabled for a given restaurant.

### Request

```GET https://cloud.touchbistro.com/api/v1/patron/restaurant/<id>/menu```

#### Params

* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.

### Response

```
{
	"restaurant_name": "Frank's Sangwiches",
	"menu": {
		"name": "Default",
		"menu_pages": [{
			"name": "Lunch",
			"menu_categories": [{
				"name": "Desserts",
				"menu_items": [{
					"name": "Nutella Strawberry Banana Crepe",
					"desc": "100% pure maple syrup and brown sugar tossed with fresh strawberries and bananas and tucked inside a crepe lined with Nutella. Choose to add ice cream and whipped cream on top.",
					"price": "6.99",
					"thumbnail_image_url": "",
					"image_url": ""
				}]
			}]
		}]
	}
}
```

#### Attributes

##### Root

Attribute | Description | Type
----- | ----- | -----
restaurant_name | The display name of the restuarant | string
menus | The collection of menus for the restaurant | menus

##### Menu

Attribute | Description | Type
----- | -----  | -----
name | The menu name | string
menu_pages | The collection of menu pages for that menu | menu_pages

##### Menu Pages

Attribute | Description | Type
----- | -----  | -----
name | The menu page name | string
menu_categories | The categories of the menu page | menu_categories

##### Menu Categories

Attribute | Description | Type
----- | -----  | -----
name | The menu category name | string
menu_items | The items in that menu category | menu_items

##### Menu Item

Attribute | Description | Type
----- | -----  | -----
name | The item name | string
desc | The description of the menu item | string
price | The item price in dollars. 0 if not priced. | double
thumbnail_image_url | The absolute path to the thumbnail image associated with the menu item. | string
image_url | The absolute path to the image associated with the menu item. | string
item_allergens | Comma-seperated allergens (coming soon) | string
item_ingredients | Comma-seperated ingredients (coming soon) | string

## <a name="bills"></a>Bills

### Authentication

This API requires an authentication token which is passed in as a query parameter. 

**Note**: This approach to security is only a stop-gap measure for interim usage and will be replaced by a proper obfuscated authentication header in the future.

### Request

```GET https://cloud.touchbistro.com/api/v1/bills?restaurant_id=<id>&from=<datetime>&to=<datetime>&authentication_token=<api_token>```

#### Params

* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.
* `<datetime>`: A datetime stamp in the ISO 8601 format (e.g., 2016-03-14T16:43:22Z)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.

### Response

```
{  
   "bills":[  
      {  
         "id":41995628,
         "uuid":"12B921EE-1B06-41E6-9A91-F060D17D5E18",
         "order_number":5867,
         "bill_number":9234,
         "tax_1":"9.87",
         "tax_2":"0.0",
         "paid_at":"2016-06-06T17:24:49.000Z",
         "paid_at_local":"2016-06-06T13:24:49.000Z",
         "order_items":[  
            {  
               "uuid":"276FF8A5-77BC-49A0-AD28-D461EB56D6FE",
               "simple_modifier":false,
               "total":"6.99"
            },
            {  
               "uuid":"2899268A-4BC0-4701-B026-C11A5D15A645",
               "simple_modifier":false,
               "total":"22.99"
            },
            {  
               "uuid":"BCB39733-3A95-467B-B3F1-92BD84D60DC0",
               "simple_modifier":false,
               "total":"18.99"
            },
            {  
               "uuid":"AAD9393D-7FD7-4C5A-9BF7-C6A9084A4019",
               "simple_modifier":false,
               "total":"8.99"
            },
            {  
               "uuid":"C6DCD5FC-C14F-43FA-BCD5-0CD01991F05F",
               "simple_modifier":false,
               "total":"9.99"
            },
            {  
               "uuid":"608A0CF4-9847-4CC1-BC33-9303C662BE9D",
               "simple_modifier":false,
               "total":"7.99"
            }
         ],
         "payments":[  
            {  
               "payment_method":"Cash",
               "total":"85.81",
               "tip":"0.0",
               "change":"0.0"
            }
         ],
         "total":"85.809999999999999",
         "sub_total":"75.94",
         "taxes":9.87,
         "universal_identifier":"10210-20160606-9234-5E18"
      }]
}
```

#### Attributes

##### Bills

Attribute | Description | Type
----- | ----- | -----
bills | The list of bills | bill

##### Bill

Attribute | Description | Type
----- | ----- | -----
id | A global identifer for the bill | big int
uuid | A unique identifer for the bill | uuid
order_number | A friendly order number identifier | small int
bill_number | A friendly bill number identifier | small int
paid_at | A datetime stamp indicating when the bill was paid with the payment processor | datetime
paid_at_local | A datetime stamp indicating when the bill was paid on the iPad | datetime
order_items | An array of items on the bill | order_item
payments | An array of payment information | payment
total | The total amount of the bill | float
sub_total | The sub-total of the bill before taxes | float
taxes | The tax amount for the bill | float
universal_identifier | The unique identifier for the bill between cloud and iOS | uid


# Index

Check out our other documentation in the [index](https://github.com/TouchBistro/sous-chef/blob/master/README.md).
