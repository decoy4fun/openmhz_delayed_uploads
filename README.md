# OpenMHz Delayed Upload Script

This script monitors a directory for new `.m4a` audio files from TrunkRecorder and their associated `.json` metadata files, and uploads them to OpenMHz after a specified delay. Script was designed for calls from TrunkRecorder but with slight modification can be used with almost any service. 

This was pieced together with existing scripts found on the TrunkRecorder Dicord from SaintOlav and BinarySentinel, as well as the assistance and ideas from TheGreatCodeHolio, Jodfie, and tadscottsmith. Most modifications made with ChatGPT. All this to say this is mostly molding what others have already done to fit my own needs. 

**Pull Requests are welcome, encouraged, and invited. I have no idea how efficient or inefficient this code is or if there's a better way to handle it**

---

## Installation/Usage

Save the `openmhz_delayed.sh` and `openmhz_delayed_config.json` files. Edit the config file as appropriate for your system(s) and save. Edit the location for your config file in the .sh script to point to your config file. Install `openmhz_delayed.sh` as a service so it's constantly monitoring and you're set! 

Make sure you remove any OpenMhz upload variables from your Trunk-Recorder config file to avoid duplicate uploads. 

---

### Global Configuration

The global configuration applies to all systems unless overridden by system-specific settings.

| Key              | Type   | Default Value | Description                                                                  |
|------------------|--------|----------------|------------------------------------------------------------------------------|
| `globalWatchDir` | String | *(required)*   | The base directory where the script will look for audio files.              |
| `alertLevel`     | String | `error`        | Minimum log level to trigger alerts. Options: `warn`, `error`. `info` would be too spammy and is not enabled. Setting to `warn` will send all warnings and errors. Setting to error will only send errors, not warnings.  |
| `alertEmail`     | String | *(optional)*   | If enabled, this will send an email for every log entry based on `alertLevel`. Requires `mail` or `msmtp` to be installed.   |
| `alertWebhook`   | String | *(optional)*   | Discord webhook URL to receive alert messages if enabled. Sends a message for every log entry based on `alertLevel`                          |


---

### System Configuration


| Key               | Type   | Default Value          | Description                                                                 |
|-------------------|--------|------------------------|-----------------------------------------------------------------------------|
| `systemName`      | String | -                      | The name of the system (matching trunk-recorder). Also acts as the default OpenMhz system name      |
| `shortName`       | String | `systemName`           | Defaults to systemName. Only required if you have multiple systems you want to upload to one system in OpenMhz               |
| `openmhzKey`      | String | -                      | OpenMhz API Key                     |
| `uploadDelay`     | Number | -                      | The delay (in seconds) before uploading a file after it is created. I've tested between 5-15 minutes successfully.       |
| `minFileAge`      | Number | `10`                   | The minimum age (in seconds) a file must be before it can be uploaded. (recommend 5-10 seconds if you are uploading to broadcastify or rdio within trunk-recorder)    |
| `talkgroupsToSkip`| String | -                      | A comma-separated list of talkgroup IDs to skip during upload. (Use this for car to car or tactial groups you may want to record, but don't want uploaded.)           |
| `watchDir`        | String | `globalWatchDir/systemName` | The directory where the script will look for audio files for this system.   |

---

## Example Configuration

Here’s an example `openmhz_delayed_config.json` file:

```json
{
  "globalWatchDir": "/home/ubuntu/Automations/OpenMhz/watch",
  "systems": [
    {
      "systemName": "System1",
      "shortName": "Sys1",
      "openmhzKey": "your_api_key_here",
      "uploadDelay": 60,
      "minFileAge": 10,
      "talkgroupsToSkip": "123,456",
      "watchDir": "/home/ubuntu/Automations/OpenMhz/watch/System1"
    },
    {
      "systemName": "System2",
      "openmhzKey": "your_api_key_here",
      "uploadDelay": 120,
      "minFileAge": 15
    }
  ]
}
```

## Notes
* The script skips uploading files for talkgroups listed in `talkgroupsToSkip`.

* Files are moved to an `_uploaded` subdirectory after successful upload.

* The script retries uploading files if the initial attempt fails.

## Dependencies
* jq: A lightweight and flexible command-line JSON processor.

* curl: A command-line tool for transferring data with URLs.
