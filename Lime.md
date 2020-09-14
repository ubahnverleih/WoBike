# Lime

**Base url**: `https://web-production.lime.bike/api/rider`

## Methods

### Login

Login is in two step:

+ Request OTP code with your phone number to receive the OTP code via SMS
+ Send back the OTP code to get a Auth token

#### Request sms code

:warning:  Account must exist  :warning:

**Method**: `GET`

**Path**: `/v1/login`

**Parameters**:

| Parameters | Descriptions             | Mandatory |
| ---------- | ------------------------ | :-------: |
| phone      | phone number intl format | X         |


**Example**

When entering a phone number, make sure to include the `%2B` before the number instead of the `+` (Example: %2B12222222222 is +1 (222) 222-2222)

```bash
curl --request GET \
  --url 'https://web-production.lime.bike/api/rider/v1/login?phone=%2B33612345678'
```

#### Send back OTP code

**Method**: `POST`

**Path**: `/v1/login`

**Header**:

| Header       | Value            | Mandatory |
| ------------ | ---------------- | :-------: |
| content-type | application/json | X         |

**Parameters**:

| Parameters | Descriptions             | Mandatory |
| ---------- | ------------------------ | :-------: |
| phone      | phone number intl format | X         |
| login_code | OTP code (length of 6)   | X         |


**Example**

```bash
curl --request POST \
  --cookie-jar - \
  --url 'https://web-production.lime.bike/api/rider/v1/login' \
  --header 'Content-Type: application/json' \
  --data '{"login_code": "123456", "phone": "+33612345678"}'
```

```JSON
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX3Rva2VuIjoiRk9PQkFSRUlSWkhBMiIsImxvZ2luX2NvdW50IjoyfQ.K5nmvUu92hYXQyeYG6O0rqo0ef2mkp7PMdtp9NrgwOE",
    "user": {
        "id": "FOOBAREIRZHA2",
        "type": "users",
        "attributes": {
            "token": "FOOBAREIRZHA2",
            "phone_number": "33612345678",
            "email_address": null,
            "has_verified_email_address": false,
            "name": "Lime Rider",
            "given_name": "Lime",
            "surname": "Rider",
            "default_payment_method": null,
            "referral_code": "REZHETH",
            "num_trips": 0,
            "edu": false,
            "subscription_item_states": [],
            "juicer_profile_status": null,
            "juicer_profile_initial_activated_at": null,
            "balance_cents": 0,
            "pending_balance_cents": 0,
            "currency": "USD"
        }
    }
}
```

Cookie


```
_limebike-web_session	U0pwQlVjcVRwMXZUTWovcHh3U251MERYTGE2dWpMdFdmNW9sL0d4SHBRVGtZd2VXN20yMjhJczhlR21nUkVHczlEREUweGNpYmtEWVFvQXBoQVNuRWdZWkVBajJPaHhDeStuUmttYVdYYWVsRDJEMUZvNE5YNU4xc1FlcjlDMi8xOVNMcDM3M3JhQWd0TDF2OWphMGR3PT0tLTBDYUtNeUJLcXRmNVd4YnorSEhlTWc9PQ%3D%3D--c8889db210d22bb4a96307b74d39c4b64d48777f
```

### Get bicycles by lat/lng

:warning: Auth (bearer AND cookie) are mandatory for this endpoint

**Method**: `GET`

**Path**: `/v1/views/map`

**Header**:

| Header        | Value        | Mandatory |
| ------------- | ------------ | :-------: |
| authorization | Bearer TOKEN | X         |

**Cookie**:

| Cookie                | Mandatory |
| --------------------- | :-------: |
| _limebike-web_session | X         |

**Parameters**:

| Parameters           | Descriptions | Mandatory | Remarks                                                             |
| -------------------- | ------------ | :-------: |---------------------------------------------------------------------|
| ne_lat               | Latitude     | X         | Bounding box for map; apparently ignored in favor of zoom parameter |
| ne_lng               | Longitude    | X         | Bounding box for map; apparently ignored in favor of zoom parameter |
| sw_lat               | Latitude     | X         | Bounding box for map; apparently ignored in favor of zoom parameter |
| sw_lng               | Longitude    | X         | Bounding box for map; apparently ignored in favor of zoom parameter |
| user_latitude        | Latitude     | X         |                                                                     |
| user_longitude       | Longitude    | X         |                                                                     |
| zoom                 | Integer      | X         | When < 15, bikes are clustered                                      |

**Example**

