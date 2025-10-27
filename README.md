# AmerenIllinoisPSP
Home Assistant RESTful interfaces, templates, helpers, automation, and apexcharts config for bringing in Real Time Pricing for use in PredBat/BatPred

This downloads the JSON data from [Ameren Illinois' Power Smart Pricing](https://www.ameren.com/illinois/account/customer-service/bill/power-smart-pricing/prices) to support both Real Time Pricing (RTP) and Power Smart Pricing (PSP) programs for Time of Use (TOU) rate tariffs.

Ameren Illinois does not publish "tomorrow" prices until about 6PM. MISO energy publishes almost the right prices around 2PM, so I setup an account/subscription to get those updates 4 hours earlier and better plan for the next day.

The automation yaml is used to ensure we only get the data at the time it should be available or we need to update it just past midnight, reducing load on the APIs.

The delivered sensor template is for the Ameren Illinois Net Metering plan post 1/1/25. It also supports being on the ChargeSmart program for delivery discounts for shifting usage to preferred times. While the template improperly assumes that all NPCP hours get the surcharge, this helps ensure that PredBat will avoid importing during those times.

The sensor templates format the raw data into the structure [PredBat](https://springfall2008.github.io/batpred/) requires for octopus integration
Add the following lines in the PredBat apps.yaml:
  metric_octopus_import: sensor.ameren_hourly_delivered_price
  metric_octopus_export: sensor.ameren_hourly_supply_price
