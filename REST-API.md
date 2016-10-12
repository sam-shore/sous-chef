# REST API

The following documentation is designed to help you connect with the REST API. Each section defines an endpoint, the request parameters, and the response attributes.

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

## <a name="Shifts"></a>Shifts

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request

```GET https://cloud.touchbistro.com/cloud/shifts?restaurant_id=<id>&from=<datetime>&to=<datetime>&authentication_token=<api_token>```

#### Params

* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.
* `<datetime>`: A datetime stamp in the ISO 8601 format (e.g., 2016-03-14T16:43:22Z)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.

### Response

```
[{
	"waiter_name": "Alex",
	"shift_type_name": "General Manager",
	"started_at_local": "2016-09-07T15:09:28.000+00:00",
	"finished_at_local": "2016-09-07T15:10:14.000+00:00",
	"duration_in_hours": 0.012777,
	"rate_of_pay": 15.75,
	"labor_cost": 0.2
}, {
	"waiter_name": "Alex",
	"shift_type_name": "General Manager",
	"started_at_local": "2016-09-07T15:11:48.000+00:00",
	"finished_at_local": "2016-09-07T15:12:00.000+00:00",
	"duration_in_hours": 0.003333,
	"rate_of_pay": 15.75,
	"labor_cost": 0.05
}, {
	"waiter_name": "Alex",
	"shift_type_name": "General Manager",
	"started_at_local": "2016-09-07T15:12:29.000+00:00",
	"finished_at_local": "2016-09-13T16:39:25.000+00:00",
	"duration_in_hours": 145.448888,
	"rate_of_pay": 15.75,
	"labor_cost": 2290.82
}, {
	"waiter_name": "Alex",
	"shift_type_name": "General Manager",
	"started_at_local": "2016-09-20T12:14:26.000+00:00",
	"finished_at_local": "2016-09-20T12:47:17.000+00:00",
	"duration_in_hours": 0.5475,
	"rate_of_pay": 15.75,
	"labor_cost": 8.62
}]
```

#### Attributes

##### Shifts

Attribute | Description | Type
----- | ----- | -----
Shifts | The list of shifts | shift

##### shift

Attribute | Description | Type
----- | ----- | -----
waiter_name | The waiter's name | string
shift_type_name | The role or staff type for the shift | string
started_at_local | The shift start time | datetime
finished_at_local | The shift end time | datetime
duration_in_hours | The decimal hour duration of the shift | float
rate_of_pay | The rate of pay for the role during that shift | decimal
labor_cost | The cost of labor total for the shift | decimal

# Index

Check out our other documentation in the [index](https://github.com/TouchBistro/sous-chef/blob/master/README.md).
