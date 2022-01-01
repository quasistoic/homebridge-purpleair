# Homebridge-purpleair
[![NPM Version](https://img.shields.io/npm/v/homebridge-airly.svg)](https://www.npmjs.com/package/homebridge-airly)

**Homebridge plugin that is showing information about air quality from PurpleAir API.**

Project is based on [homebridge-weather](https://github.com/werthdavid/homebridge-weather), [homebridge-arinow](https://github.com/ToddGreenfield/homebridge-airnow) and the original [homebridge-purpleair](https://github.com/SANdood/homebridge-purpleair).

## Installation
1. Install Homebridge using: `(sudo) npm install -g --unsafe-perm homebridge`.
1. Install this plugin:
    1. find the directory that `homebridge` was installed in (e.g. `/usr/local/lib/node-modules`)
    2. create `homebridge-purpleair` in that directory
    3. copy `index.js` and `package.js` into this directory
    4. make sure the file/directory ownership and RWX permissions are the same as other modules in that directory
1. Update your `homebridge` configuration file like the example below.

This plugin supports Air Quality, PM2.5, Temperature and Humidity.

## Configuration
Example config.json

```json
"accessories": [
    {
          "accessory": "PurpleAir",
          "purpleID": "PURPLE_AIR_STATION_ID",
          "updateFreq": 90,
          "name": "PurpleAir Air Quality",
          "statsKey": "v",
          "adjust": "NONE",
          "includePM10": true,
          "allowFlaggedData": false
    }
]
```

## Config file
Fields:
- `accessory` must be "PurpleAir" (required).
- `purpleID` PurpleAir Station ID (a number).
- `updateFreq` minimum number of seconds between reads from PurpleAir API (a number - default is 90 seconds)
- `name` Is the name of accessory (required).
- `statsKey` Selects the key from the sensor to report. The sensor reports various time based averages which can be selected. These are: v (real time), v1 (10 minute average), v2 (30 minute average), v3 (1 hour average), v4 (6 hour average), v5 (24 hour average), v6 (1 week average).
- `adjust` Adjust the raw PM2.5 value based on various algorithms. These are: NONE (raw values), EPA, LRAPA and AQANDU.
- `includePM10` Include PM10 measurements in the AQI calculation. The highest AQI calculated from PM2.5 and PM10 will be used to calculate the air quality. The AQI calculations from PM2.5 and PM10 are not the same. Value should be a literal true or false, not string quoted.
- `allowFlaggedData` Allows sensor data to be used even if it has been flagged by PurpleAir as potentially erroneous.

To find your specific "PURPLE_AIR_STATION_ID" (a string):
1. Use the PurpleAir Map to locate a station (https://www.purpleair.com/map)
1. Open this URL in a new Window or Tab: (https://www.purpleair.com/json)
1. Search for the NAME of the station you found in step A (*using JSONview in Google Chrome makes this a bit easier)*
1. The Station ID is the first element in the results[:] map - you will enter this ID (1-5 digits) into the preferences for the Air Quality Station
    1. If you have an outdoor sensor, there should be 2 entries in the big JSON file, one for each sensor. Please use only the FIRST entry - the code will find the second and average the values, as done for the PurpleAir map.
