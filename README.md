# Introduction

All Tixton API end-points begin with https://api.tixton.com/v1/ and use the HTTPS protocol. Response
payloads are typically JSON. However, some end-points return only a simple string identifier.

## Rate Limiting

The current Tixton API rate limit is 200 requests per minute.

## Creating, Retrieving, Updating, Deleting Objects

Tixton API is designed to be [RESTful](http://en.wikipedia.org/wiki/Representational_state_transfer). 
An endpoint (URL) may support up to four different HTTP verbs. GET requests fetch information 
about an object, POST requests create objects, PUT requests update objects, and finally DELETE 
requests will delete objects.

## Authentication

There are two ways to pass the authentication token in Tixton API v1, using header or inside body parameter. It is recommended to use authentication header.

Request that requires authentication may return '404 Not Found', instead of '403 Forbidden'. This is to prevent the accidental leakage of private repositories to unauthorized users.

**Auth Token** (sent in a header)

```
curl -H "Authorization: Bearer JWT-TOKEN" https://api.tixton.com/v1
```

**Auth Token** (sent as a parameter)

```
curl -H https://api.tixton.com/v1/:endpoint?token=JWT-TOKEN
```


## Structure

Every response is contained within an envelope. The envelope has set of objects with 
which you can expect to interact:

```html
{
    "meta": {
        "code": 200,
        "message": "OK",
        "pagination": {
            "total": 18,
            "count": 15,
            "per_page": 15,
            "current_page": 1,
            "total_pages": 2,
            "links": {
                "next": "..."
            }
        }
    },
    "errors": {
        "password": "The password field is required"
    },
    "data": {
        ...
    },
    
}
```

**META**

The meta is used to communicate information about the response to the developer. Successful API call contains meta code '200' and message 'OK'.

For GET request that may return large set of data, meta includes pagination object. You can use the URL in ```links.next``` or ```links.prev``` for a convenient way to navigate through the sequence of data.

```html
{
    "meta": {
        "code": 200,
        "message": "OK",
        "pagination": {
            "total": 40,
            "count": 15,
            "per_page": 15,
            "current_page": 2,
            "total_pages": 3,
            "links": {
                "next": "...",
                "prev": "..."
            }
        }
    }
}
```

In case of unsuccessful request, our API server will return response such as:

```html
{
    "meta": {
        "code": 400,
        "message": "..."
    }
}
```

**ERRORS**

The errors object is used to display extra error information, will be useful especially for form validation errors. Sample errors can be found as follow:

```html
{
    "errors": {
        "password": "The password field is required",
        "name": "The name field is required"
    },
    "meta": {
        "code": 422,
        "message": "..."
    }
}
```

**DATA**

The data is the core of the response. It may be a single resource or collection.

Single resource is represented in json object:

```html
{
    "data": {
        "id": "PO201605251539-3FQ1AT",
        "name": "Foobar",
        "email": "foo@bar.com",
        "contact": "+6298324233",
    },
    "meta": {
        "code": 200,
        "message": "OK"
    }
}
```

While collection is represented in json array:

```html
{
    "data": [
        {
            "id": "PO201605251539-3FQ1AT",
            "name": "Foobar",
            "email": "foo@bar.com",
            "contact": "+6298324233",
        },
        {
            "id": "PO201605251539-3FQ1AT",
            "name": "Foobar",
            "email": "foo@bar.com",
            "contact": "+6298324233",
        },
    ],
    "meta": {
        "code": 200,
        "message": "OK",
        "pagination": {
            ...
        }
    }
    }
}
```

## Payment Method

| Code              | Description                                                                                                       |
|-------------------|-------------------------------------------------------------------------------------------------------------------|
| Braintree         | Purchasing using credit card will go through [Braintree](https://braintree.com), which is our payment gateway      |
| ManualTransfer    | We also offer bank transfer as our method of purchase. You must pay within 1 (one) hour after issuance of invoice  |
| Credit            | Credit method is for Tixton's existing users only. This method is available if and only if Tixton user's credit is sufficient to purchase the full price or payable is 0. Tixton user's credit includes Toncoin, coupon / bonus credit and cashable credit        |


# Static Data

## Currency

Available currency

| Code  | Description           |
|-------| ----------------------|
| IDR   | Indonesian Rupiah     |   
| USD   | US Dollar             |
| SGD   | Singapore Dollar      |
| MYR   | Malaysian Ringgit     |
| HKD   | Hongkong Dollar       |
| CNY   | Chinese Yuan Renminbi |
| JPY   | Japanese Yuan         |

## Cities

| Code  | Description           | Country       |
|-------|-----------------------|---------------|
| 1     | Jakarta               | Indonesia     |
| 2     | Bandung               | Indonesia     |
| 3     | Semarang              | Indonesia     |
| 4     | Yogyakarta            | Indonesia     |
| 5     | Surabaya              | Indonesia     |
| 13    | Lombok                | Indonesia     |
| 14    | Makassar              | Indonesia     |
| 15    | Bangka                | Indonesia     |
| 16    | Bintan                | Indonesia     |
| 17    | Laboan Bajo           | Indonesia     |
| 19    | Malang                | Indonesia     |
| 23    | Solo ( Surakarta )    | Indonesia     |
| 24    | Batam                 | Indonesia     |
| 27    | Batu                  | Indonesia     |
| 28    | Belitung              | Indonesia     |
| 29    | Magelang              | Indonesia     |
| 6     | Singapore             | Singapore     |
| 7     | Kuala Lumpur          | Malaysia      |
| 8     | Penang                | Malaysia      |
| 9     | Johor                 | Malaysia      |
| 20    | Langkawi              | Malaysia      |
| 21    | Malacca               | Malaysia      |
| 22    | Hongkong              | Hongkon       |


## Hotel stars

| Code  | Description   |
|-------| --------------|
| 3     | 3 stars       |
| 4     | 4 stars       |
| 5     | 5 stars       |

## Facility code

| Code  | Description   |
|-------| --------------|
| x     | 3 stars       |
| 4     | 4 stars       |
| 5     | 5 stars       |

# Errors Response

## Missing field

**Example response**

```html
{
    errors: {
        password: "The password field is required.",
        name: "The name field is required."
    },
    meta: {
        code: 422,
        message: 'VALIDATION_FAILED'
    }
}
```

# User

## Login

**API endpoint**

``` 
POST https://api.tixton.com/v1/auth/login 
```

**Parameters**


| Name     | Type   | Description                                       |
|----------|--------|---------------------------------------------------|
| email    | string | **Required**. A valid email address               |
| password | string | **Required**. A valid password                    |

**Example request**

```html
{
    "email": "foo@bar.com",
    "password": "fooBarMuse"
}
```

**Response**

HTTP : 200

The response to valid credentials is as following:

```
HTTP/1.1 200 OK
X-RateLimit-Limit : 200
X-RateLimit-Remaining : 199
```

```html
{
    "data": {
        "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbG9jYWxob3N0L3YxL2F1dGgvbG9naW4iLCJpYXQiOjE0NjQxNTQ0MzksImV4cCI6MTQ2NDE1ODAzOSwibmJmIjoxNDY0MTU0NDM5LCJqdGkiOiI4MTZkY2Q1ZDljMzA2MGMxY2FmNzhlOWQ1NGIxZmUzYyIsInN1YiI6M30.kwG8saUnElsD1tCS3FZMFiuHtSVfl8PF2XJ6UYDNebA"
    },
    "meta": {
        "code": 200,
        "message": "USER_LOGGED_IN"
    }
}
```

Afterwards, the token should be used in subsequent API call by inserting into HEADER Authorization.

HTTP : 401

The response to invalid credentials is as following:

```html
{
    "meta": {
        "code": 401,
        "message": "INVALID_CREDENTIALS"
    }
}
```



## Register

**API endpoint**

``` 
POST https://api.tixton.com/v1/auth/register 
```

**Parameters**

| Name              | Type          | Description                                               |
|-------------------|---------------|-----------------------------------------------------------|
| email             | string        | **Required**. A valid email                               |
| password          | string        | **Required**. A valid password, min 6 characters          |
| date_of_birth     | date          | **Required**. User date of birth, example ```1988-10-02```,  min age 16 years old  |
| contact           | numeric       | **Required**. Phone number, example ```+628181234567      |
| city_residence    | string        | **Required**. City of residence                           |
| base_currency     | integer       | **Required**. Please see currency section for details     |
| aff               | string        | Promo code                                                |

**Example request**

```html
{
    "email": "foo@bar.com",
    "password": "fooBarMuse123",
    "date_of_birth": "1990-10-02",
    "contact": "+628798733423",
    "city_residence": "Bandung, Indonesia",
    "base_currency": "IDR",
    "aff": "PromoCode"
}
```

**Response**

HTTP : 200

```
HTTP/1.1 200 OK
X-RateLimit-Limit : 200
X-RateLimit-Remaining : 199
```

```html
{
    "data": [],
    "meta": {
        "code": 200,
        "message": "USER_CREATED"
    }
}
```

HTTP : 422

```
HTTP/1.1 422 Unprocessable Entity
X-RateLimit-Limit : 200
X-RateLimit-Remaining : 199
```

```html
{
    "errors": {
        "email": "The email has already been taken.",
        "password": "The password field is required.",
        "name": "The name field is required.",
        "date_of_birth": "The date of birth field is required.",
        "contact": "The contact number field is required.",
        "city_residence": "The city of residence field is required.",
        "base_currency": "The currency field is required."
    },
    "meta": {
        "code": 422,
        "message": "VALIDATION_FAILED"
    }
}
```

Note

After user registers, an activation email will be sent to the specified email address.

## Activate

User needs to activate Tixton's account before using by using the activation link sent to the email address. 

**API endpoint**

```
GET https://api.tixton.com/v1/auth/activate/:confirmation_code
```

**Parameters**

| Name              | Type      | Description                                                 |
| ------------------|-----------|-------------------------------------------------------------|
| confirmation_code | string    | **Required**. Unique code in the email address verification |   


**Response**

HTTP : 200

```
HTTP/1.1 200 OK
X-RateLimit-Limit : 200
X-RateLimit-Remaining : 199
```

```html
{
    "data": [],
    "meta": {
        "code": 200,
        "message": "USER_ACTIVATED"
    }
}
```

HTTP : 400

If the token is not found or the user has already activated Tixton's account.

```html
{
    "meta": {
        "code": 400,
        "message": "TOKEN_NOT_FOUND"
    }
}
```



# Hotel API

## Search

The Search API is similar to performing a search on Google. It is optimized and designed to help find the hotel room based on the search criteria. The search result is paginated, and the paging information is described in ```meta.pagination```.

Tixton Search API provides up to 1,000 results for each search.

**API endpoint**

```
GET https://api.tixton.com/v1/search
```
**Parameter**

| Name          | Type      | Description                               |
|---------------|-----------|-------------------------------------------|
| city_id       | integer   | Please see static data                    |
| check_in      | date      | a valid date in yyyy-mm-dd, example ```2016-09-02```    |
| check_out     | date      | a valid date in yyyy-mm-dd, example ```2016-09-04```    |
| query         | string    | The search keywords, as well as any qualifiers.         |
| stars         | array     | The hotel stars, combination of 3, 4, 5                 |
| facility      | array     | Please see static data                    |
| currency      | string    | Please see static data                    |

**Example request**

```
GET https://api.tixton.com/v1/search?city_id=1&check_in=2016-02-02&check_out=2016-02-05&query=Fave&stars=3,4&facility=101,102,99&currency=IDR
```

**Response**

HTTP : 200

```html
{
    "data": [
        {
            "id": "L1605201535-WKWEXL",
            "name": "the Majesty Hotel Bandung",
            "address": "Jalan Suria Sumantri 91 Bandung 40164, Bandung",
            "relavance": 0,
            "duration": 1,
            "nights": "night",
            "check_in": "20 May 2016",
            "check_out": "21 May 2016",
            "type": "Superior",
            "capacity": 2,
            "price": {
                "currency": "IDR",
                "market": "1.331.000",
                "per_night": "393.976",
                "saving": "70"
            },
            "status": "On sale",
            "hotel": {
                "data": {
                    "id": 184,
                    "name": "the Majesty Hotel Bandung",
                    "address": "Jalan Suria Sumantri 91 Bandung 40164, Bandung",
                    "star": 4,
                    "check_in_time": null,
                    "check_out_time": null,
                    "description": null,
                    "ta_id": null,
                    "phone": null,
                    "g_place_id": null,
                    "thumbnail": {
                        "name": "Primary",
                        "url": "https://cdn03.tixton.com/img/business/t/h/business-the-majesty-hotel-bandung-hotel-bandung4011.jpg",
                        "extension": "jpg",
                        "mime": "image/jpg",
                        "width": null,
                        "height": null,
                        "size": null
                    },
                    "images": [
                        {
                            "name": "Primary",
                            "url": "https://cdn03.tixton.com/img/business/t/h/business-the-majesty-hotel-bandung-hotel-bandung4011.jpg",
                            "extension": "jpg",
                            "mime": "image/jpg",
                            "width": null,
                            "height": null,
                            "size": null
                        },
                        {
                            "name": "Swimming Pool",
                            "url": "https://cdn01.tixton.com/img/business/t/h/business-the-majesty-hotel-bandung-hotel-bandung4011.jpg",
                            "extension": "jpg",
                            "mime": "image/jpg",
                            "width": null,
                            "height": null,
                            "size": null
                        },
                    ],
                    "location": {
                        "lat": "-6.8834786",
                        "lng": "107.5816621"
                    },
                    "facilities": [
                        {
                            "id": 29,
                            "type": "Hotel",
                            "name": "24hr Room Service",
                            "created_at": "2016-03-16 09:12:35",
                            "updated_at": "2016-03-16 09:12:35",
                            "pivot": {
                                "hotel_id": 2200,
                                "master_id": 29,
                                "created_at": "2016-04-06 17:39:20",
                                "updated_at": "2016-04-06 17:39:20"
                            }
                        },
                        {
                            "id": 32,
                            "type": "Hotel",
                            "name": "Business Center",
                            "created_at": "2016-03-16 09:12:35",
                            "updated_at": "2016-03-16 09:12:35",
                            "pivot": {
                                "hotel_id": 2200,
                                "master_id": 32,
                                "created_at": "2016-04-06 17:39:20",
                                "updated_at": "2016-04-06 17:39:20"
                            }
                        },
                    ]
                }
            },
            "city": {
                "data": {
                    "id": 2,
                    "name": "Bandung",
                    "country": {
                        "data": {
                            "id": 1,
                            "name": "Indonesia"
                        }
                    }
                }
            }
        },
        ...
    ],
    "meta": {
        "code": 200,
        "pagination": {
            "total": 18,
            "count": 15,
            "per_page": 15,
            "current_page": 1,
            "total_pages": 2,
            "links": {
                "next": "...",
                "prev": "..."
            }
        }
    }
}
```

HTTP : 404

```html
{
    "meta": {
        "code": 404,
        "message": "SEARCH_NOT_FOUND"
    }
}
```

## View Detail

**API endpoint**

```
GET https://api.tixton.com/v1/listing/:listing_id
```

**Parameters**

| Name          | type      | description               |
|---------------|-----------|---------------------------|
| listing_id    | string    | **Required**. Listing id  |
| currency      | string    | See static data           |

**Response**

HTTP : 200

```html
{
    "data": {
        "id": "L1604211535-EKUPQS",
        "name": "Hotel Hyatt Jakarta",
        "address": "Jalan M H Thamrin Kav 28-3, Thamrin, Jakarta, Indonesia 10230",
        "relavance": 0,
        "duration": 2,
        "nights": "nights",
        "check_in": "23 May 2016",
        "check_out": "25 April 2016",
        "type": "DELUXE",
        "capacity": 8,
        "price": {
            "currency": "SGD",
            "market": "0.08",
            "per_night": "0.07",
            "saving": "15"
        },
        "status": "On sale",
        "hotel": {
            "data": {
                "id": 4,
                "name": "Hotel Hyatt Jakarta",
                "address": "Jalan M H Thamrin Kav 28-3, Thamrin, Jakarta, Indonesia 10230",
                "star": 4,
                "check_in_time": "13:00",
                "check_out_time": "12:00",
                "description": "....",
                "ta_id": 307132,
                "phone": "+1-309-587-8042",
                "g_place_id": null,
                "thumbnail": {
                    "name": "Primary",
                    "url": "http://api.tixton.app/images/hotel_init.png",
                    "extension": "jpg",
                    "mime": "image/jpg",
                    "width": null,
                    "height": null,
                    "size": null
                },
                "images": {
                    "0": {
                        "name": "Primary",
                        "url": "http://api.tixton.app/images/hotel_init.png",
                        "extension": "jpg",
                        "mime": "image/jpg",
                        "width": null,
                        "height": null,
                        "size": null
                    }
                },
                "location": {
                    "lat": "-6.1939058",
                    "lng": "106.8222198"
                },
                "facilities": []
            }
        },
        "city": {
            "data": {
                "id": 1,
                "name": "Jakarta",
                "country": {
                    "data": {
                        "id": 1,
                        "name": "Indonesia"
                    }
                }
            }
        }
    },
    "meta": {
        "code": 200,
        "message": "OK"
    }
}
```

HTTP : 404

```html
{
    "meta": {
        "code": 404,
        "message": "LISTING_NOT_FOUND"
    }
}
```

## Creating Purchase Order

**API endpoint**

```
POST https://api.tixton.com/v1/purchase/:serial
```

**Parameters**

| Name                  | Type      | Description                                                       |
|-----------------------|-----------|-------------------------------------------------------------------|
| serial                | string    | **Required**. Listing tixton serial number                        |
| name                  | string    | **Required**. The guest name                                      |
| email                 | string    | **Required**. The guest email                                     |
| contact               | numeric   | **Required**. The guest contact                                   |
| additional_request    | text      | The additional request                                            |
| currency              | string    | Please see static data, if not provided we will fallback into IDR |

**Notes**

By passing a valid authentication token inside the purchase order request, our system calculate the payable by adjusting with credit amount.

**Example request**

```html
{
    "name": "Foobar",
    "email": "foo@bar.com",
    "contact": "+6298324233",
    "additional_request": "Lula lop",
}
```

**Response**

HTTP : 200

```html
{
    "data": {
        "id": "PO201605251539-3FQ1AT",
        "name": "Foobar",
        "email": "foo@bar.com",
        "contact": "+6298324233",
        "additional_request": "Lula lop",
        "toncoin": 390080,
        "toncoin_cashable": 10,
        "bonus_credit": 0,
        "payable": 59910,
        "stay_night": 2,
        "check_in": "2017-02-18",
        "check_out": "2017-02-20",
        "price": {
            "currency": "IDR",
            "price": 450000
        },
        "token": "3gRq1234wewebhwe78y23423mnbhgwghfeweewreuI",
        "payment": {
            "status": "Pending",
            "method": null,
            "valid_until": null
        },
        "listing": {
            ...
        }
    },
    "meta": {
        "code": 200,
        "message": "OK"
    }
}
```

HTTP : 404

In the case of listing not found or already purchased, error code '404' is returned.

```html
{
    "meta": {
        "code": 404,
        "message": "LISTING_NOT_FOUND"
    }
}
```

HTTP : 500

In the case of system unable to create purchase for this listing, error code '500' is returned.

```html
{
    "meta": {
        "code": 500,
        "message": "FAILED_CREATE_PO"
    }
}
```

## Payment Purchase Order

Tixton accepts 3 payment methods:

1. Credit

This option is only available for Tixton's user and appears only when making purchase with zero payable. It means that the price of hotel room is covered fully using Tixton credit, with combination of Toncoin, coupon or cashable. User needs to be logged in and authenticated with valid JWTToken.

Below is sample to setup payment in the HTML page:

```html
<form class="" method="POST" action="https://api.tixton.com/v1/purchase/L23423423432-3FQ1AT">
  <input type="hidden" name="payment_method" value="Credit">
  <input type="hidden" name="_method" value="PUT">
  <input type="hidden" name="purchase_order" value="PO201605251539-3FQ1AT">
  <div class="text-center">
    <button type="submit" class="btn btn-orange btn-block btn-lg">PAY USING CREDIT</button>
  </div>
</form>
```

2. Braintree (credit card)

Braintree method should be chosen for credit card payment.

Using similar purchase data in previous example, below is how to setup payment using Braintree:

```html
<form class="" method="POST" action="https://api.tixton.com/v1/purchase/L23423423432-3FQ1AT">
  <input type="hidden" name="payment_method" value="Braintree">
  <input type="hidden" name="_method" value="PUT">
  <input type="hidden" name="purchase_order" value="PO201605251539-3FQ1AT">
  <div id="bt_dropin"></div>
  <div class="text-center">
    <button type="submit" class="btn btn-orange btn-block btn-lg">PAY IDR 59910</button>
  </div>
</form>
<script src="https://js.braintreegateway.com/v2/braintree.js"></script>
<script>
  function setupBT (argument) {
    braintree.setup("3gRq1234wewebhwe78y23423mnbhgwghfeweewreuI", "dropin", {
      container: 'bt_dropin'
    });
  }
  if (window.addEventListener) {
    window.addEventListener("load", setupBT, false);
  } else if (window.attachEvent) {
    window.attachEvent("onload", setupBT);
  } else {
    window.onload = setupBT;
  }
</script>
```

3. Bank Transfer

Payment using bank transfer is currently available only in IDR (Indonesian Rupiah). It must be communicated to user that the payment is to be made within 1 (one) hour after issuance of invoice. 

The setup is as following:

```html
<form class="" method="POST" action="https://api.tixton.com/v1/purchase/L23423423432-3FQ1AT">
  <input type="hidden" name="payment_method" value="ManualTransfer">
  <input type="hidden" name="_method" value="PUT">
  <input type="hidden" name="purchase_order" value="PO201605251539-3FQ1AT">
  <div class="text-center">
    <button type="submit" class="btn btn-orange btn-block btn-lg">BANK TRANSFER</button>
  </div>
</form>
```

**API endpoint**

```
PUT https://api.tixton.com/v1/purchase/:listing_serial
```

**Parameters**

| Name                  | Type      | Description                                                                       |
|-----------------------|-----------|-----------------------------------------------------------------------------------|
| listing_serial        | string    | **Required**. Listing tixton serial number                                        |
| purchase_order        | string    | **Required**. Purchase order                                                      |
| payment_method        | string    | **Required**. Braintree, Bank Transfer, Credit                                    |
| payment_method_nonce  | string    | **Required** if payment method using Braintree (generated by braintree client)    |

**Response**

HTTP : 200

```html
{
    "data": {
        "id": "PO201605251539-3FQ1AT",
        "name": "Foobar",
        "email": "foo@bar.com",
        "contact": "+6298324233",
        "additional_request": "Lula lop",
        "toncoin": "390080.00",
        "toncoin_cashable": "10.00",
        "bonus_credit": "0.00",
        "payable": "59910.00",
        "stay_night": 2,
        "check_in": "18 February 2017",
        "check_out": "20 February 2017",
        "price": {
            "currency": "IDR",
            "price": "450000.00"
        },
        "token": null,
        "payment": {
            "status": "Successful",
            "method": "Braintree",
            "valid_until": null // Jika menggunakan brank transfer valid until akan berisi waktu
        },
        "listing": {
            ...
        }
    },
    "meta": {
        "code": 200,
        "message": "SUCCESSFULL"
    }
}
```

HTTP : 404

If listing is not found, or already purchased:

```html
{
    "meta": {
        "code": 404,
        "message": "PO_NOT_FOUND"
    }
}
```

HTTP 500

If something goes wrong with the payment process:

```html
{
    "meta": {
        "code": 500,
        "message": "UNABLE_TO_PROCESS"
    }
}
```

## Confirm Purchase Order

After making bank transfer, user needs to confirm that the payment has been made. We have provided an API endpoint to handle this.

**API endpoint**

```
POST https://api.tixton.com/v1/purchase/confirm
```

**Parameters**

| Name              | Type      | Description                               |
|-------------------|-----------|-------------------------------------------|
| serial_number     | string    | **Required**. Purchase order number       |
| bank_name         | string    | **Required**. The bank name               |
| bank_account      | string    | **Required**. Your account bank number    |
| bank_account_name | string    | **Required**.                             |
| amount            | numeric   | **Required**.                             |

**Example request**

```html
{
    "serial_number": "PO201605251539-3FQ1AT",
    "bank_name": "BANK ABC",
    "bank_account": "999-2344-3334",
    "bank_account_name": "Edo Setiabudi",
    "amount": "59910.00"
}
```

**Response**

HTTP : 200

```html
{
    "data": {
        "id": "PO201605251731-FFEDRE",
        "name": "Foobar",
        "email": "foo@bar.com",
        "contact": "+6298324233",
        "additional_request": "Lula lop",
        "toncoin": "0.00",
        "toncoin_cashable": "0.00",
        "bonus_credit": "0.00",
        "payable": "450000.00",
        "stay_night": 2,
        "check_in": "2017-02-18",
        "check_out": "2017-02-20",
        "price": {
            "currency": "IDR",
            "price": "450000.00"
        },
        "token": null,
        "payment": {
            "status": "Successful",
            "method": "Bank Transfer",
            "valid_until": "2016-05-25 18:31:43"
        },
        "listing": {
            ...
        }
    },
    "meta": {
        "code": 200,
        "message": "SUCCESSFUL"
    }
}
```

HTTP : 404

```html
{
    "meta": {
        "code": 404,
        "message": "PURCHASE_NOT_FOUND"
    }
}
```

HTTP : 422

```html
{
    "errors": {
        "bank_account": "The bank account must be a number."
    },
    "meta": {
        "code": 422,
        "message": "VALIDATION_FAILED"
    }
}
```

HTTP : 500

```html
{
    "meta": {
        "code": 500,
        "message": "UNABLE_TO_PROCESS"
    }
}
```

## Sell Hotel

**API endpoint**

```
POST https://api.tixton.com/v1/booking
```

**Parameters**

| Name              | Type      | Description           |
|-------------------|-----------|-----------------------|
| country_id        | integer   | ..                    |
| city_name         | string    | ..                    |
| city_id           | integer   | ..                    |
| hotel_name        | string    | ..                    |
| check_in          | date      | ..                    |
| check_out         | date      | ..                    |
| room_type         | string    | ..                    |
| booker_name       | string    | ..                    |
| booker_contact    | numeric   | ..                    |
| booker_passport   | string    | ..                    |
| booking_src_id    | integer   | ..                    |
| booking_src_info  | .         | ..                    |
| booking_ref       | string    | ..                    |
| agreement         | bool      | ..                    |
| terms             | bool      | ..                    |


**Response**

HTTP : 200 

```html
{
    "data": {
        "id": "B1605231724-UBR7OW",
        "city_name": "Jakarta",
        "hotel_name": "Hotel Jakarta",
        "check_in": "26 May 2016",
        "check_out": "27 May 2016",
        "booker_name": "Purwandi M",
        "booking_src_info": null,
        "booking_ref": "REF",
        "remarks": null,
        "booker_passport": "PASS2432",
        "booker_contact": "+62873276432",
        "aff_id": null,
        "room_type": "Deluxe",
        "place_id": null,
        "meta": {},
        "pin_number": null,
        "status": "Rejected",
        "bookSrc": {
            "data": {
                "id": 1,
                "name": "Hotels.com",
                "link": "http://www.hotels.com"
            }
        }
    },
    "meta": {
        "code": 200,
        "message": "Successfully sell booking."
    }
}
```

HTTP : 422

```html
{
    "errors": {
        "city_name": "The city name field is required."
    },
    "meta": {
        "code": 422,
        "message": "VALIDATION_FAILED"
    }
}
```

# Travel Wish

## Create 

**API endpoint**

```
POST https://api.tixton.com/v1/travel-wish
```

**Parameters**

| Name          | Type      | Description               |
|---------------|-----------|---------------------------|
| name          | string    | Your name                 |
| email         | string    | Your valid email address  |
| contact       | numeric   | Your phone number         |
| city_id       | integer   | Please see static data    |
| check_in      | date      | Example 2017-02-01        |
| check_out     | date      | Example 2017-02-02        |
| num_rooms     | integer   | Number of rooms           |
| notes         | text      | Your traveling notes      |
| is_flexible   | boolean   | true or false             |

**Example requests**

```html
{
    'name': 'Member 2 Tixton',
    'email': 'member2@tixton.com',
    'contact': '08983125680',
    'city_id': 4,
    'check_in': "2017-02-01",
    'check_out': "2017-02-02",
    'num_rooms': 2,
    'notes': 'Ini dari testing doang',
    'is_flexible': true
}
```

**Response**

HTTP : 200

```html
{
    "meta": {
        "code": 200,
        "message": "CREATED"
    }
}
```

HTTP : 500

```html
{
    "meta": {
        "code": 500,
        "message": "UNABLE_CREATE"
    }
}
```

## Get Travel Wish 

**API endpoint**

```
GET http://api.tixton.com/v1/travel-wish
GET http://api.tixton.com/v1/me/travel-wish
```

**Response**

```html
{
    "data": [
        {
            "id": 2,
            "check_in": "2017-04-22",
            "check_out": "2017-04-24",
            "is_flexible": false,
            "is_completed": false,
            "num_rooms": 2,
            "token": "ujiCobaS",
            "city": {},
            "listing": {
                data: [] // available listings
            },
            "user": {}
        },
        {
            ...
        }
    ],
    "meta": {
        "code": 200,
        "message": "OK",
        "pagination": {
            "total": 2,
            "count": 2,
            "per_page": 15,
            "current_page": 1,
            "total_pages": 1,
            "links": []
        }
    }
}
```

## Detail Travel Wish


**API endpoint**

```
GET http://api.tixton.com/v1/travel-wish/:token-travel-wish
```

**Parameters**

| Name                  | Type      | Description           |
|-----------------------|-----------|-----------------------|
| :token-travel-wish    | string    | Token travel wish     |

**Response**

```html
{
    "data": {
        "id": 2,
        "check_in": "2017-04-22",
        "check_out": "2017-04-24",
        "is_flexible": false,
        "is_completed": false,
        "num_rooms": 2,
        "token": "ujiCobaS",
        "city": {},
        "listing": {
            data: [] // available listings
        },
        "user": {}
    },
    "meta": {
        "code": 200,
        "message": "OK",
    }
}
```

# Profile

## Me

**API endpoint**

```
GET /me
```

**Response**

```html
{
    "data": {
        "id": 2,
        "email": "member1@tixton.com",
        "roles": "member",
        "token_affiliate": null,
        "token_referral": "tXOBaQ",
        "social_avatar": null,
        "profile": {
            "data": {
                "name": "Member 1 Tixton",
                "date_of_birth": "2000-01-02",
                "contact": null,
                "city_residence": "Jakarta"
            }
        }
    },
    "meta": {
        "code": 200,
        "message": "OK"
    }
}
```

## User Credit

**API endpoint**

```
GET /me/credit
```

**Response**

```html
{
    "data": {
        "id": 2,
        "email": "member1@tixton.com",
        "roles": "member",
        "token_affiliate": null,
        "token_referral": "tXOBaQ",
        "social_avatar": null,
        "profile": {
            "data": {
                "name": "Member 1 Tixton",
                "date_of_birth": "2000-01-02",
                "contact": null,
                "city_residence": "Jakarta"
            }
        },
        "credit": {
            "data": {
                "currency": "IDR",
                "toncoin": {
                    "credit": "630050.00",
                    "active": "390080.00",
                    "hold": "239970.00"
                },
                "cashable": {
                    "credit": "10.00",
                    "active": "10.00",
                    "hold": "0.00"
                },
                "coupon": {
                    "credit": "30.00",
                    "active": "0.00",
                    "hold": "30.00"
                }
            }
        }
    },
    "meta": {
        "code": 200,
        "message": "OK"
    }
}
```

## Update profile

**API endpoint**

```
PUT https://api.tixton.com/v1/me/profile
```

**Parameters**

| Name              | Type      | Description           |
|-------------------|-----------|---------------------- |
| name              | string    | Name or user          |
| date_of_birth     | date      | Example 1987-09-02    |
| contact           | numeric   | Example +787832442    |
| city_residence    | string    | Example Jakarta       |  

**Example request**

```html
{
    "name": "Foobar",
    "date_of_birth": "1995-09-02",
    "contact": "+3242332434",
    "city_residence": "Jakarta"
}
```

**Response**

HTTP : 200

```html
{
    "meta": {
        "code": 200,
        "message": "PROFILE_UPDATED"
    }
}
``` 

HTTP : 500

```html
{
    "meta": {
        "code": 500,
        "message": "FAILED_UPDATE"
    }
}
``` 

## Update currency

**API endpoint**

```
PUT https://api.tixton.com/v1/me/currency
```

**Parameters**

| Name              | Type      | Description               |
|-------------------|-----------|---------------------------|
| base_currency     | string    | Please see static data    |

**Example request**

```html
{
    "base_currency": "IDR"
}
```

**Response**

HTTP : 200

```html
{
    "meta": {
        "code": 200,
        "message": "CURRENCY_CHANGED"
    }
}
``` 

HTTP : 400

```html
{
    "meta": {
        "code": 400,
        "message": "FAILED_UPDATE"
    }
}
``` 

```html
{
    "meta": {
        "code": 400,
        "message": "USER_HAVE_TRANSACTION"
    }
}
``` 

## Change Password

**API endpoint**

```
PUT https://api.tixton.com/v1/me/password
```

**Parameters**

| Name              | Type      | Description                   |
|-------------------|-----------|-------------------------------|
| current           | string    | The current user password     |
| password          | string    | The new user password         |
| confirm_password  | string    | The password confimation      |

**Example Request**

```html
{
    current : "password",
    password : "password123",
    confirm_password : "password123"
}
```

**Response**

HTTP : 200

```html
{
    "meta": {
        "code": 200,
        "message": "PASSWORD_CHANGED"
    }
}
``` 


HTTP : 400

```html
{
    "meta": {
        "code": 400,
        "message": "CURRENT_PASSWORD_SAME_NEW_PASSWORD"
    }
}
``` 

HTTP : 400

```html
{
    "meta": {
        "code": 400,
        "message": "CURRENT_PASSWORD_INVALID"
    }
}
```

HTTP : 500

```html
{
    "meta": {
        "code": 500,
        "message": "UNABLE_CHANGE"
    }
}
```