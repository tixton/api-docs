# Introduction

All Tixton API end-points begin with https://api.tixton.com/v1/ and use the HTTPS protocol.

Response payloads are typically JSON; however, a few end-points return only a simple string identifier.

# Rate Limiting

The current Tixton API rate limit is 200 requests per minute.

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

# Authenticate User

## Login a user

Example request

``` 
POST /auth/login 
```

Parameters


| Name     | Type   | Description                       |
|----------|--------|-----------------------------------|
| email    | string | A valid email address from member |
| password | string | A valid password form member      |

Response

```json
[
    {
        "id": 3,
        "organization": "Snappy Help",
        "domain": "help.besnappy.com",
        "plan_id": 1,
        "active": 1,
        "created_at": "2012-12-05 15:24:20",
        "updated_at": "2013-05-07 19:48:06",
        "custom_domain": ""
    }
]
```

## Register a user

Example request

``` 
POST /auth/register 
```

Parameters

| Name              | Type          | Description                               |
|-------------------|---------------|-------------------------------------------|
| email             | string        | A valid user email                        |
| password          | string        | A valid password from user, min 6 char    |
| date_of_birth     | date          | Date of birth, example ```1988-10-02```   |
| contact           | numeric       | Phone number                              |
| city_residence    | string        | City of residence                         |
| base_currency     | integer       | Please see currency section for details   |
| aff               | alphanumeric  | Promo code                                |

Response

```header

```

```json

```

# Hotel API

## Search

Example request

```
GET /listing
```
Parameter

| Name          | Type      | Description                               |
|---------------|-----------|-------------------------------------------|
| city_id       | integer   | Please see static data                    |
| check_in      | date      | a valid date example ```2016-09-02```     |
| check_out     | date      | a valid date example ```2016-09-02```     |
| query         | string    |                                           |
| stars         | array     | 3, 4, 5                                   |
| facility      | array     |                                           |

Response

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
                        "url": "https://cdn03.tiket.photos/img/business/t/h/business-the-majesty-hotel-bandung-hotel-bandung4011.jpg",
                        "extension": "jpg",
                        "mime": "image/jpg",
                        "width": null,
                        "height": null,
                        "size": null
                    },
                    "images": [
                        {
                            "name": "Primary",
                            "url": "https://cdn03.tiket.photos/img/business/t/h/business-the-majesty-hotel-bandung-hotel-bandung4011.jpg",
                            "extension": "jpg",
                            "mime": "image/jpg",
                            "width": null,
                            "height": null,
                            "size": null
                        },
                        {
                            "name": "Swimming Pool",
                            "url": "https://cdn01.tiket.photos/img/business/t/h/business-the-majesty-hotel-bandung-hotel-bandung4011.jpg",
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
        {
        
        }
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
                "next": "http://tixton.app/api/v1/search?query=&city_id=&country_id=&check_in=&check_out=&facilities=&stars=&order=&currency=IDR&locale=en&page=2"
            }
        }
    }
}

```

## View Detail

```
GET /listing/L3123122321
```

Response

```json


```

## Buy Hotel

## Sell Hotel

# Travel Wish

## Create 

## Detail Travel Wish

# Profile
