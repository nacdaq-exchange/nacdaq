# API Documentation for [NACDAQ](https://nacdaq.pro)
> Version 0.1 beta, Updated on August 28, 2019. (C) Nacdaq<br>
> Please be aware, that API endpoints or response structure could change in time. Follow this doc or debug messages in API response to be in touch.

# API root
```https://api.nacdaq.pro```

# General information
+ Every request response consists of three elements:
  + status (explained below)
  + message (contains some additional info)
  + data (requested data, if there are no issues)
+ There is a limit of 5 requests per second to an endpoint from an IP address. Excessing the limit will lead to 2 minute blocking of the requesting IP address. Permanent exceeding the limit will lead to a permanent IP ban.

# Statuses
| Status code   | Explanation     | Possible solution  |
| ------------- |:-------------:| -----:|
| 0      | success |  |
| 400      | Bad input data      |   Check your input parameters |
| 403      | Authorization issue      |   Check "message" field in the response and your credentials |
| 404      | Not found    |   Check "message" field in the response |
| 2000 | API issue      |    Contact us if you got this |

# Authorization
You have to attach headers ```X-NACDAQ-API-KEY``` and ```X-NACDAQ-API-SECRET```, containing corresponding credentials (obtained in your profile) in your requests in order to pass API authorization. 
#### PHP example
```php
protected function ndCurl($url, $method = "GET", $data = false) { 
    // i.e. $url = https://api.nacdaq.pro/api/v1/currency/list
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    if ($method == "POST") {
        curl_setopt($ch, CURLOPT_POST, 1);
        curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
    }
    $headers = array(
        "X-NACDAQ-API-KEY: 12345",
        "X-NACDAQ-API-SECRET: 12345"
    );
    curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    $server_output = curl_exec($ch);
    curl_close ($ch);
    return json_decode($server_output);
}
```

# API endpoints
---
## Currency list
**GET** ```/api/v1/currency/list```<br>
Returns actual exchange currency list


## Pair list
**GET** ```/api/v1/pair/list```<br>
Returns actual exchange pair list


## Ticker data
**GET** ```/api/v1/ticker```<br>
Returns ticker data for symbol(s)
#### Parameters list
| Parameter  | Value     | Description  | Mandatory  |
| ------------- |:-------------:| -----:| -----:|
| symbol      | refer "Pair list" API method | market symbol | no |


## Order statuses
**GET** ```/api/v1/order/statuses```<br>
Returns possible order statuses


## Wallets and balances
**GET** ```/api/v1/wallet/own```<br>
Returns actual balances on user's wallets


## Own orders list
**GET** ```/api/v1/order/own```<br>
Return users' orders list
#### Parameters list
| Parameter  | Value     | Description  | Mandatory  |
| ------------- |:-------------:| -----:| -----:|
| filter_sell      | 0 or 1 | sell orders only | no |
| filter_buy      | 0 or 1  | buy orders only |   no |
| status | refer "Order statuses" API method      | order status |   no |
| pair | refer "Pair list" API method      | market symbol |   no |
| limit | >0      | limit returned orders count |   no |

## View order
**GET** ```/api/v1/order/XXXXXX```<br>
Returns order's with ID = XXXXXX data

## Cancel order
**GET** ```/api/v1/order/cancel/XXXXXX```<br>
Cancels order with ID = XXXXXX

## Market depth
**GET** ```/api/v1/order/marketdepth?symbol=XXX_XXX```<br>
Return market depth for symbol XXX_XXX
