# REST API

The following documentation is designed to help you connect with the REST API. Each section defines an endpoint, the request parameters, and the response attributes.

## Contents

Endpoint | Description
--- | --- |
[Menu](#menu) | Retrieve menu metadata, including menu item names and images
[Bills](#bills) | Retrieve closed bills metadata, including sales amounts
[Summary](#Dashboard) | Retrieve the main summary dashboard
[Drilldown](#Drilldown) | Retrieve the drilldown dashboard
[Sales by Day](#SalesByDay) | Retrieve the sales by day report
[Sales by Category](#SalesbyCategory) | Retrieve the sales by category report
[Shifts](#shifts) | Retrieve shifts, including labor costs

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

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request

```GET https://cloud.touchbistro.com/api/v1/bills?restaurant_id=<id>&from=<datetime>&to=<datetime>&authentication_token=<api_token>```

#### Params

* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.
* `<datetime>`: A datetime stamp in the ISO 8601 format (e.g., 2016-03-14T16:43:22Z)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.

### Response

```
{
	"bills": [{
		"id": 55032173,
		"uuid": "48D23CB6-82B8-4C56-8DF2-28D67649125F",
		"order_number": 5896,
		"bill_number": 9268,
		"tax_1": "0.26",
		"tax_2": "0.0",
		"tax_3": "0.0",
		"tax_1_name": "Tax 1",
		"tax_2_name": "Tax 2",
		"tax_3_name": "Tax 3",
		"paid_at": "2016-08-05T18:18:51.000Z",
		"paid_at_local": "2016-08-05T14:18:51.000Z",
		"order_items": [{
			"uuid": "9EFF7CC7-BFD2-412F-956E-FC4235056D90",
			"parent_id": null,
			"simple_modifier": false,
			"discounts_total": "0.0",
			"menu_item": "Lobster Tail",
			"quantity": "1.0",
			"total": "2.0"
		}],
		"payments": [{
			"payment_method": "VISA",
			"total": "2.26",
			"tip": "3.0",
			"change": "0.0"
		}],
		"total": "2.26",
		"sub_total": "2.0",
		"taxes": 0.26,
		"universal_identifier": "8452-20160805-9268-125F"
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

## <a name="Shifts"></a>Shifts

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request

```GET https://cloud.touchbistro.com/cloud/reporting/shifts?start=<datetime>&end=<datetime>&report=shifts&authentication_token=<api_token>&restaurant_id=<id>```

#### Params

* `<datetime>`: A datetime stamp in the UNX format (e.g., 1471219200)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.
* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.

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

## <a name="Dashboard"></a>Dashboard

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request
```GET https://cloud.touchbistro.com/cloud/reporting/dashboard?start=<datetime>&end=<datetime>&report=dashboard&authentication_token=<api_token>&restaurant_id=<id>```

#### Params

* `<datetime>`: A datetime stamp in the UNX format (e.g., 1471219200)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.
* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.


### Response

```
[
  [
    {
      "sales_revenue": 891.24,
      "tip_revenue": 89.17,
      "parties": 18,
      "bills": 18,
      "seats": 36,
      "average_spend_per_party": 49.51,
      "average_spend_per_bill": 49.51,
      "average_spend_per_seat": 24.76
    }
  ],
  [
    {
      "sales_category_name": "Food",
      "sales_revenue": 542.25,
      "quantity": 81
    },
    {
      "sales_category_name": "Alcohol",
      "sales_revenue": 348.99,
      "quantity": 73
    }
  ],
  [
    {
      "payment_method_name": "Cash",
      "sales_total": 611.3,
      "RecordCount": 12
    },
    {
      "payment_method_name": "Visa",
      "sales_total": 348.08,
      "RecordCount": 1
    },
    {
      "payment_method_name": "AMEX",
      "sales_total": 137.63,
      "RecordCount": 2
    },
    {
      "payment_method_name": "MasterCard",
      "sales_total": 102.31,
      "RecordCount": 3
    }
  ],
  [
    {
      "menu_category_name": "Beer",
      "sales_revenue": 348.99,
      "quantity": 73
    },
    {
      "menu_category_name": "Mains",
      "sales_revenue": 303.85,
      "quantity": 20
    },
    {
      "menu_category_name": "Appetizers",
      "sales_revenue": 120.97,
      "quantity": 30
    },
    {
      "menu_category_name": "Breakfast",
      "sales_revenue": 48.37,
      "quantity": 7
    },
    {
      "menu_category_name": "Vodka",
      "sales_revenue": 24.3,
      "quantity": 4
    },
    {
      "menu_category_name": "Coffee",
      "sales_revenue": 19.4,
      "quantity": 8
    },
    {
      "menu_category_name": "Soft Drinks",
      "sales_revenue": 12.75,
      "quantity": 8
    },
    {
      "menu_category_name": "Desserts",
      "sales_revenue": 12.61,
      "quantity": 4
    }
  ],
  [
    {
      "menu_item_name": "Pulled Pork",
      "sales_revenue": 114.75,
      "quantity": 9,
      "record_count": 9
    },
    {
      "menu_item_name": "Pasta and Clams",
      "sales_revenue": 79.6,
      "quantity": 5,
      "record_count": 5
    },
    {
      "menu_item_name": "Victory Ale",
      "sales_revenue": 79.05,
      "quantity": 4,
      "record_count": 6
    },
    {
      "menu_item_name": "Loaded Nachos",
      "sales_revenue": 35,
      "quantity": 7,
      "record_count": 7
    },
    {
      "menu_item_name": "Wing Nibbler",
      "sales_revenue": 16.5,
      "quantity": 3,
      "record_count": 3
    },
    {
      "menu_item_name": "Sweet Potato Fries",
      "sales_revenue": 16.25,
      "quantity": 5,
      "record_count": 5
    },
    {
      "menu_item_name": "House Salad",
      "sales_revenue": 12,
      "quantity": 3,
      "record_count": 3
    },
    {
      "menu_item_name": "Fries",
      "sales_revenue": 12,
      "quantity": 4,
      "record_count": 6
    },
    {
      "menu_item_name": "Soup Of The Day",
      "sales_revenue": 11.97,
      "quantity": 3,
      "record_count": 3
    },
    {
      "menu_item_name": "Omelette",
      "sales_revenue": 9.95,
      "quantity": 1,
      "record_count": 1
    },
    {
      "menu_item_name": "French Toast",
      "sales_revenue": 8.45,
      "quantity": 1,
      "record_count": 1
    },
    {
      "menu_item_name": "Cappuccino",
      "sales_revenue": 8,
      "quantity": 2,
      "record_count": 2
    },
    {
      "menu_item_name": "Trent Dark Ale",
      "sales_revenue": 6.25,
      "quantity": 1,
      "record_count": 2
    },
  ]
]
```

#### Attributes

##### Sales
Attribute | Description | Type
----- | ----- | -----
sales_revenue | Sales total before taxes for the given time period. | decimal
tip_revenue | Tips recorded for the given time period. This will not include cash tips. | float

##### Customers
Attribute | Description | Type
----- | ----- | -----
parties | The number of parties sat for the given time period (i.e., the number of orders) | int
bills | The number of guest checks issued for the given time period. Bill number will be higher than party number if tables/orders split into separate bills. | int
seats | The number of individuals served for the given time period. | string

##### Average Spend
Attribute | Description | Type
----- | ----- | -----
average_spend_per_party | Sales revenue divided by parties. | decimal
average_spend_per_bill | Sales revenue divided by bills issued. | decimal
average_spend_per_seat | Sales revenue divided by seats. | decimal
##### Top Sales Categories
Attribute | Description | Type
----- | ----- | -----
sales_category_name | Name of the sales category. | string
sales_revenue | Total before tax sales accrued for menu items under that sales category. | decimal
quantity | The number of menu items sold under the sales category. | int

##### Tender Types
Attribute | Description | Type
----- | ----- | -----
payment_method_name | Type of payment (cash, credit card name, etc.) | string
sales_total | Total before tax sales paid via the payment method. | decimal
RecordCount | The number of bills paid or partially paid with the payment type. | int
##### Top Menu Categories
Attribute | Description | Type
----- | ----- | -----
menu_category_name | Menu category name | string
sales_revenue | Total before tax sales accrued for menu items under that menu category. | decimal
quantity | The number of menu items sold under the menu category. | int

##### Top Menu Items
Attribute | Description | Type
----- | ----- | -----
menu_item_name | Menu item name. | string
sales_revenue |  Total before tax sales accrued for that menu item | decimal
quantity | The number of the menu items sold | decimal
record_count | The number of these items ordered, including voided items. | int

## <a name="Drilldown"></a>Drilldown

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request
```GET https://cloud.touchbistro.com/cloud/reporting/drilldown?start=<datetime>&end=<datetime>&report=drilldown&authentication_token=<api_token>&restaurant_id=<id>```

#### Params

* `<datetime>`: A datetime stamp in the UNX format (e.g., 1471219200)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.
* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.


### Response

```
[
  {
    "hour_of_day": 10,
    "section_name": "Takeout",
    "sales_category_name": "Food",
    "menu_category_name": "Soft Drinks",
    "menu_item_name": "Cola",
    "records": 3,
    "sales_revenue": 2.40,
    "void_revenue":1.20,
    "quantity": 2,
    "void_quantity": 1
  },
  {
    "hour_of_day": 9,
    "section_name": "Main",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Cocktails",
    "menu_item_name": "Singapore Sling",
    "records": 2,
    "sales_revenue": 25.1,
    "void_revenue": 0,
    "quantity": 2,
    "void_quantity": 0
  },
  {
    "hour_of_day": 9,
    "section_name": "Takeout",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Cocktails",
    "menu_item_name": "Rum And Coke",
    "records": 1,
    "sales_revenue": 0,
    "void_revenue": 5.53,
    "quantity": 0,
    "void_quantity": 1
  },
  {
    "hour_of_day": 10,
    "section_name": "Takeout",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Beer",
    "menu_item_name": "Muller Time Lager",
    "records": 2,
    "sales_revenue": 0,
    "void_revenue": 9.5,
    "quantity": 0,
    "void_quantity": 2
  },
  {
    "hour_of_day": 15,
    "section_name": "Main",
    "sales_category_name": "Food",
    "menu_category_name": "Appetizers",
    "menu_item_name": "Sweet Potato Fries",
    "records": 2,
    "sales_revenue": 6.5,
    "void_revenue": 0,
    "quantity": 2,
    "void_quantity": 0
  },
  {
    "hour_of_day": 9,
    "section_name": "Main",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Beer",
    "menu_item_name": "Trent Dark Ale",
    "records": 2,
    "sales_revenue": 12.5,
    "void_revenue": 0,
    "quantity": 4,
    "void_quantity": 0
  },
  {
    "hour_of_day": 10,
    "section_name": "Takeout",
    "sales_category_name": "Food",
    "menu_category_name": "Appetizers",
    "menu_item_name": "House Salad",
    "records": 1,
    "sales_revenue": 0,
    "void_revenue": 4,
    "quantity": 0,
    "void_quantity": 1
  },
  {
    "hour_of_day": 13,
    "section_name": "Cash Register",
    "sales_category_name": "Food",
    "menu_category_name": "Appetizers",
    "menu_item_name": "House Salad",
    "records": 1,
    "sales_revenue": 4,
    "void_revenue": 0,
    "quantity": 1,
    "void_quantity": 0
  },
  {
    "hour_of_day": 13,
    "section_name": "Cash Register",
    "sales_category_name": "Food",
    "menu_category_name": "Coffee",
    "menu_item_name": "Latte",
    "records": 1,
    "sales_revenue": 4.5,
    "void_revenue": 0,
    "quantity": 1,
    "void_quantity": 0
  },
  {
    "hour_of_day": 13,
    "section_name": "Cash Register",
    "sales_category_name": "Food",
    "menu_category_name": "Mains",
    "menu_item_name": "Pulled Pork",
    "records": 2,
    "sales_revenue": 25.5,
    "void_revenue": 0,
    "quantity": 2,
    "void_quantity": 0
  },
  {
    "hour_of_day": 15,
    "section_name": "Cash Register",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Cocktails",
    "menu_item_name": "Zombie",
    "records": 1,
    "sales_revenue": 9.99,
    "void_revenue": 0,
    "quantity": 1,
    "void_quantity": 0
  },
  {
    "hour_of_day": 15,
    "section_name": "Main",
    "sales_category_name": "Food",
    "menu_category_name": "Mains",
    "menu_item_name": "Pulled Pork",
    "records": 2,
    "sales_revenue": 114.75,
    "void_revenue": 0,
    "quantity": 9,
    "void_quantity": 0
  },
  {
    "hour_of_day": 13,
    "section_name": "Cash Register",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Beer",
    "menu_item_name": "Muller Time Lager",
    "records": 1,
    "sales_revenue": 4.75,
    "void_revenue": 0,
    "quantity": 1,
    "void_quantity": 0
  },
  {
    "hour_of_day": 14,
    "section_name": "Takeout",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Beer",
    "menu_item_name": "Trent Dark Ale",
    "records": 1,
    "sales_revenue": 6.25,
    "void_revenue": 0,
    "quantity": 1,
    "void_quantity": 0
  },
  {
    "hour_of_day": 14,
    "section_name": "Takeout",
    "sales_category_name": "Food",
    "menu_category_name": "Appetizers",
    "menu_item_name": "Loaded Nachos",
    "records": 1,
    "sales_revenue": 5,
    "void_revenue": 0,
    "quantity": 1,
    "void_quantity": 0
  },
  {
    "hour_of_day": 15,
    "section_name": "Cash Register",
    "sales_category_name": "Food",
    "menu_category_name": "Vodka",
    "menu_item_name": "SKVVY",
    "records": 1,
    "sales_revenue": 6,
    "void_revenue": 0,
    "quantity": 1,
    "void_quantity": 0
  },
  {
    "hour_of_day": 15,
    "section_name": "Main",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Beer",
    "menu_item_name": "Old Trog IPA",
    "records": 1,
    "sales_revenue": 5.97,
    "void_revenue": 0,
    "quantity": 1,
    "void_quantity": 0
  },
  {
    "hour_of_day": 11,
    "section_name": "Main",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Cocktails",
    "menu_item_name": "Singapore Sling",
    "records": 1,
    "sales_revenue": 12.55,
    "void_revenue": 0,
    "quantity": 1,
    "void_quantity": 0
  },
  {
    "hour_of_day": 11,
    "section_name": "Takeout",
    "sales_category_name": "Food",
    "menu_category_name": "Coffee",
    "menu_item_name": "Espresso",
    "records": 1,
    "sales_revenue": 1.4,
    "void_revenue": 0,
    "quantity": 1,
    "void_quantity": 0
  },
  {
    "hour_of_day": 14,
    "section_name": "Takeout",
    "sales_category_name": "Food",
    "menu_category_name": "Vodka",
    "menu_item_name": "SKVVY",
    "records": 1,
    "sales_revenue": 6,
    "void_revenue": 0,
    "quantity": 1,
    "void_quantity": 0
  }
]
```

Attribute | Description | Type
----- | ----- | -----
hour_of_day | Hour the sale was made, based on a 24 hour clock (e.g., 2 pm is returned as 14) | int
section_name | Name of the section this item was sold in | string
sales_category_name | Name of the sales category this item is under.| string
menu_category_name | Name of the menu this item is under.| string
menu_item_name | The menu item's name. | string
records | The number of these items ordered (sold or voided) in the hour | int
sales_revenue | Total before tax sales accrued for that menu item. | decimal
void_revenue | The total value of this item voided. | decimal
quantity | The total number of this item sold. | int
void_quantity | The total number of this item voided. | int


## <a name="SalesByDay"></a>Sales by Day

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request
```GET https://cloud.touchbistro.com/cloud/reporting/sales-by-day?start=<datetime>&end=<datetime>&report=sales-by-day&authentication_token=<api_token>&restaurant_id=<id>```

#### Params

* `<datetime>`: A datetime stamp in the UNX format (e.g., 1471219200)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.
* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.

### Response

```
[
  {
    "sales_revenue": 62.74,
    "quantity": 8,
    "void_revenue": 0,
    "group_day": "2016-09-06"
  },
  {
    "sales_revenue": 33.2,
    "quantity": 4,
    "void_revenue": 0,
    "group_day": "2016-09-07"
  },
  {
    "sales_revenue": 180.64,
    "quantity": 23,
    "void_revenue": 0,
    "group_day": "2016-09-08"
  },
  {
    "sales_revenue": 127.22,
    "quantity": 12,
    "void_revenue": 152.88,
    "group_day": "2016-09-09"
  },
  {
    "sales_revenue": 1380.555,
    "quantity": 180,
    "void_revenue": 2.65,
    "group_day": "2016-09-11"
  },
  {
    "sales_revenue": 515.55,
    "quantity": 64,
    "void_revenue": 0,
    "group_day": "2016-09-13"
  },
  {
    "sales_revenue": 3974.44,
    "quantity": 503,
    "void_revenue": 9.99,
    "group_day": "2016-09-14"
  },
  {
    "sales_revenue": 707.46,
    "quantity": 101,
    "void_revenue": 4,
    "group_day": "2016-09-15"
  },
  {
    "sales_revenue": 28,
    "quantity": 3,
    "void_revenue": 0,
    "group_day": "2016-09-16"
  },
  {
    "sales_revenue": 50.2825,
    "quantity": 11,
    "void_revenue": 59.33,
    "group_day": "2016-09-20"
  },
  {
    "sales_revenue": 6.25,
    "quantity": 2,
    "void_revenue": 5,
    "group_day": "2016-09-27"
  },
  {
    "sales_revenue": 20,
    "quantity": 4,
    "void_revenue": 0,
    "group_day": "2016-09-28"
  }
]
```

Attribute | Description | Type
----- | ----- | -----
sales_revenue | The per day sales totals (before tax). | string
quantity | The number of menu items sold. This does not include voids. | decimal
void_revenue | The value of voids (items voided after they were sent to the kitchen) authorized by a manager or admin. | decimal
group_day | Sales day | string


## <a name="SalesbyCategory"></a>Sales by Category

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request
```GET https://cloud.touchbistro.com/cloud/reporting/sales-by-menu-category?start=<datetime>&end=<datetime>&report=sales-by-menu-category&authentication_token=<api_token>&restaurant_id=<id>```

#### Params

* `<datetime>`: A datetime stamp in the UNX format (e.g., 1471219200)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.
* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.


### Response

```
[
  {
    "menu_category_name": "Mains",
    "sales_revenue": 349.6625,
    "void_revenue": 12.75,
    "quantity": 24,
    "void_quantity": 1
  },
  {
    "menu_category_name": "Beer",
    "sales_revenue": 348.99,
    "void_revenue": 14.25,
    "quantity": 73,
    "void_quantity": 3
  },
  {
    "menu_category_name": "Appetizers",
    "sales_revenue": 191.94,
    "void_revenue": 5,
    "quantity": 48,
    "void_quantity": 1
  },
  {
    "menu_category_name": "Vodka",
    "sales_revenue": 24.3,
    "void_revenue": 0,
    "quantity": 4,
    "void_quantity": 0
  },
  {
    "menu_category_name": "Desserts",
    "sales_revenue": 21.92,
    "void_revenue": 5.56,
    "quantity": 6,
    "void_quantity": 1
  },
  {
    "menu_category_name": "Coffee",
    "sales_revenue": 20.65,
    "void_revenue": 0,
    "quantity": 9,
    "void_quantity": 0
  },
  {
    "menu_category_name": "Soft Drinks",
    "sales_revenue": 12.75,
    "void_revenue": 0,
    "quantity": 8,
    "void_quantity": 0
  },
]
```

Attribute | Description | Type
----- | ----- | -----
menu_category_name | Name of the menu category. | string
sales_revenue | The before taxes sales total for each category. | decimal
void_revenue | The value of voids (items voided after they were sent to the kitchen) authorized by a manager or admin in this category. | string
quantity | The actual number of items ordered and billed within this category. This does not include voids. | decimal
void_quantity | The number of items voided within this category. | decimal



# Index

Check out our other documentation in the [index](https://github.com/TouchBistro/sous-chef/blob/master/README.md).
