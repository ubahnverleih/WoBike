# Emmy

[Emmy](https://emmy-sharing.de/) is a german rental service for electric 
motorbikes 🛵 (also called scooter, the wording is a mess). Currently, Emmy 
offers its service in Berlin, Hamburg and Munich.

## General stuff
API is based on exchanging json messages.
**Base url**: `https://api.emmy.ninja`

Below you find most API calls that the official emmy android app uses. However, 
some of them are not really tested. Additionally, there are even more API calls 
mostly related to the verification process (Upload video etc).

## Auth

### Login
```bash
curl --location --request POST 'https://api.emmy.ninja/auth/login' \
--header 'Content-Type: application/json' \
--data-raw '{
    "password": "pass",
    "username": "email"
}'
```

You'll receive a bunch of information related to your user. For authentication 
the `accessToken` is needed. With that, you can create the authorization header 
as following: `Authorization: Bearer $ACCESS_TOKEN`. Sometimes you need the 
`signupToken` instead of the accessToken. When ever this is needed the 
authorization is written that way: `Authorization: Bearer $SIGNUP_TOKEN`

### Logout
There is also an option to log out (invalidate the accessToken?)
tbc

### Reset Password

```bash
curl --location --request POST 'https://api.emmy.ninja/reset-password' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email": "mail@example.org"
}'
```

### Renew Password

```bash
curl --location --request POST 'https://api.emmy.ninja/users/$USER_ID/password' \
--header 'Authorization: Bearer $SIGNUP_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email": "mail@example.org",
    "oldPassword": "foo",
    "newPassword": "bar"
}'
```


## Vehicle related information
Some requests work without suppling authorization as they are public 
information.

### List vehicles
Lists all vehicles that are available for renting.

```bash
curl --location --request GET 'https://api.emmy.ninja/vehicles'
```

### Show vehicle

Show information for a specific vehicle by `$VEHICLE_ID`. Get the ID by 
calling the general `vehicles` endpoint. Note: The ID seems to be 
auto-increment. You'll find more vehicles by simply incrementing the ID - even 
those that are currently not available for rental.

```bash
curl --location --request GET 'https://api.emmy.ninja/vehicles/$VEHICLE_ID'
```

### Send damage report

When you notice a damage on a vehicle before you go on a ride, you can send a 
damage report. Note, that you need a rentalId for it that you obtain when 
[creating a rental](#create-rental) on a vehicle.

```bash
curl --location --request POST 'https://api.emmy.ninja/vehicles/$VEHICLE_ID/damages' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-raw '{
    "description": "A human readable description of the damage",
    "damages": [
        {
            "position": "foo",
            "type": "bar"
        }
    ],
    "rentalId": $RENTAL_ID
}' 
```


## Rental

### Create Rental
In order to create a rental, you need a `$VEHICLE_ID`.

```bash
curl --location --request POST 'https://api.emmy.ninja/vehicles/$VEHICLE_ID/rentals' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{}'
```
This starts a reservation and you have 15 min time to actually start the rental.
In the response you receive a rentalId that you have to store as `$RENTAL_ID` 
for later usage.

### Start Rental

:warning: Calling this method successfully will charge your account! You have to
pay the price that is given in the [vehicle information](#show-vehicle) as 
`pricingDuringRide`.

```bash
curl --location --request POST 'https://api.emmy.ninja/vehicles/$VEHICLE_ID/rentals/$RENTAL_ID/start' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "isCarClean": false,
    "isCarDamaged": false,
    "isDriversLicenceWithUser": false
}'
```

Note: you can set all values to `true` or `false`. The API doesn't care and will
start the rental either way.

### Stop Rental

```bash
curl --location --request POST 'https://api.emmy.ninja/vehicles/$VEHICLE_ID/rentals/$RENTAL_ID/stop' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{}'
```

### Pause Rental

You can also pause the rental if you want to park your vehicle and but want to 
use it later. This will reduce the price per minute as specified in 
`pricingDuringPark` in the [vehicle information](#show-vehicle)


```bash
curl --location --request POST 'https://api.emmy.ninja/vehicles/$VEHICLE_ID/rentals/$RENTAL_ID/park' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{}'
```


## User relatet stuff
The API exposes some information on the user.

### Create User
:warning: This Endpoint is not tested. No valid value for `languageId` is known
so far.

```bash
curl --location --request POST 'https://api.emmy.ninja/users' \
--header 'Content-Type: application/json' \
--data-raw '{
    "locationId": 1,
    "birthDate": "1980-01-01",
    "gender": 0,
    "firstName": "firstName",
    "lastName": "lastName",
    "email": "mail@example.org",
    "password": "password",
    "mobilePhone":"000000",
    "newsletterAccepted": false,
    "agbChecked": false,
    "planId": 1,
    "languageId": 0
}'
```

### User Information

```bash
curl --location --request GET 'https://api.emmy.ninja/users/$USER_ID' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

### User Statistics
Get interesting facts such as yout total number of rides or the total ridden 
distance in meters.

```bash
curl --location --request GET 'https://api.emmy.ninja/users/$USER_ID/stats' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

### List Rentals 

List all your previous rentals.

```bash
curl --location --request GET 'https://api.emmy.ninja/users/$USER_ID/rentals' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

### Show Rental

Show a specific rental by `$RENTAL_ID`.

```bash
curl --location --request GET 'https://api.emmy.ninja/users/$USER_ID/rentals/$RENTAL_ID' \
--header 'Authorization: Bearer $ACCESS_TOKEN' 
```

### Change Address

When you change you address, the old address is still stored and returnd as part
of the user information. The new address will become the new default.

```bash
curl --location --request POST 'https://api.emmy.ninja/users/$USER_ID/addresses' \
--header 'Authorization: Bearer $SIGNUP_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "firstName": "John",
    "lastName": "Doe",
    "street": "street name",
    "houseNumber": "99",
    "zipCode": "10000",
    "city": "city",
    "countryCode": "DE"
}'
```

### Show Payment Methods

List current payment options.

```bash
curl --location --request GET 'https://api.emmy.ninja/users/$USER_ID/payment-methods' \
--header 'Authorization: Bearer $SIGNUP_TOKEN'
```

### Payment Credit Card

:warning: This endpoint is not tested. Correct format of values are not known.

```bash
curl --location --request POST 'https://api.emmy.ninja/users/$USER_ID/payment-methods/cc' \
--header 'Authorization: Bearer $SIGNUP_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "number": "number",
    "expirationDate": "expDate",
    "cardHolder": "cardHolder",
    "type": "type",
    "cvc": "cvc"
}'
```

### Payment Bank Account

:warning: This endpoint is not tested. Correct format of values are not known.

```bash
curl --location --request POST 'https://api.emmy.ninja/users/$USER_ID/payment-methods/dd' \
--header 'Authorization: Bearer $SIGNUP_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "accountHolder": "accountHolder",
    "sepaMandateCheck": false,
    "iban": "iban",
    "bic": "bic"
}'
```


## Other stuff

### Plans

List available signup plan.

```bash
curl --location --request GET 'https://api.emmy.ninja/plans'
```

### Notifications

Sometimes, Emmy notifies its user about recent events such as an increase of 
prices, offline fleets because of bad weather condidition, etc.

```bash
curl --location --request GET 'https://api.emmy.ninja/notifications' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

### Locations

Get the business and non-parking territories.

```bash
curl --location --request GET 'https://api.emmy.ninja/locations'
```

### Buy Credit Packages

Emmy is offering prepaid credit packages so that you can lower the price per 
minute.

:warning: When submitting this request your account is charged depending on the
value of `$CREDIT_PACKAGE_CODE`.

:warning: This endpoint is not tested. No valid values for `$CREDIT_PACKAGE_CODE` are known so far. You may try one of the following: 
`["Asphalteuphoristen", "Gelegenheitsflaneure"]` cmp. 
[https://emmy-sharing.de/preise/](https://emmy-sharing.de/preise/)

```bash
curl --location --request POST 'https://api.emmy.ninja/credit-packages/buy' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "code": "$CREDIT_PACKAGE_CODE"
}'
```

### Validate Promotion Codes

:warning: This endpoint is not tested.

Check, if a promotion code is valid an can be redeemed. You need to have a
`$PROMOTION_CODE` that you might get from marketing actions. Use `SideMenu` for 
`$REDEMPTION_PURPOSE`. If you want to enter a promotion code from a friend,
use `Registration` instead

```bash
curl --location --request POST 'https://api.emmy.ninja/promotion-codes/validate' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-raw '{
    "code":"$PROMOTION_CODE",
    "redemptionPurpose": "$REDEMPTION_PURPOSE"
}'
```

### Redeem Promotion Codes

:warning: This endpoint is not tested.

```bash
curl --location --request POST 'https://api.emmy.ninja/promotion-codes/redeem' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer $SIGNUP_TOKEN' \
--data-raw '{
    "code": "$PROMOTION_CODE",
    "redemptionPurpose": "$REDEMPTION_PURPOSE"
}'

```

### Verification

:warning: Currently no understanding, what this endpoint is used for. 

Variable `$PLATFORM` must be one of the following: `["android", "ios"]`

```bash
curl --location --request GET 'https://api.emmy.ninja/verification/onfido-token' \
--header 'Emmy-Application-Platform: $PLATFORM' \
--header 'Authorization: Bearer $SIGNUP_TOKEN'
verification/onfido-token
```

The response contains a JWT. But what is it used for?