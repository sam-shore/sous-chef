# REST API

The following documentation is designed to help you connect with the REST API. Each section defines an endpoint, the request parameters, and the response attributes.

## Contents

Endpoint | Description
--- | --- |
[Menu](#menu) | Retrieve menu metadata, including menu item names and images
[Bills](#bills) | Retrieve closed bill metadata, including sales amounts, for all bills
[Bill](#bill) | Retrieve closed bill metadata, including sales amounts, for a single bill
[Bills (Extended)](#bill-report-list) | Retrieve the extended view of bills for reporting purposes
[Bill (Extended)](#bill-report) | Retrieve the extended view of a single bill for reporting purposes
[Summary](#Dashboard) | Retrieve the main summary dashboard
[Drilldown](#Drilldown) | Retrieve the drilldown dashboard
[Sales by Day](#SalesByDay) | Retrieve the sales by day report
[Sales by Category](#SalesbyCategory) | Retrieve the sales by category report
[Menu Item Sales](#MenuItemSales) | Retrieve the menu item sales report
[Payments](#Payments) | Retrieve the payments report
[Sales by Section](#SalesbySection) | Retrieve the sales by section report
[Order Type](#OrderType) | Retrieve the order type report
[Time of Day](#TimeofDay) | Retrieve the time of day ("heat map") report
[Discounts and Voids](#Discounts) | Retrieve the discounts and voids report 
[Tips](#Tips) | Retrieve the tips report 
[Items](#Items) | Retrieve the items report 
[Shifts](#shifts) | Retrieve shifts, including labor costs
[Tax Summary](#Tax) | Retrieve the tax summary report

## <a name="menu"></a>Menu

### Authentication

This API is publically accessible once enabled for a given restaurant.

### Request

```GET https://cloud.touchbistro.com/api/v1/restaurants/<id>/menu```

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

```GET https://cloud.touchbistro.com/api/v1/bills?restaurant_id=<id>&from=<datetime>&to=<datetime>&page=<integer>&per_page=<integer>&authentication_token=<api_token>```

#### Params

* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.
* `<datetime>`: A datetime stamp in the ISO 8601 format (e.g., 2016-03-14T16:43:22Z). The span between "from" and "to" must be less than one day.
* `<page>`: An integer representing the page of results to return.
* `<per_page>`: An integer representing the number of results to return per page.
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.

### Response

```
{
	"bills": [{
		{
			id: 63415807,
			uuid: "855AA618-7F0D-4748-A5BC-A6C3D73922D1",
			order_number: 5906,
			bill_number: 9272,
			tax_1: "0.26",
			tax_2: "0.0",
			tax_3: "0.0",
			tax_1_name: "Tax 1",
			tax_2_name: "Tax 2",
			tax_3_name: "Tax 3",
			paid_at: "2016-08-16T23:47:31.000Z",
			paid_at_local: "2016-08-16T19:47:31.000Z",
			order_items: [{
				uuid: "ED3B3710-5786-4D5A-BBF7-080059D47D61",
				parent_id: null,
				simple_modifier: false,
				discounts_total: "0.0",
				menu_item: "Lobster Tail",
				menu_item_upc: "",
				estimated_tax_1: null,
				estimated_tax_2: null,
				estimated_tax_3: null,
				quantity: "1.0",
				total: "2.0"
			}],
			payments: [{
				payment_method: "VISA",
				total: "2.26",
				tip: "1.0",
				change: "0.0"
			}],
			total: "2.26",
			sub_total: "2.0",
			taxes: 0.26,
			universal_identifier: "8452-20160816-9272-22D1"
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


## <a name="bill"></a>Bill

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request

```GET https://cloud.touchbistro.com/api/v1/bill?universal_bill_identifier=<id>&authentication_token=<api_token>```

#### Params

* `<universal_bill_identifier>`: The unique identifier for the bill in question
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.

### Response

```
{
	id: 63415807,
	uuid: "855AA618-7F0D-4748-A5BC-A6C3D73922D1",
	order_number: 5906,
	bill_number: 9272,
	tax_1: "0.26",
	tax_2: "0.0",
	tax_3: "0.0",
	tax_1_name: "Tax 1",
	tax_2_name: "Tax 2",
	tax_3_name: "Tax 3",
	paid_at: "2016-08-16T23:47:31.000Z",
	paid_at_local: "2016-08-16T19:47:31.000Z",
	order_items: [{
		uuid: "ED3B3710-5786-4D5A-BBF7-080059D47D61",
		parent_id: null,
		simple_modifier: false,
		discounts_total: "0.0",
		menu_item: "Lobster Tail",
		menu_item_upc: "",
		estimated_tax_1: null,
		estimated_tax_2: null,
		estimated_tax_3: null,
		quantity: "1.0",
		total: "2.0"
	}],
	payments: [{
		payment_method: "VISA",
		total: "2.26",
		tip: "1.0",
		change: "0.0"
	}],
	total: "2.26",
	sub_total: "2.0",
	taxes: 0.26,
	universal_identifier: "8452-20160816-9272-22D1"
}
```

#### Attributes

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


## <a name="bill-report-list"></a>Bill (Extended)

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request

```GET https://cloud.touchbistro.com/cloud/reporting/bills?start=<start>&end=<end>&restaurant_id=<id>&authentication_token=<api_token>```

#### Params

* `<start>`: The UNIX start time for the period.
* `<end>`: The UNIX end time for the period. Must be less than 24 hour span
* `<id>`:The unique identifier for your restaurant. This will be provided to you by TouchBistro.
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.

### Response

```
[{
    "bill_id": 79251598,
    "bill_number": 9299,
    "order_number": 5928,
    "paid_at_local": "2016-09-20T11:19:08.000+00:00",
    "sales_revenue_with_tax": 2.26,
    "payment_amount": 0,
    "payment_count": 1,
    "waiter_name": "Alex",
    "take_out_type_id": null,
    "take_out_type": "dinein",
    "tax_amount": 0.26,
    "sales_revenue": 2,
    "discount_revenue": 0,
    "void_revenue": 0,
    "tax_1": 0.26,
    "tax_2": 0,
    "tax_3": 0
},...
]
```

#### Attributes

##### TBD

Attribute | Description | Type
----- | ----- | -----
id | The unique numeric identifier for the bill | integer
uuid | The guaranteed unique identifier for the bill | guid
closed_party_id | The unique nermic identifier for the party | integer
bill_number | The customer-facing numeric identifier for the bill | integer
tax_1 | The amount of tax 1 applied | decimal
tax_2 | The amount of tax 2 applied | decimal
tax_3 | The amount of tax 3 applied | decimal
tax_1_name | The customer-facing name of tax 1 | string
tax_2_name | The customer-facing name of tax 2 | string
tax_3_name | The customer-facing name of tax 2 | string
paid_at | The datetime of when the bill was paid | datetime
paid_at_local | The datetime of when the bill was paid | datetime
waiter | The waiter who closed the bill | object
restaurant | The restaurant the bill belongs to | object
order_items | The order items on the closed bill | object
party | The party the bill belongs to | object
payments | The payments associated with the closed bill | object
table_name | The table associated with the closed bill | string


## <a name="bill-report"></a>Bill (Extended)

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request

```GET https://cloud.touchbistro.com/cloud/reporting/bill-report?id=<bill_id>&restaurant_id=<id>&authentication_token=<api_token>```

#### Params

* `<bill_id>`: The unique identifier for a particular bill. This can be retrieved by using the /bills/ endpoint to retrieve the full list of bills.
* `<id>`:The unique identifier for your restaurant. This will be provided to you by TouchBistro.
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.

### Response

```
{
  "id": 87109549,
  "uuid": "EB0C378E-3D3F-4ABF-A290-A2386647CD51",
  "closed_party_id": 82035925,
  "bill_number": 9312,
  "tax_1": "3.27",
  "tax_2": "0.0",
  "tax_3": "0.0",
  "tax_1_name": "Tax 1",
  "tax_2_name": "Tax 2",
  "tax_3_name": "Tax 3",
  "paid_at": "2016-10-19T13:00:02.000Z",
  "paid_at_local": "2016-10-19T09:00:02.000Z",
  "waiter": {
    "id": 14495010,
    "name": "Alex",
    "base_restaurant_id": 5173,
    "lock_version": 44,
    "created_at": "2016-06-29T17:28:49.650Z",
    "updated_at": "2016-11-07T16:42:05.180Z",
    "versioned_at": "1970-01-01T00:00:00.000Z",
    "deleted_at": null,
    "base_waiter_id": 94948,
    "closed_data_upload_id": 62283692
  },
  "restaurant": {
    "id": 12933,
    "name": "Frank's Sangwiches",
    "created_at": "2016-06-14T18:02:19.853Z",
    "updated_at": "2016-11-01T17:43:30.487Z",
    "currency_code": "USD",
    "last_online": "2016-11-16T13:04:30.453Z",
    "encrypted_legacy_open_table_token": null,
    "encrypted_legacy_open_table_token_iv": null,
    "encrypted_legacy_open_table_token_salt": null,
    "start_of_day_offset": 0,
    "tax_1": "0.13",
    "tax_1_name": "Tax 1",
    "tax_2": "0.0",
    "tax_2_name": "Tax 2",
    "reduced_tax_1_enabled": false,
    "reduced_tax_1_threshold": "0.0",
    "reduced_tax_1": "0.0",
    "reduced_tax_2_enabled": false,
    "reduced_tax_2_threshold": "0.0",
    "reduced_tax_2": "0.0",
    "tax_3": "0.0",
    "tax_2_on_tax_1": false,
    "inclusive_tax": false,
    "tax_1_number": "R000123456789",
    "base_active_menu_id": 9114,
    "inclusive_tax_on_report": false,
    "versioned_at": "2016-10-27T13:51:25.433Z",
    "tax_3_name": "Tax 3",
    "reduced_tax_3_enabled": false,
    "reduced_tax_3_threshold": "0.0",
    "reduced_tax_3": "0.0",
    "credit_card_tip_out": false,
    "tip_out_name": "",
    "tip_out_amount": "0.0",
    "minimum_party_size": 6,
    "auto_gratuity_amount": "18.0",
    "gratuity_mode": "tip_includes_gratuity",
    "lock_version": 24,
    "enable_cloud_menu_management": true,
    "account_id": 7200,
    "user_id": 8452,
    "encrypted_open_table_token": null,
    "encrypted_open_table_token_iv": null,
    "encrypted_open_table_token_salt": null,
    "base_restaurant_id": 5173,
    "deleted_at": null,
    "enable_tax_1": true,
    "enable_tax_2": true,
    "enable_tax_3": true,
    "gratuity_display_name": "Gratuity",
    "real_time_dashboard": false,
    "tax_2_number": "",
    "tax_3_number": "",
    "enable_public_menu_view": true,
    "display_hidden_menu_items_in_menu_view": null
  },
  "order_items": [{
    "id": 431163239,
    "uuid": "509D2107-4558-4C0D-8C29-4B081BE1DBAD",
    "parent_id": 431163236,
    "simple_modifier": true,
    "quantity": 1,
    "simple_modifiers_total": null,
    "complex_modifiers_total": null,
    "total": null,
    "void": null,
    "discounts": null,
    "name": "Lime",
    "menu_item_id": null,
    "menu_item": "",
    "menu_category_id": 0,
    "menu_category": "",
    "sales_category_id": 0,
    "sales_category": ""
  }, {
    "id": 431163234,
    "uuid": "487BA685-B4FB-4A8B-AF2C-A0C7338FE79D",
    "parent_id": null,
    "simple_modifier": false,
    "quantity": 1,
    "simple_modifiers_total": "0.0",
    "complex_modifiers_total": "0.0",
    "total": "5.59",
    "void": "0.0",
    "discounts": "2.4",
    "name": null,
    "menu_item_id": 847427,
    "menu_item": "Margarita",
    "menu_category_id": 173897,
    "menu_category": "Drinks",
    "sales_category_id": 13045,
    "sales_category": "Alcohol"
  }, {
    "id": 431163235,
    "uuid": "9A8BDB2E-4E08-4E64-8F12-4C1B6E389A9F",
    "parent_id": null,
    "simple_modifier": false,
    "quantity": 1,
    "simple_modifiers_total": "0.0",
    "complex_modifiers_total": "0.0",
    "total": "4.19",
    "void": "0.0",
    "discounts": "1.8",
    "name": null,
    "menu_item_id": 897466,
    "menu_item": "Beer",
    "menu_category_id": 173897,
    "menu_category": "Drinks",
    "sales_category_id": 13045,
    "sales_category": "Alcohol"
  }, {
    "id": 431163236,
    "uuid": "79F2F05E-2280-4BB2-B7B8-9BF2D164384B",
    "parent_id": null,
    "simple_modifier": false,
    "quantity": 1,
    "simple_modifiers_total": "0.0",
    "complex_modifiers_total": "0.0",
    "total": "4.89",
    "void": "0.0",
    "discounts": "2.1",
    "name": null,
    "menu_item_id": 897461,
    "menu_item": "Martini",
    "menu_category_id": 173897,
    "menu_category": "Drinks",
    "sales_category_id": 13045,
    "sales_category": "Alcohol"
  }, {
    "id": 431163237,
    "uuid": "489B49E2-3378-40C9-B637-C4C6791E80A4",
    "parent_id": null,
    "simple_modifier": false,
    "quantity": 1,
    "simple_modifiers_total": "0.0",
    "complex_modifiers_total": "0.0",
    "total": "4.89",
    "void": "0.0",
    "discounts": "2.1",
    "name": null,
    "menu_item_id": 882816,
    "menu_item": "Mojito",
    "menu_category_id": 173897,
    "menu_category": "Drinks",
    "sales_category_id": 13045,
    "sales_category": "Alcohol"
  }, {
    "id": 431163238,
    "uuid": "07714D18-6C62-4CA1-8C82-31AF6B9F8749",
    "parent_id": null,
    "simple_modifier": false,
    "quantity": 1,
    "simple_modifiers_total": "0.0",
    "complex_modifiers_total": "0.0",
    "total": "5.59",
    "void": "0.0",
    "discounts": "2.4",
    "name": null,
    "menu_item_id": 882814,
    "menu_item": "Bloody Mary",
    "menu_category_id": 173897,
    "menu_category": "Drinks",
    "sales_category_id": 13045,
    "sales_category": "Alcohol"
  }],
  "party": [{
    "restaurant": "Frank's Sangwiches",
    "order_number": 5939,
    "seat_count": 2
  }],
  "payments": [{
    "payment_method": "Cash",
    "total": "28.42",
    "change": "0.0",
    "tip": "0.0"
  }],
  "table_name": "206"
}
```

#### Attributes

##### TBD

Attribute | Description | Type
----- | ----- | -----
id | The unique numeric identifier for the bill | integer
uuid | The guaranteed unique identifier for the bill | guid
closed_party_id | The unique nermic identifier for the party | integer
bill_number | The customer-facing numeric identifier for the bill | integer
tax_1 | The amount of tax 1 applied | decimal
tax_2 | The amount of tax 2 applied | decimal
tax_3 | The amount of tax 3 applied | decimal
tax_1_name | The customer-facing name of tax 1 | string
tax_2_name | The customer-facing name of tax 2 | string
tax_3_name | The customer-facing name of tax 2 | string
paid_at | The datetime of when the bill was paid | datetime
paid_at_local | The datetime of when the bill was paid | datetime
waiter | The waiter who closed the bill | object
restaurant | The restaurant the bill belongs to | object
order_items | The order items on the closed bill | object
party | The party the bill belongs to | object
payments | The payments associated with the closed bill | object
table_name | The table associated with the closed bill | string



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
      "quantity": 81.0
    },
    {
      "sales_category_name": "Alcohol",
      "sales_revenue": 348.99,
      "quantity": 73.0
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
      "quantity": 73.0
    },
    {
      "menu_category_name": "Mains",
      "sales_revenue": 303.85,
      "quantity": 20.0
    },
    {
      "menu_category_name": "Appetizers",
      "sales_revenue": 120.97,
      "quantity": 30.0
    },
    {
      "menu_category_name": "Breakfast",
      "sales_revenue": 48.37,
      "quantity": 7.0
    },
    {
      "menu_category_name": "Vodka",
      "sales_revenue": 24.3,
      "quantity": 4.0
    },
    {
      "menu_category_name": "Coffee",
      "sales_revenue": 19.4,
      "quantity": 8.0
    },
    {
      "menu_category_name": "Soft Drinks",
      "sales_revenue": 12.75,
      "quantity": 8.0
    },
    {
      "menu_category_name": "Desserts",
      "sales_revenue": 12.61,
      "quantity": 4.0
    }
  ],
  [
    {
      "menu_item_name": "Pulled Pork",
      "sales_revenue": 114.75,
      "quantity": 9.0,
      "record_count": 9
    },
    {
      "menu_item_name": "Pasta and Clams",
      "sales_revenue": 79.6,
      "quantity": 5,
      "record_count": 5.0
    },
    {
      "menu_item_name": "Victory Ale",
      "sales_revenue": 79.05,
      "quantity": 4.0,
      "record_count": 6
    },
    {
      "menu_item_name": "Loaded Nachos",
      "sales_revenue": 35,
      "quantity": 7.0,
      "record_count": 7
    },
    {
      "menu_item_name": "Wing Nibbler",
      "sales_revenue": 16.5,
      "quantity": 3.0,
      "record_count": 3
    },
    {
      "menu_item_name": "Sweet Potato Fries",
      "sales_revenue": 16.25,
      "quantity": 5.0
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
      "quantity": 4.0,
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
      "quantity": 1.0,
      "record_count": 1
    },
    {
      "menu_item_name": "French Toast",
      "sales_revenue": 8.45,
      "quantity": 1.0,
      "record_count": 1
    },
    {
      "menu_item_name": "Cappuccino",
      "sales_revenue": 8,
      "quantity": 2.0,
      "record_count": 2
    },
    {
      "menu_item_name": "Trent Dark Ale",
      "sales_revenue": 6.25,
      "quantity": 1.0,
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
parties | Number of orders taken | int

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
quantity | The number of menu items sold under the sales category. | decimal

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
quantity | The number of menu items sold under the menu category. | decimal

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
    "void_revenue": 0.0,
    "quantity": 2.0,
    "void_quantity": 0.0
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
    "quantity": 0.0,
    "void_quantity": 1.0
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
    "void_revenue": 0.0,
    "quantity": 2.0,
    "void_quantity": 0.0
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
    "void_revenue": 4.0,
    "quantity": 0.0,
    "void_quantity": 1.0
  },
  {
    "hour_of_day": 13,
    "section_name": "Cash Register",
    "sales_category_name": "Food",
    "menu_category_name": "Appetizers",
    "menu_item_name": "House Salad",
    "records": 1,
    "sales_revenue": 4,
    "void_revenue": 0.0,
    "quantity": 1.0,
    "void_quantity": 0.0
  },
  {
    "hour_of_day": 13,
    "section_name": "Cash Register",
    "sales_category_name": "Food",
    "menu_category_name": "Coffee",
    "menu_item_name": "Latte",
    "records": 1,
    "sales_revenue": 4.5,
    "void_revenue": 0.0,
    "quantity": 1.0,
    "void_quantity": 0.0
  },
  {
    "hour_of_day": 13,
    "section_name": "Cash Register",
    "sales_category_name": "Food",
    "menu_category_name": "Mains",
    "menu_item_name": "Pulled Pork",
    "records": 2,
    "sales_revenue": 25.5,
    "void_revenue": 0.0,
    "quantity": 2.0,
    "void_quantity": 0.0
  },
  {
    "hour_of_day": 15,
    "section_name": "Cash Register",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Cocktails",
    "menu_item_name": "Zombie",
    "records": 1,
    "sales_revenue": 9.99,
    "void_revenue": 0.0,
    "quantity": 1.0,
    "void_quantity": 0.0
  },
  {
    "hour_of_day": 15,
    "section_name": "Main",
    "sales_category_name": "Food",
    "menu_category_name": "Mains",
    "menu_item_name": "Pulled Pork",
    "records": 2,
    "sales_revenue": 114.75,
    "void_revenue": 0.0,
    "quantity": 9.0,
    "void_quantity": 0.0
  },
  {
    "hour_of_day": 13,
    "section_name": "Cash Register",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Beer",
    "menu_item_name": "Muller Time Lager",
    "records": 1,
    "sales_revenue": 4.75,
    "void_revenue": 0.0,
    "quantity": 1.0,
    "void_quantity": 0.0
  },
  {
    "hour_of_day": 14,
    "section_name": "Takeout",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Beer",
    "menu_item_name": "Trent Dark Ale",
    "records": 1,
    "sales_revenue": 6.25,
    "void_revenue": 0.0,
    "quantity": 1.0,
    "void_quantity": 0.0
  },
  {
    "hour_of_day": 14,
    "section_name": "Takeout",
    "sales_category_name": "Food",
    "menu_category_name": "Appetizers",
    "menu_item_name": "Loaded Nachos",
    "records": 1,
    "sales_revenue": 5,
    "void_revenue": 0.0,
    "quantity": 1.0,
    "void_quantity": 0.0
  },
  {
    "hour_of_day": 15,
    "section_name": "Cash Register",
    "sales_category_name": "Food",
    "menu_category_name": "Vodka",
    "menu_item_name": "SKVVY",
    "records": 1,
    "sales_revenue": 6,
    "void_revenue": 0.0,
    "quantity": 1.0,
    "void_quantity": 0.0
  },
  {
    "hour_of_day": 15,
    "section_name": "Main",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Beer",
    "menu_item_name": "Old Trog IPA",
    "records": 1,
    "sales_revenue": 5.97,
    "void_revenue": 0.0,
    "quantity": 1.0,
    "void_quantity": 0.0
  },
  {
    "hour_of_day": 11,
    "section_name": "Main",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Cocktails",
    "menu_item_name": "Singapore Sling",
    "records": 1,
    "sales_revenue": 12.55,
    "void_revenue": 0.0,
    "quantity": 1.0,
    "void_quantity": 0.0
  },
  {
    "hour_of_day": 11,
    "section_name": "Takeout",
    "sales_category_name": "Food",
    "menu_category_name": "Coffee",
    "menu_item_name": "Espresso",
    "records": 1,
    "sales_revenue": 1.4,
    "void_revenue": 0.0,
    "quantity": 1.0,
    "void_quantity": 0.0
  },
  {
    "hour_of_day": 14,
    "section_name": "Takeout",
    "sales_category_name": "Food",
    "menu_category_name": "Vodka",
    "menu_item_name": "SKVVY",
    "records": 1,
    "sales_revenue": 6,
    "void_revenue": 0.0,
    "quantity": 1.0,
    "void_quantity": 0.0
  }
]
```

Attribute | Description | Type
----- | ----- | -----
hour_of_day | Hour the sale was made, based on a 24 hour clock (e.g., 2 pm is returned as 14) | int
section_name | Name of the section this item was sold in | string
sales_category_name | Name of the sales category this item is under.| string
menu_category_name | Name of the menu category this item is under.| string
menu_item_name | The menu item's name. | string
records | The number of these items ordered (sold or voided) in the hour | int
sales_revenue | Total before tax sales accrued for that menu item. | decimal
void_revenue | The total value of this item voided. | decimal
quantity | The total number of this item sold. | decimal
void_quantity | The total number of this item voided. | decimal


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
    "quantity": 8.0,
    "void_revenue": 0,
    "discount_revenue": 0,
    "group_day": "2016-09-06"
    "bill_count": 18
  },
  {
    "sales_revenue": 33.2,
    "quantity": 4.0,
    "void_revenue": 0,
    "discount_revenue": 0,
    "group_day": "2016-09-07"
    "bill_count": 9
  },
  {
    "sales_revenue": 180.64,
    "quantity": 23.0,
    "void_revenue": 0,
    "discount_revenue": 0,
    "group_day": "2016-09-08"
    "bill_count": 6
  },
  {
    "sales_revenue": 127.22,
    "quantity": 12.0,
    "void_revenue": 152.88,
    "group_day": "2016-09-09"
    "bill_count": 3
  },
  {
    "sales_revenue": 1380.555,
    "quantity": 180.0,
    "void_revenue": 2.65,
    "discount_revenue": 0,
    "group_day": "2016-09-11"
    "bill_count": 18
  },
  {
    "sales_revenue": 515.55,
    "quantity": 64.0,
    "void_revenue": 0,
    "discount_revenue": 0,
    "group_day": "2016-09-13"
    "bill_count": 12
  },
  {
    "sales_revenue": 3974.44,
    "quantity": 503.0,
    "void_revenue": 9.99,
    "discount_revenue": 0,
    "group_day": "2016-09-14"
    "bill_count": 6
},
  {
    "sales_revenue": 707.46,
    "quantity": 101.0,
    "void_revenue": 4,
    "discount_revenue": 0,
    "group_day": "2016-09-15"
    "bill_count": 9
  },
  {
    "sales_revenue": 28,
    "quantity": 3.0,
    "void_revenue": 0,
    "discount_revenue": 0,
    "group_day": "2016-09-16"
    "bill_count": 4
  }
]
```

Attribute | Description | Type
----- | ----- | -----
sales_revenue | The per day sales totals (before tax). | string
quantity | The number of menu items sold. This does not include voids. TouchBistro supports ordering fractional amounts so the value is decimal. | decimal
void_revenue | The value of voids (items voided after they were sent to the kitchen) authorized by a manager or admin. | decimal
group_day | Sales day | string
bill_count | The number of bills closed for the day. | int


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
    "quantity": 24.0,
    "void_quantity": 1.0
  },
  {
    "menu_category_name": "Beer",
    "sales_revenue": 348.99,
    "void_revenue": 14.25,
    "quantity": 73.0,
    "void_quantity": 3.0
  },
  {
    "menu_category_name": "Appetizers",
    "sales_revenue": 191.94,
    "void_revenue": 5,
    "quantity": 48.0,
    "void_quantity": 1.0
  },
  {
    "menu_category_name": "Vodka",
    "sales_revenue": 24.3,
    "void_revenue": 0,
    "quantity": 4.0,
    "void_quantity": 0.0
  },
  {
    "menu_category_name": "Desserts",
    "sales_revenue": 21.92,
    "void_revenue": 5.56,
    "quantity": 6,
    "void_quantity": 1.0
  },
  {
    "menu_category_name": "Coffee",
    "sales_revenue": 20.65,
    "void_revenue": 0,
    "quantity": 9.0,
    "void_quantity": 0.0
  },
  {
    "menu_category_name": "Soft Drinks",
    "sales_revenue": 12.75,
    "void_revenue": 0,
    "quantity": 8.0,
    "void_quantity": 0.0
  },
]
```

Attribute | Description | Type
----- | ----- | -----
menu_category_name | Name of the menu category. | string
sales_revenue | The before taxes sales total for each category. | decimal
void_revenue | The value of voids (items voided after they were sent to the kitchen) authorized by a manager or admin in this category. | string
quantity | The actual number of items ordered and billed within this category. This does not include voids. TouchBistro supports ordering fractional amounts so the value is decimal. | decimal
void_quantity | The number of items voided within this category. | decimal

## <a name="MenuItemSales"></a>Menu Item Sales

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request
```GET https://cloud.touchbistro.com/cloud/reporting/sales-by-menu-item?start=<datetime>&end=<datetime>&report=sales-by-menu-item&authentication_token=<api_token>&restaurant_id=<id>```

#### Params

* `<datetime>`: A datetime stamp in the UNX format (e.g., 1471219200)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.
* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.


### Response
```
[
  {
    "menu_item_name": "Muller Time Lager",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Beer",
    "sales_revenue": 251.75,
    "void_revenue": 14.25,
    "quantity": 53.0,
    "void_quantity": 3.0,
    "record_count": 12,
    "sales_percentage": 24.7157201306718
  },
  {
    "menu_item_name": "Pulled Pork",
    "sales_category_name": "Food",
    "menu_category_name": "Mains",
    "sales_revenue": 160.5625,
    "void_revenue": 12.75,
    "quantity": 13.0,
    "void_quantity": 1.0,
    "record_count": 14,
    "sales_percentage": 15.7633279582164
  },
  {
    "menu_item_name": "Bacon Burger",
    "sales_category_name": "Food",
    "menu_category_name": "Mains",
    "sales_revenue": 109.5,
    "void_revenue": 0,
    "quantity": 6.0,
    "void_quantity": 0.0,
    "record_count": 6,
    "sales_percentage": 10.7502337807689
  },
  {
    "menu_item_name": "Loaded Nachos",
    "sales_category_name": "Food",
    "menu_category_name": "Appetizers",
    "sales_revenue": 83,
    "void_revenue": 5,
    "quantity": 17.0,
    "void_quantity": 1.0,
    "record_count": 18,
    "sales_percentage": 8.14857903017183
  },
  {
    "menu_item_name": "Pasta and Clams",
    "sales_category_name": "Food",
    "menu_category_name": "Mains",
    "sales_revenue": 79.6,
    "void_revenue": 0,
    "quantity": 5.0,
    "void_quantity": 0,
    "record_count": 5,
    "sales_percentage": 7.81478181688768
  },
  {
    "menu_item_name": "Victory Ale",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Beer",
    "sales_revenue": 79.05,
    "void_revenue": 0,
    "quantity": 17.0,
    "void_quantity": 0.0,
    "record_count": 4,
    "sales_percentage": 7.76078520885642
  },
  {
    "menu_item_name": "House Salad",
    "sales_category_name": "Food",
    "menu_category_name": "Appetizers",
    "sales_revenue": 29,
    "void_revenue": 0,
    "quantity": 8.0,
    "void_quantity": 0.0,
    "record_count": 8,
    "sales_percentage": 2.84709387801184
  },
  {
    "menu_item_name": "Pancakes",
    "sales_category_name": "Food",
    "menu_category_name": "Breakfast",
    "sales_revenue": 23.97,
    "void_revenue": 0,
    "quantity": 3.0,
    "void_quantity": 0.0,
    "record_count": 3,
    "sales_percentage": 2.35327035365324
  },
  {
    "menu_item_name": "Singapore Sling",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Cocktails",
    "sales_revenue": 0,
    "void_revenue": 25.1,
    "quantity": 0.0,
    "void_quantity": 2.0
    "record_count": 2,
    "sales_percentage": 0
  },
  {
    "menu_item_name": "Zombie",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Cocktails",
    "sales_revenue": 0,
    "void_revenue": 19.98,
    "quantity": 0.0,
    "void_quantity": 2.0,
    "record_count": 1,
    "sales_percentage": 0
  }
]
```
Attribute | Description | Type
----- | ----- | -----
menu_item_name | Name of the menu item | string
sales_category_name | A configured sales category. | string
menu_category_name | A menu category. | string
sales_revenue | The before taxes sales total for each menu item. Totals do not include tax and gratuity. The total also does not represent voids. | decimal
void_revenue | Total  voids in dollar amounts authorized for each menu item. | decimal
quantity | The number of times this item has been billed. TouchBistro supports ordering fractional amounts so the value is decimal. | decimal
void_quantity | The number of times the item has been voided. | decimal
record_count | The total number of times this item has been ordered (billed plus voided). | int
sales_percentage | Percentage of sales. | float

## <a name="Payments"></a>Payments

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request
```GET https://cloud.touchbistro.com/cloud/reporting/payments?start=<datetime>&end=<datetime>&report=payments&authentication_token=<api_token>&restaurant_id=<id>```

#### Params

* `<datetime>`: A datetime stamp in the UNX format (e.g., 1471219200)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.
* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.


### Response

```
[
  {
    "payment_method_name": "Cash",
    "total_amount": 750.29,
    "tip_total": 0.0,
    "record_count": 32,
    "sales_total": 750.29
  },
  {
    "payment_method_name": "Visa",
    "total_amount": 300.63,
    "tip_total": 53.45,
    "record_count": 2,
    "sales_total": 354.08
  },
  {
    "payment_method_name": "AMEX",
    "total_amount": 118.5,
    "tip_total": 19.13,
    "record_count": 2,
    "sales_total": 137.63
  },
  {
    "payment_method_name": "MasterCard",
    "total_amount": 85.37,
    "tip_total": 16.94,
    "record_count": 3,
    "sales_total": 102.31
  }
]
```
Attribute | Description | Type
----- | ----- | -----
payment_method_name | Type of payment (cash, credit card name, etc.) | string
total_amount | Amount tendered or charged. Totals include tax. | decimal
tip_total | The amount in tips or auto grats. The cash payment method will not include tips or auto grats. | decimal
record_count | Number of bills settled with the payment type. | int
sales_total | Amount tendered or charged. Totals include tax, credit card gratuities, and auto-grats. The cash total will not include tips or auto grats. | decimal

## <a name="SalesbySection"></a>Sales by Section

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request
```GET https://cloud.touchbistro.com/cloud/reporting/sales-by-section?start=<datetime>&end=<datetime>&report=sales-by-section&authentication_token=<api_token>&restaurant_id=<id>```

#### Params

* `<datetime>`: A datetime stamp in the UNX format (e.g., 1471219200)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.
* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.


### Response

```
[
  {
    "section_name": "Counter",
    "sales_revenue": 33,
    "void_revenue": 0,
    "discount_revenue": 0,
    "quantity": 4.0,
    "void_quantity": 0.0,
    "bill_count": 5
  },
  {
    "section_name": "Main",
    "sales_revenue": 3094.725,
    "void_revenue": 2.65,
    "discount_revenue": 55.625,
    "quantity": 361.0,
    "void_quantity": 53.0,
    "bill_count":18
  },
  {
    "section_name": "Takeout",
    "sales_revenue": 799.45,
    "void_revenue": 156.87,
    "discount_revenue": 14,
    "quantity": 127.0,
    "void_quantity": 24.0,
    "bill_count": 9
  },
  {
    "section_name": "Cash Register",
    "sales_revenue": 3159.1625,
    "void_revenue": 74.33,
    "discount_revenue": 23.6875,
    "quantity": 423.0,
    "void_quantity": 15.0,
    "bill_count": 16
  }
]
```
Attribute | Description | Type
----- | ----- | -----
section_name | Name given to the floor plan section. | string
sales_revenue | Sales, before taxes, for this section. | float
void_revenue | Total value of voids authorized under this section. | float
discount_revenue | Total value of discounts given under this section. | float
quantity | Total number of menu items billed in this section. | decimal
void_quantity | Total number of voids authorized in this section. This does not include discounts. | decimal
bill_count | The total number of bills closed for that section. | int

## <a name="OrderType"></a>Order Type

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request
```GET https://cloud.touchbistro.com/cloud/reporting/order-type?start=<datetime>&end=<datetime>&report=order-type&authentication_token=<api_token>&restaurant_id=<id>```

#### Params

* `<datetime>`: A datetime stamp in the UNX format (e.g., 1471219200)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.
* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.


### Response

```
[
  {
    "day": "2016-09-06",
    "take_out_type": "dinein",
    "records": 9,
    "sales_revenue": 62.74,
    "void_revenue": 12.95,
    "discount_revenue": 0,
    "quantity": 8.0,
    "void_quantity": 1.0
  },
  {
    "day": "2016-09-07",
    "take_out_type": "bartab",
    "records": 3,
    "sales_revenue": 20.65,
    "void_revenue": 0,
    "discount_revenue": 0,
    "quantity": 3.0,
    "void_quantity": 0.0
  },
  {
    "day": "2016-09-08",
    "take_out_type": "bartab",
    "records": 5,
    "sales_revenue": 37.6,
    "void_revenue": 0,
    "discount_revenue": 0,
    "quantity": 5.0,
    "void_quantity": 0.0
  },
  {
    "day": "2016-09-08",
    "take_out_type": "dinein",
    "records": 18,
    "sales_revenue": 143.04,
    "void_revenue": 0,
    "discount_revenue": 12.5,
    "quantity": 18,
    "void_quantity": 0
  },
  {
    "day": "2016-09-09",
    "take_out_type": "bartab",
    "records": 20,
    "sales_revenue": 0,
    "void_revenue": 152.88,
    "discount_revenue": 0,
    "quantity": 0.0,
    "void_quantity": 20.0
  },
  {
    "day": "2016-09-11",
    "take_out_type": "bartab",
    "records": 31,
    "sales_revenue": 216.91,
    "void_revenue": 0,
    "discount_revenue": 0,
    "quantity": 31.0,
    "void_quantity": 0.0
  },
  {
    "day": "2016-09-11",
    "take_out_type": "delivery",
    "records": 22,
    "sales_revenue": 109.71,
    "void_revenue": 0,
    "discount_revenue": 0,
    "quantity": 22.0,
    "void_quantity": 0.0
  },
  {
    "day": "2016-09-11",
    "take_out_type": "takeout",
    "records": 17,
    "sales_revenue": 196.66,
    "void_revenue": 0,
    "discount_revenue": 0,
    "quantity": 17.0,
    "void_quantity": 0.0
  },
  {
    "day": "2016-09-13",
    "take_out_type": "takeout",
    "records": 4,
    "sales_revenue": 5.65,
    "void_revenue": 0,
    "discount_revenue": 0,
    "quantity": 4.0,
    "void_quantity": 0.0
  },
  {
    "day": "2016-09-14",
    "take_out_type": "delivery",
    "records": 5,
    "sales_revenue": 33.49,
    "void_revenue": 0,
    "discount_revenue": 0,
    "quantity": 5.0,
    "void_quantity": 0.0
  },
  {
    "day": "2016-09-14",
    "take_out_type": "dinein",
    "records": 428,
    "sales_revenue": 3805.16,
    "void_revenue": 6,
    "discount_revenue": 6,
    "quantity": 470.0,
    "void_quantity": 1.0
  },
  {
    "day": "2016-09-15",
    "take_out_type": "delivery",
    "records": 12,
    "sales_revenue": 42.99,
    "void_revenue": 0,
    "discount_revenue": 0,
    "quantity": 12.0,
    "void_quantity": 0.0
  },
  {
    "day": "2016-09-15",
    "take_out_type": "dinein",
    "records": 65,
    "sales_revenue": 664.47,
    "void_revenue": 4,
    "discount_revenue": 37.5,
    "quantity": 89.0,
    "void_quantity": 1.0
  },
]
```

Attribute | Description | Type
----- | ----- | -----
day | Sales day | string
take_out_type | The order type. | string
records | The number of items ordered (sold or voided) for the sales day. | int
sales_revenue | Total before tax sales accrued for order.| decimal
void_revenue | The total value of this item voided. | decimal
discount_revenue | The total value of this item discounted. | decimal
quantity | The total number of this item sold. | decimal
void_quantity | The total number of this item voided. | decimal

## <a name="TimeofDay"></a>Time of Day

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request
```GET https://cloud.touchbistro.com/cloud/reporting/time-of-day?start=<datetime>&end=<datetime>&report=time-of-day&authentication_token=<api_token>&restaurant_id=<id>```

#### Params

* `<datetime>`: A datetime stamp in the UNX format (e.g., 1471219200)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.
* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.

### Response

```
[
  {
    "sales_revenue": 402.63,
    "quantity": 49.0,
    "void_revenue": 0,
    "void_quantity": 1.0,
    "day_of_week": 1,
    "hour_of_day": 11
  },
  {
    "sales_revenue": 333.73,
    "quantity": 49.0,
    "void_revenue": 2.65,
    "void_quantity": 6.0,
    "day_of_week": 1,
    "hour_of_day": 12
  },
  {
    "sales_revenue": 465.475,
    "quantity": 63.0,
    "void_revenue": 0.0,
    "void_quantity": 1,
    "day_of_week": 1,
    "hour_of_day": 14
  },
  {
    "sales_revenue": 178.72,
    "quantity": 19.0,
    "void_revenue": 0,
    "void_quantity": 0.0,
    "day_of_week": 1,
    "hour_of_day": 16
  },
  {
    "sales_revenue": 5,
    "quantity": 1.0,
    "void_revenue": 0,
    "void_quantity": 0.0,
    "day_of_week": 3,
    "hour_of_day": 10
  },
  {
    "sales_revenue": 24.97,
    "quantity": 8.0,
    "void_revenue": 45.08,
    "void_quantity": 4.0,
    "day_of_week": 3,
    "hour_of_day": 11
  },
  {
    "sales_revenue": 20.3125,
    "quantity": 2.0,
    "void_revenue": 14.25,
    "void_quantity": 3.0,
    "day_of_week": 3,
    "hour_of_day": 12
  },
  {
    "sales_revenue": 451.53,
    "quantity": 49.0,
    "void_revenue": 0,
    "void_quantity": 3.0,
    "day_of_week": 3,
    "hour_of_day": 13
  },
  {
    "sales_revenue": 30.24,
    "quantity": 5.0,
    "void_revenue": 5,
    "void_quantity": 1.0,
    "day_of_week": 3,
    "hour_of_day": 15
  },
  {
    "sales_revenue": 38.57,
    "quantity": 9.0,
    "void_revenue": 0,
    "void_quantity": 0.0,
    "day_of_week": 3,
    "hour_of_day": 17
  },
  {
    "sales_revenue": 64.2,
    "quantity": 11.0,
    "void_revenue": 0,
    "void_quantity": 0.0,
    "day_of_week": 3,
    "hour_of_day": 21
  },
  {
    "sales_revenue": 20,
    "quantity": 4.0,
    "void_revenue": 0,
    "void_quantity": 0.0,
    "day_of_week": 4,
    "hour_of_day": 10
  },
  {
    "sales_revenue": 257.64,
    "quantity": 18.0,
    "void_revenue": 0,
    "void_quantity": 12.0,
    "day_of_week": 4,
    "hour_of_day": 11
  },
  {
    "sales_revenue": 8.25,
    "quantity": 2.0,
    "void_revenue": 0,
    "void_quantity": 1.0,
    "day_of_week": 4,
    "hour_of_day": 12
  },
  {
    "sales_revenue": 1551.54,
    "quantity": 190.0,
    "void_revenue": 0,
    "void_quantity": 6.0,
    "day_of_week": 4,
    "hour_of_day": 13
  },
  {
    "sales_revenue": 429.08,
    "quantity": 64.0,
    "void_revenue": 0,
    "void_quantity": 1.0,
    "day_of_week": 4,
    "hour_of_day": 14
  },
  {
    "sales_revenue": 1369.51,
    "quantity": 165.0,
    "void_revenue": 0,
    "void_quantity": 0.0,
    "day_of_week": 4,
    "hour_of_day": 15
  },
  {
    "sales_revenue": 68.58,
    "quantity": 14.0,
    "void_revenue": 3.99,
    "void_quantity": 1.0,
    "day_of_week": 4,
    "hour_of_day": 16
  },
  {
    "sales_revenue": 71.86,
    "quantity": 14.0,
    "void_revenue": 0,
    "void_quantity": 1.0,
    "day_of_week": 4,
    "hour_of_day": 18
  },
  {
    "sales_revenue": 180.78,
    "quantity": 29.0,
    "void_revenue": 0,
    "void_quantity": 0.0,
    "day_of_week": 4,
    "hour_of_day": 20
  },
  {
    "sales_revenue": 33,
    "quantity": 4.0,
    "void_revenue": 0,
    "void_quantity": 0.0,
    "day_of_week": 4,
    "hour_of_day": 21
  },
  {
    "sales_revenue": 16.7,
    "quantity": 3.0,
    "void_revenue": 6,
    "void_quantity": 1.0,
    "day_of_week": 4,
    "hour_of_day": 23
  },
  {
    "sales_revenue": 20.7,
    "quantity": 4.0,
    "void_revenue": 0,
    "void_quantity": 0.0,
    "day_of_week": 5,
    "hour_of_day": 0
  },
  {
    "sales_revenue": 143.04,
    "quantity": 18.0,
    "void_revenue": 0,
    "void_quantity": 12.0,
    "day_of_week": 5,
    "hour_of_day": 9
  },
  {
    "sales_revenue": 313.49,
    "quantity": 40.0,
    "void_revenue": 0,
    "void_quantity": 0.0,
    "day_of_week": 5,
    "hour_of_day": 10
  },
  {
    "sales_revenue": 43.75,
    "quantity": 9.0
    "void_revenue": 4,
    "void_quantity": 1.0,
    "day_of_week": 5,
    "hour_of_day": 13
  },
  {
    "sales_revenue": 37.6,
    "quantity": 5.0,
    "void_revenue": 0,
    "void_quantity": 0.0,
    "day_of_week": 5,
    "hour_of_day": 14
  },
  {
    "sales_revenue": 107.23,
    "quantity": 8.0,
    "void_revenue": 0,
    "void_quantity": 1.0,
    "day_of_week": 5,
    "hour_of_day": 16
  },
  {
    "sales_revenue": 103.61,
    "quantity": 20.0,
    "void_revenue": 0,
    "void_quantity": 0.0,
    "day_of_week": 5,
    "hour_of_day": 18
  },
  {
    "sales_revenue": 88.15,
    "quantity": 21.0,
    "void_revenue": 0,
    "void_quantity": 14.0,
    "day_of_week": 5,
    "hour_of_day": 21
  },
  {
    "sales_revenue": 51.23,
    "quantity": 3.0,
    "void_revenue": 0,
    "void_quantity": 1.0,
    "day_of_week": 5,
    "hour_of_day": 23
  },
  {
    "sales_revenue": 0,
    "quantity": 0.0,
    "void_revenue": 27.08,
    "void_quantity": 5.0,
    "day_of_week": 6,
    "hour_of_day": 9
  },
  {
    "sales_revenue": 0,
    "quantity": 0.0,
    "void_revenue": 125.8,
    "void_quantity": 16.0,
    "day_of_week": 6,
    "hour_of_day": 10
  },
  {
    "sales_revenue": 127.22,
    "quantity": 12.0,
    "void_revenue": 0,
    "void_quantity": 0.0,
    "day_of_week": 6,
    "hour_of_day": 15
  },
  {
    "sales_revenue": 28,
    "quantity": 3.0,
    "void_revenue": 0,
    "void_quantity": 0.0,
    "day_of_week": 6,
    "hour_of_day": 23
  }
]
```

Attribute | Description | Type
----- | ----- | -----
sales_revenue | Sales total before taxes for the given day and hour block. | decimal
quantity | Number of menu items sold. | decimal
void_revenue | Value of items voided and not sold. | decimal
void_quantity | Number of items voided. | decimal
day_of_week | Number representing the day of the week. Sunday=1. Monday=2. And so on. | int
hour_of_day | Hour the sale was made, based on a 24-hour clock (e.g., 2 pm is returned as 14). | int


## <a name="Discounts"></a>Discounts and Voids

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request
```GET https://cloud.touchbistro.com/cloud/reporting/discounts?start=<datetime>&end=<datetime>&report=discounts&authentication_token=<api_token>&restaurant_id=<id>```

#### Params

* `<datetime>`: A datetime stamp in the UNX format (e.g., 1471219200)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.
* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.

### Response

```
[
  {
    "waiter_name": "John H.",
    "manager_name": "Cam P.",
    "discounted_at_local": "2016-09-27T15:21:28.000+00:00",
    "discount_description": "Mistake",
    "menu_item_price": 5,
    "menu_item_name": "Loaded Nachos",
    "sales_category_name": "Food",
    "menu_category_name": "Appetizers",
    "sales_revenue": 0,
    "void_revenue": 5,
    "discount_revenue": 0,
    "quantity": 0.0,
    "void_quantity": 1.0
  },
 {
    "waiter_name": "Frank R.",
    "manager_name": "Admin",
    "discounted_at_local": "2016-09-20T11:57:40.000+00:00",
    "discount_description": "Coupon",
    "menu_item_price": 4,
    "menu_item_name": "House Salad",
    "sales_category_name": "Food",
    "menu_category_name": "Appetizers",
    "sales_revenue": 1,
    "void_revenue": 0,
    "discount_revenue": 3,
    "quantity": 1.0,
    "void_quantity": 0.0
  },
  {
    "waiter_name": "Mary S.",
    "manager_name": "Admin",
    "discounted_at_local": "2016-10-17T10:25:43.000+00:00",
    "discount_description": "Mistake",
    "menu_item_price": 5.56,
    "menu_item_name": "Fudge Brownie",
    "sales_category_name": "Food",
    "menu_category_name": "Desserts",
    "sales_revenue": 0,
    "void_revenue": 5.56,
    "discount_revenue": 0,
    "quantity": 0.0,
    "void_quantity": 1.0
  },
  {
    "waiter_name": "Mary S.",
    "manager_name": "Admin",
    "discounted_at_local": "2016-10-17T13:09:33.000+00:00",
    "discount_description": "Mistake",
    "menu_item_price": 12.75,
    "menu_item_name": "Pulled Pork",
    "sales_category_name": "Food",
    "menu_category_name": "Mains",
    "sales_revenue": 0,
    "void_revenue": 12.75,
    "discount_revenue": 0,
    "quantity": 0.0,
    "void_quantity": 1.0
  }
]
```

Attribute | Description | Type
----- | ----- | -----
waiter_name | The waiter that issued the void or discount. | string
manager_name | The manager or admin that approved the void or discount. | string
discounted_at_local | The date of the discount. | string
discount_description | The void or discount applied. If the item has two discounts (for example a 25% off birthday discount and a $2 coupon discount), each discount will appear as a separate object. | string
menu_item_price | The price of the item discounted or voided. | decimal
menu_item_name | The menu item that was discounted. | string
sales_category_name | The sales category the voided or discounted item is under. | string
menu_category_name | The menu category the voided or discounted item is under. | string
sales_revenue | The actual sale amount realized after the discount. This will be 0 for voids. | decimal
void_revenue | The amount voided. | decimal
discount_revenue | The amount of the discount applied. | decimal
quantity | The number of discounts applied. This may be greater than 1 if you discounted multiple menu items of same type (added using the Quantity option) on the same bill. | decimal
void_quantity | The number of the items voided on the bill. If more than one of the same item was voided on a bill, this number will be greater than zero. For example, if two Singapore Sling drinks were added to an order, sent, and then voided, Void Quantity would be 2. | decimal


## <a name="Tips"></a>Tips

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request
```GET https://cloud.touchbistro.com/cloud/reporting/waiter-tips?start=<datetime>&end=<datetime>&report=waiter-tips&authentication_token=<api_token>&restaurant_id=<id>```

#### Params

* `<datetime>`: A datetime stamp in the UNX format (e.g., 1471219200)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.
* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.


### Response

```
[
  {
    "waiter_name": "Lucy",
    "payment_method_name": "AMEX",
    "total_applied": 118.5,
    "total_tip": 19.13,
    "record_count": 2
  },
  {
    "waiter_name": "Lucy",
    "payment_method_name": "Cash",
    "total_applied": 390.96,
    "total_tip": 0,
    "record_count": 11
  },
  {
    "waiter_name": "Mary S.",
    "payment_method_name": "Cash",
    "total_applied": 220.34,
    "total_tip": 0,
    "record_count": 1
  },
  {
    "waiter_name": "Lucy",
    "payment_method_name": "MasterCard",
    "total_applied": 85.37,
    "total_tip": 16.94,
    "record_count": 3
  },
  {
    "waiter_name": "Lucy",
    "payment_method_name": "Visa",
    "total_applied": 294.98,
    "total_tip": 53.1,
    "record_count": 1
  }
]
```

Attribute | Description | Type
----- | ----- | -----
waiter_name | The waiter name. | string
payment_method_name | The method used to pay. | string
total_applied | The pre-tip/pre-tax total of the bills paid with the payment method. | decimal
total_tip | The total tips charged to the payment method. | decimal
record_count | The number of bills that were paid with this payment method. | int

## <a name="Items"></a>Items

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request
```GET https://cloud.touchbistro.com/cloud/reporting/waiter-items?start=<datetime>&end=<datetime>&report=waiter-items&authentication_token=<api_token>&restaurant_id=<id>```

#### Params

* `<datetime>`: A datetime stamp in the UNX format (e.g., 1471219200)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.
* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.


### Response

```
[
  {
    "waiter_name": "Lucy",
    "menu_item_name": "Apple Pie",
    "sales_category_name": "Food",
    "menu_category_name": "Desserts",
    "sales_revenue": 3.75,
    "void_revenue": 0,
    "discount_revenue": 0,
    "quantity": 1.0,
    "void_quantity": 0.0,
    "record_count": 1
  },
  {
    "waiter_name": "Lucy",
    "menu_item_name": "Bacon Burger",
    "sales_category_name": "Food",
    "menu_category_name": "Mains",
    "sales_revenue": 36.5,
    "void_revenue": 0,
    "discount_revenue": 0,
    "quantity": 2.0,
    "void_quantity": 0.0,
    "record_count": 2
  },
  {
    "waiter_name": "Lucy",
    "menu_item_name": "Fries",
    "sales_category_name": "Food",
    "menu_category_name": "Appetizers",
    "sales_revenue": 6,
    "void_revenue": 0,
    "discount_revenue": 0,
    "quantity": 3.0,
    "void_quantity": 0.0,
    "record_count": 3
  },
{
    "waiter_name": "Mary S.",
    "menu_item_name": "Fudge Brownie",
    "sales_category_name": "Food",
    "menu_category_name": "Desserts",
    "sales_revenue": 5.56,
    "void_revenue": 5.56,
    "discount_revenue": 0,
    "quantity": 1.0,
    "void_quantity": 1.0,
    "record_count": 2
  },
{
    "waiter_name": "Mary S.",
    "menu_item_name": "Trent Dark Ale",
    "sales_category_name": "Alcohol",
    "menu_category_name": "Beer",
    "sales_revenue": 18.75,
    "void_revenue": 6.25,
    "discount_revenue": 12.5,
    "quantity": 5.0,
    "void_quantity": 1.0,
    "record_count": 4
},
{
    "waiter_name": "Mary S.",
    "menu_item_name": "Pulled Pork",
    "sales_category_name": "Food",
    "menu_category_name": "Mains",
    "sales_revenue": 25.5,
    "void_revenue": 12.75,
    "discount_revenue": 0,
    "quantity": 2.0,
    "void_quantity": 1.0,
    "record_count": 3
  }  {
    "waiter_name": "Mary S.",
    "menu_item_name": "Tea",
    "sales_category_name": "Food",
    "menu_category_name": "Coffee",
    "sales_revenue": 1.25,
    "void_revenue": 0,
    "discount_revenue": 0,
    "quantity": 1.0,
    "void_quantity": 0.0,
    "record_count": 1
  }
]
```

Attribute | Description | Type
----- | ----- | -----
waiter_name | The waiter name. | string
menu_item_name | The menu item the waiter has sold. | string
sales_category_name | The sales category the item is under. | string
menu_category_name | The menu categories (“mains”, “appetizers”, etc.) the item is under. | string
sales_revenue | The total sales the waiter generated for that menu item. If discounts or voids were applied, this figure will represent the sales realized after the discounts and voids. | decimal
void_revenue | The total dollar amount of the void. | decimal
discount_revenue | The total amount of the discount the waiter applied to that menu item. | decimal
quantity | The total number of the items discounted by the waiter. | int
void_quantity | The total number of times this item was voided. | decimal
record_count | The total number of the item sold by the waiter. | decimal

## <a name="Tax"></a>Tax Summary

### Authentication

This API requires an authentication token which is passed in as a query parameter. As a result, the request must be sent over HTTPS.

### Request
```GET https://cloud.touchbistro.com/cloud/reporting/tax-summary?start=<datetime>&end=<datetime>&report=tax-summary&authentication_token=<api_token>&restaurant_id=<id>```

#### Params

* `<datetime>`: A datetime stamp in the UNX format (e.g., 1471219200)
* `<api_token>`: The unique API Token associated with your TouchBistro Dev account. This will be provided to you by TouchBistro.
* `<id>`: The unique identifier for your restaurant. This will be provided to you by TouchBistro.


### Response

```
[
  {
    "tax_name": "State",
    "bill_count_at_fullrate": 5,
    "tax_amount_at_fullrate": 3.3,
    "bill_count_at_reducedrate": 2,
    "tax_amount_at_reducedrate": 0.12,
    "tax_total": 3.42
  },
  {
    "tax_name": "City",
    "bill_count_at_fullrate": 3,
    "tax_amount_at_fullrate": 1.55,
    "bill_count_at_reducedrate": 0,
    "tax_amount_at_reducedrate": 0,
    "tax_total": 1.55
  },
]
```
Attribute | Description | Type
----- | ----- | -----
tax_name | Name of the tax. | string
bill_count_at_fullrate | Number of bills that had the full tax applied. | int
tax_amount_at_fullrate | Amount collected at the full tax rate. | decimal
bill_count_at_reducedrate | Number of bills that collected tax at a programmed reduced rate. | int
tax_amount_at_reducedrate | Amount collected at the reduced rate. | decimal
tax_total | Total tax collected. | decimal



# Index

Check out our other documentation in the [index](https://github.com/TouchBistro/sous-chef/blob/master/README.md).
