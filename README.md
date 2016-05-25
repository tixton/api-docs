# Introduction

All Tixton API end-points begin with https://api.tixton.com/v1/ and use the HTTPS protocol.

Response payloads are typically JSON; however, a few end-points return only a simple string identifier.

## Rate Limiting

The current Tixton API rate limit is 200 requests per minute.

## Deleting Objects

We do our best to have all our URLs be [RESTful](http://en.wikipedia.org/wiki/Representational_state_transfer). Every endpoint (URL) may support one of four different http verbs. GET requests fetch information about an object, POST requests create objects, PUT requests update objects, and finally DELETE requests will delete objects.

## Authentication


There are two ways to authenticate through Tixton API v1. Requests that require authentication will return 404 Not Found, instead of 403 Forbidden, in some places. This is to prevent the accidental leakage of private repositories to unauthorized users.

**Auth Token** (sent in a header)

```
curl -H "Authorization: Bearer JWT-TOKEN" https://api.tixton.com/v1
```

**Auth Token** (sent as a parameter)

```
curl -H https://api.tixton.com/v1/:endpoint?token=JWT-TOKEN
```


## Structure

Every response is contained by an envelope. That is, each response has a predictable set of keys with which you can expect to interact:

```json
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
    "erros": {
        "password": "The password field is required"
    },
    "data": {
        ...
    },
    
}
```

**META**

The meta key is used to communicate extra information about the response to the developer. If all goes well, you'll only ever see a code key with value 200. On the meta key also include pagination key. The pagination key is used to provided a convenient way to access more data in any request for sequential data. Simply call the url in the ```next``` parameter and we'll respond with the next set of data.

```json
{
    "meta": {
        "code": 200,
        "message": "OK",
        "pagination": {

        }
    }
}
```

However, sometimes things go wrong, and in that case you might see a response like:

```json
{
    "meta": {
        "code": 400,
        "message": "..."
    }
}
```

**ERRORS**

The errors key is used to display extra information about the form validation errors, and in that case you might see a response like:

```json
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

```json
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
| email    | string | **Required**. A valid email address from member   |
| password | string | **Required**. A valid password form member        |

**Example request**

```json
{
    "email": "foo@bar.com",
    "password": "fooBarMuse"
}
```

**Response**

HTTP : 200

If credentials is valid, you will be response like:

```
HTTP/1.1 200 OK
X-RateLimit-Limit : 200
X-RateLimit-Remaining : 199
```

```json
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

You must keep this token and add into HEADER Authorization parameter whenever you access our API.

HTTP : 401

But if the credentials is invalid you wull see a response like:

```json
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
| email             | string        | **Required**. A valid user email                          |
| password          | string        | **Required**. A valid password from user, min 6 char      |
| date_of_birth     | date          | **Required**. User date of birth, example ```1988-10-02```,  min age 16 years old  |
| contact           | numeric       | **Required**. Phone number                                |
| city_residence    | string        | **Required**. City of residence                           |
| base_currency     | integer       | **Required**. Please see currency section for details     |
| aff               | string        | Promo code                                                |

**Example request**

```json
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

```json
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

```json
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

After user registered, our system will sent a email verification.

## Activate

After user successfully registered our system. The user need verify their email address. 

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

```json
{
    "data": [],
    "meta": {
        "code": 200,
        "message": "USER_ACTIVATED"
    }
}
```

HTTP : 400

If the token is not found or the user is already activate their token.

```json
{
    "meta": {
        "code": 400,
        "message": "TOKEN_NOT_FOUND"
    }
}```



# Hotel API

## Search

The Search API is optimized to help you find the specific item you're looking for. Think of it the way you think of performing a search on Google. It's designed to help you find the one result you're looking for (or maybe the few results you're looking for). Just like searching on Google, you sometimes want to see a few pages of search results so that you can find the item that best meets your needs. To satisfy that need, the Tixton Search API provides up to 1,000 results for each search.

**API endpoint**

```
GET https://api.tixton.com/v1/search
```
**Parameter**

| Name          | Type      | Description                               |
|---------------|-----------|-------------------------------------------|
| city_id       | integer   | Please see static data                    |
| check_in      | date      | a valid date example ```2016-09-02```     |
| check_out     | date      | a valid date example ```2016-09-02```     |
| query         | string    | The search keywords, as well as any qualifiers. |
| stars         | array     | The hotel stars 3, 4, 5                   |
| facility      | array     | Please see static data                    |
| currency      | string    | Please see static data                    |

**Example request**

```
GET https://api.tixton.com/v1/search?city_id=1&check_in=2016-02-02&check_out=2016-02-05&query=Fave&stars=3,4&facility=101,102,99&currency=IDR
```

**Response**

HTTP : 200

```json
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

```json
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

```json
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
                "description": "Dolores a eius consequatur nihil nam eum. Odit molestiae placeat optio pariatur laborum maiores. Quos et repudiandae dignissimos ut quis voluptas quia. Quos quidem aut molestiae voluptatem minima cum odit consequatur. Pariatur rerum voluptatem reprehenderit laudantium et quasi. Ut quia reprehenderit accusantium esse et debitis. Non impedit est pariatur eum iste. Explicabo dolor corrupti officiis saepe officia. Autem quia non minus minus voluptates soluta esse omnis. Qui nemo explicabo qui molestias. Doloribus et sunt quas hic est. Error nulla similique temporibus quo.",
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

```json
{
    "meta": {
        "code": 404,
        "message": "LISTING_NOT_FOUND"
    }
}
```


## Buy Hotel

### Creating PO

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

If you passing HEADER ```Authorization: Bearer JWT-TOKEN``` on your request, our system will be verified your token and if your token is 
match in our user record, we will be charge some amount in user credit based on token request.

**Example request**

```json
{
    "name": "Foobar",
    "email": "foo@bar.com",
    "contact": "+6298324233",
    "additional_request": "Lula lop",
}
```

**Response**

```json
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
        "check_in": "18 February 2017",
        "check_out": "20 February 2017",
        "price": {
            "currency": "IDR",
            "price": 450000
        },
        "token": "3gRquI",
        "payment": {
            "status": "Pending",
            "method": "Log",
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

### Payment

**API endpoint**

```
PUT /purchase/:serial
```

**Parameters**

| Name                  | Type      | Description                       |
|-----------------------|-----------|-----------------------------------|
| serial                | string    | Listing tixton serial number      |
| purchase_order        | string    | Purchase order                    |
| payment_method        | string    | Braintree, Bank Transfer, Credit  |

**Response**

```json
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
            "method": "Log",
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

## Sell Hotel

**API endpoint**

```
POST /booking
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

```json
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


# Travel Wish

## Get Travel Wish 

## Create 

## Detail Travel Wish

# Profile

## Me

**API endpoint**

```
GET /me
```

**Response**

```json
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

```json
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
PUT /me/profile
```

**Parameters**

| Name              | Type      | Description           |
|-------------------|-----------|---------------------- |
| name              | string    | Name or user          |
| date_of_birth     | date      | Example 1987-09-02    |
| contact           | numeric   | Example +787832442    |
| city_residence    | string    | Example Jakarta       |  

**Response**

```json

``` 


## Update profile

**API endpoint**

```
PUT /me/currency
```

**Parameters**

| Name              | Type      | Description               |
|-------------------|-----------|---------------------------|
| base_currency     | string    | Please see static data    |

**Response**

```json

``` 

## Change Password

**API endpoint**

```
PUT /me/password
```

**Parameters**

| Name              | Type      | Description                   |
|-------------------|-----------|-------------------------------|
| current           | string    | The current user password     |
| password          | string    | The new user password         |
| confirm_password  | string    | The password confimation      |

**Example Request**

```json
{
    current : "password",
    password : "password123",
    confirm_password : "password123"
}
```

**Response**

```json

``` 