```bash
curl --request GET \
  --url 'https://web-production.lime.bike/api/rider/v1/views/map?ne_lat=52.6&ne_lng=13.5&sw_lat=52.4&sw_lng=13.3&user_latitude=52.5311&user_longitude=13.3849&zoom=16' \
  --header 'authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c' \
  --cookie '_limebike-web_session=N0xLYmE5ZytSSkRZa0FwQUdvYk1TalBaVWwzcnRDWUloT1Y1Z2ZNOVZSc0NCd3ZRZTFOVkxaS2lOcHFpemx6Y1pxT3ZudU1Zenk2ODlYRHBFZ1dxRWtaZGQybzRTQm96V09TWVdycENLcUltMHYzRWlUaEZlMDBqOCt4ODJqSWZwR09PSEtuNDdINnF3VGpkR3g2SjRBPT0tLTlJVHhSVFRDOE1CNm14S203VGxRd2c9PQ%253D%253D--9f55d56be64fefc5d5af3daf9e2fe9f7d7408cc0'
```

```JSON
{
    "data": {
        "id": "views::mapview",
        "type": "map_view",
        "attributes": {
            "user": {
                "id": "PSEALP4T4YYZX",
                "type": "users",
                "attributes": {
                    "token": "PSEALP4T4YYZX",
                    "phone_number": "18135196038",
                    "email_address": null,
                    "has_verified_email_address": false,
                    "name": "Lime Rider",
                    "given_name": "Lime",
                    "surname": "Rider",
                    "default_payment_method": null,
                    "referral_code": "R4Y6MQC",
                    "num_trips": 0,
                    "edu": false,
                    "subscription_item_states": [],
                    "promotion_notification": true,
                    "donation_profile": {
                        "id": "F7O24UILJM2E7",
                        "type": "donation_profiles",
                        "attributes": {
                            "donation_type": "none",
                            "donation_organizations": [],
                            "total_donation_amount": {
                                "currency_code": "USD",
                                "amount": 0.0,
                                "amount_str": "0.0",
                                "currency_symbol": "$",
                                "display_string": "$0"
                            },
                            "total_donation_amount_cents": null,
                            "currency": null
                        }
                    },
                    "pod_vehicle_banner_status": null,
                    "juicer_profile_status": null,
                    "juicer_profile_initial_activated_at": null,
                    "accepted_user_agreement_version": 2,
                    "accepted_compliance_version": null,
                    "balance": {
                        "currency_code": "USD",
                        "amount": 0.0,
                        "amount_str": "0.0",
                        "currency_symbol": "$",
                        "display_string": "$0"
                    },
                    "pending_balance": {
                        "currency_code": "USD",
                        "amount": 0.0,
                        "amount_str": "0.0",
                        "currency_symbol": "$",
                        "display_string": "$0"
                    },
                    "new_juicer_earnings_amount": {
                        "currency_code": "USD",
                        "amount": 150.0,
                        "amount_str": "150.0",
                        "currency_symbol": "$",
                        "display_string": "$150"
                    },
                    "new_juicer_earnings_promo_amount": "150",
                    "new_juicer_earnings_promo_amount_cents": 15000,
                    "new_juicer_earnings_promo_amount_cent": 15000,
                    "new_juicer_earnings_promo_currency": "USD",
                    "juicer_referral_type": null,
                    "juicer_referral_code": null,
                    "juicer_referral_amount": null,
                    "juicer_referral_amount_cents": null,
                    "juicer_referral_currency": null,
                    "balance_cents": 0,
                    "pending_balance_cents": 0,
                    "currency": "USD",
                    "show_group_ride": false,
                    "id_verification_pending": false,
                    "current_rental_id": null
                }
            },
            "regions": null,
            "zones": [
                {
                    "id": "OCH34CL3DMIXY",
                    "type": "zones",
                    "attributes": {
                        "name": "Selwyn Service Area",
                        "polyline": "n}fiGyax{_@bZ~T~UjUkIjZmt@tcDmYbtAkNlp@sIvY}Lv]sBuBgNq_@cd@yfAyOuYyP}[mQ{\\qSma@aWai@sr@w{AmWek@qRib@iTmd@}EcQmSs{@sUedAv|CszEzVknB{u@ekDoNgdBcTm\\kPat@lFsW~FmgAqr@yh@~Fct@yCiqApt@ucAtA_t@a@mU~FmQ|JaLjLuJnNwGxWoBwD}OuHqJob@wIse@c@_bA_WbSgcBnd@m\\zRkb@trA{qAwwAwbDgoBvaDinCvkA_xHfqBcm@z_HsIr`M_n@tyClo@nbDdfBlsAlj@rNljHbdB`{@rfA|^_k@dn@bmCtfAd_CndAh|B~lAn~BzVpm@pKxW~EpLvFfNvBrBeR|d@sb@tiAgZlRoXku@w[dWkSjOjCxV|DbWnBxIlCfInBnH|AbEw@t@m@f@uAtAqJad@aFaRuFiN{p@f^fFtRgX`QhO`k@sKjSxAnBvd@ph@}Wdk@|ElFvXrZxCsGv}@wjB_Okn@vDuBxf@~hBnPvi@nPhl@`_@}UzHcIz~@_z@_Twl@x`@ck@rm@gz@oKa_@id@rYiJc[qFzEgGiY`g@ca@_Ya_Aed@nc@aDoHyEvDyI{Zpg@}tAhTehAtUkgAtl@goC`Vm|@xIfNlAoB`AhQf@rAZtF`AK~Bt_@l^zJhAbRbGjaAvG_HfS}S|NiNrDZxAd@p@dB|AbBnBhAtCCdDqAYch@sSo@E{InA{bA_OmAkBqYMsUfI{FlNyL_EqRcU_CnGag@yf@yXwT{TeWleAgGeDgH`Q{NiLYhIcIzTsNePuSqYwJkHif@_d@if@q]_hBlTiA{oAyY`\\iUy_@aFpEkLrKeXrVaJjMaJ}CkRjg@nF`DuWrr@rSpR`[iw@tGpGdCkDh_@b[lMpNlGiEvWnSvOk\\LqPvCqa@tLkAfpA{IdmAd`A~Yv_@cPxq@",
                        "category": "service_zone",
                        "icon_latitude": "-43.559385",
                        "icon_longitude": "172.56689",
                        "show_icon": false,
                        "zone_styling_id": "116"
                    }
                },
                
                ...
            ],
            "bike_clusters": null,
            "bikes": [
                {
                    "id": "ET-HV36KUOCA4ZZNPGB3B4JKCGRPJHQWAWPFUC4BYY",
                    "type": "bikes",
                    "attributes": {
                        "status": "locked",
                        "plate_number": "XXX-683",
                        "latitude": -43.532048,
                        "longitude": 172.636678,
                        "last_activity_at": "2020-09-14T06:21:22.000Z",
                        "type_name": "scooter",
                        "battery_level": "high",
                        "meter_range": 22247,
                        "rate_plan": "NZD $1 to unlock +\nNZD $0.38 / 1 min",
                        "bike_icon_id": 50,
                        "last_three": "683",
                        "license_plate_number": null,
                        "brand": "lime"
                    }
                },
                
		...
            ],
            "selected_bike": null,
            "bike_pins": [],
            "selected_bike_pin": null,
            "parking_spots": null,
            "icons": [
                {
                    "id": "50",
                    "type": "icons",
                    "attributes": {
                        "url": "https://d22d5yy1i19g9i.cloudfront.net/icons/scooter_high_battery.png?fingerprint=a14ab0c75f4838b41729eb1ee746dae9",
                        "description_icon_url": null,
                        "description_link_url": null,
                        "description": "translation missing: en.scooter_high_battery"
                    }
                },
                
		...
            ],
            "current_level": "city",
            "levels": {
                "region": 7.0,
                "subregion": 11.0,
                "city": 14.0
            },
            "most_recent_trip": null,
            "banner": null,
            "donation_organization": null,
            "parking_spots_radius_meters": 20,
            "map_pins": [],
            "group_ride_id": null,
            "group_ride_participants": null
        }
    },
    "meta": {
        "latest_user_agreement_version": "3",
        "min_ios_version": "2.11.0",
        "min_android_code": 93,
        "flags": "IOS_VERSION_V1.0,ANDROID_VERSION_V1.0",
        "groups": {
            "braze_integration": true,
            "take_photo": "find_my_ride",
            "show_battery_level_on_map": true,
            "unlock_confirmation": false,
            "short_stop": true,
            "bluetooth_unlock": true,
            "parked_or_not": false,
            "branch_link_referral": true,
            "persist_rate_ride": true,
            "no_header_map": true,
            "fetch_end_trip_experience": true,
            "long_pause_group": false,
            "group_ride_v2": false,
            "group_ride_max_rider_cap": "5",
            "group_ride_max_guest_cap": "10",
            "id_scanner": "google_vision",
            "id_scan_show_manual_delay_sec": 15,
            "new_method_completing_trip": true,
            "cvv_optional": false,
            "auto_reload_on_by_default": true,
            "group_ride_revamp": false,
            "auto_reload_experience_updates": true,
            "auto_reload_always_enabled": false,
            "remove_add_balance_confirmation": false,
            "scooter_reservation": true,
            "show_group_ride_tutorial": true,
            "use_client_groups": false,
            "use_moshi": false,
            "android_trip_events_refactor": false,
            "android_how_to_ride_tutorial": false,
            "timeout_retry": false,
            "cancel_reserve_red_button": true,
            "show_loyalty": true,
            "group_ride": false,
            "show_self_help": false,
            "show_loyalty_spend_points": false,
            "unlock_button_group": "white_bar",
            "ride_history_with_pagination": true,
            "map_style_with_poi": true,
            "default_to_unlock_group": "control",
            "recommend_nearby_bike_v2": true,
            "customized_speed_cap_enabled": false,
            "comfort_ride_enabled": false,
            "server_side_id_verification": false,
            "request_bluetooth_permission_at_unlock_screen": null,
            "donation_group": "control",
            "enable_stripe_sca_flow": true,
            "use_trip_summary_v2": false,
            "top_banner_carousel": false,
            "use_banner_v2": true,
            "use_new_microblink": true,
            "use_tutorial_v2": false,
            "use_app_refresh_theme": false,
            "simplify_unlock_new_map_experience": false,
            "map_improvements": {
                "battery_sort": true,
                "cache_zones": true,
                "clustering": true,
                "fetch_zones": false,
                "icons": true
            },
            "use_referrals_v2": false,
            "zip_code_country_selector": false,
            "group_ride_entry_point": "default",
            "zone_messaging_enabled": true,
            "area_rate_plan_on_scanner": true,
            "backend_driven_trip_rating": true,
            "bike_reservation": true,
            "enable_promo_notification_optout": true,
            "show_auto_reload": true,
            "map_levels_v1": true,
            "map_balance_icon": false,
            "paypal_payment_method": false,
            "payment_zip_code": false,
            "payment_billing_address": false,
            "payment_creation_ui": true,
            "payment_cardholder_name": false,
            "card_scanner": null,
            "apple_pay_address_ui": false,
            "enable_google_pay": false,
            "payments": {
                "cvv_optional": false,
                "payment_card_io_scan": null,
                "payment_creation_ui": true,
                "payment_billing_address": false,
                "payment_cardholder_name": false,
                "payment_zip_code": false
            },
            "apple_pay": true,
            "enable_juicer_settings": true,
            "enable_juicer_in_app_message": true,
            "ios_juicer_multi_task_v1": true,
            "android_juicer_multi_task_v1": true,
            "complete_trip_before_take_photo": true,
            "android_upload_juicer_attributes": true,
            "enable_juicer_endpoint_v2": true,
            "juicer_bluetooth_unlock": false,
            "android_enable_backend_driven_cancel_task": true,
            "android_tooltip_v2": true,
            "android_enable_bulk_scan_of_deploy_tasks": null,
            "ios_juicer_task_bundle": null,
            "android_juicer_task_bundle": null,
            "enable_juicer_vat_profile": null,
            "enable_juicer_backend_banner": false,
            "enable_juicer_in_app_funnel": false,
            "use_backend_driven_earn_with_lime_button": false,
            "use_backend_driven_switch_to_juicer_mode_button": false,
            "juicer_earnings_notification": true,
            "juicer_satellite_mode": true,
            "juicer_level_2": true,
            "ios_juicer_rebalance_v1": false,
            "android_juicer_rebalance_v1": false,
            "juicer_hide_scooter_details": false,
            "enable_juicer_mock_gps_blocker": true,
            "edit_email_magic_link": false
        },
        "trip_id": null,
        "trip_status": null,
        "group_id": null,
        "group_ride_status": null,
        "group_ride_version": "legacy",
        "notifications": [],
        "messages": [
            {
                "message_token": "VQAWAKHROENWK",
                "template_token": "RTDOBHRHCNIBW",
                "title": "Update",
                "image_title": "image_title",
                "lifecycle": "asap",
                "body": "We've updated our Privacy Notice.",
                "user_input": false,
                "image_url": "https://limebike-web-public-assets.s3-us-west-1.amazonaws.com/messages/vafxp85kp4%403x.png",
                "image_medium_url": "https://limebike-web-public-assets.s3-us-west-1.amazonaws.com/messages/cnwneo6ogoi%402x.png",
                "image_small_url": "https://limebike-web-public-assets.s3-us-west-1.amazonaws.com/messages/o0t9sa64ut%401x.png",
                "layout_type": "basic",
                "button_action": "url_link",
                "button_text": "Learn More",
                "text_input_hint": "",
                "option_text": "",
                "button_action_link": {
                    "link": "https://www.li.me/privacy/"
                },
                "button_action_email": null
            }
        ],
        "country_code": "US",
        "user_agreement_version": 3,
        "user_agreement_country_code": "US",
        "need_to_show_agreement": true,
        "user_agreement_url": "https://li.me/user-agreement/us",
        "user_agreement_title": "We've Updated Our User Agreement",
        "user_agreement_message": "By tapping \"I Agree\" You confirm that You have read, understood, and agreed to Lime’s updated User Agreement.",
        "user_agreement_primary_button": "I Agree",
        "user_agreement_view_terms_button": "View Agreement",
        "need_to_show_group_ride_agreement": true,
        "current_rental_id": null,
        "current_rental_status": null
    }
}
```

## Implementations

* https://www.npmjs.com/package/@multicycles/lime (JavaScript)
