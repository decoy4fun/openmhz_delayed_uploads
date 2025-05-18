# OpenMHz Delayed Upload Script

This script monitors a directory for new `.m4a` audio files from TrunkRecorder and their associated `.json` metadata files, and uploads them to OpenMHz after a specified delay. Script was designed for calls from TrunkRecorder but with slight modification can be used with almost any service. 

This was pieced together with existing scripts found on the TrunkRecorder Dicord from SaintOlav and BinarySentinel, as well as the assistance and ideas from TheGreatCodeHolio, Jodfie, and tadscottsmith. Most modifications made with ChatGPT. All this to say this is mostly molding what others have already done to fit my own needs. 

**Pull Requests are welcome, encouraged, and invited. I have no idea how efficient or inefficient this code is or if there's a better way to handle it**

---

## Configuration

Save the `openmhz_delayed.sh` and `openmhz_delayed_config.json` files. Edit the config file as appropriate for your system(s) and save. Edit the location for your config file in the .sh script to point to your config file. Install `openmhz_delayed.sh` as a service so it's constantly monitoring and you're set! 

---

### Global Configuration

The global configuration applies to all systems unless overridden by system-specific settings.

| Key               | Type   | Default Value | Description                                                                 |
|-------------------|--------|---------------|-----------------------------------------------------------------------------|
| `globalWatchDir`  | String | -             | The base directory where the script will look for audio files.              |

---

### System Configuration

Each system configuration can override the global settings. The following keys are available for each system:

| Key               | Type   | Default Value          | Description                                                                 |
|-------------------|--------|------------------------|-----------------------------------------------------------------------------|
| `systemName`      | String | -                      | The name of the system. Used to create subdirectories and log entries.      |
| `shortName`       | String | `systemName`           | A short name for the system, used in the OpenMHz API URL.                   |
| `openmhzKey`      | String | -                      | The API key used to authenticate with the OpenMHz API.                      |
| `uploadDelay`     | Number | -                      | The delay (in seconds) before uploading a file after it is created.         |
| `minFileAge`      | Number | `10`                   | The minimum age (in seconds) a file must be before it can be uploaded.      |
| `talkgroupsToSkip`| String | -                      | A comma-separated list of talkgroup IDs to skip during upload.              |
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
