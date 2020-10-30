# T-Rex NVIDIA GPU miner

## Overview

T-Rex is a versatile cryptocurrency mining software. It supports a variety of algorithms and we, as developers, are trying to do our best to make it as	fast and as convenient to use as possible.

Developer fee is 1% (3% for Tensority).

## Usage

Full list of command line options:
```
    -a, --algo                     Specify the hash algorithm to use.
                                   astralhash
                                   balloon
                                   bcd
                                   bitcore
                                   c11
                                   dedal
                                   ethash
                                   geek
                                   hmq1725
                                   honeycomb
                                   jeonghash
                                   kawpow
                                   lyra2z
                                   megabtx
                                   megamec
                                   mtp
                                   mtp-tcr
                                   padihash
                                   pawelhash
                                   phi
                                   polytimos
                                   progpow
                                   progpow-veil
                                   progpow-veriblock
                                   progpowz
                                   sha256q
                                   sha256t
                                   skunk
                                   sonoa
                                   tensority
                                   timetravel
                                   tribus
                                   x11r
                                   x16r
                                   x16rt
                                   x16rv2
                                   x16s
                                   x17
                                   x21s
                                   x22i
                                   x25x
                                   x33
        --coin                     [Ethash, ProgPOW] Set coin name.
                                   Helps avoid DAG rebuilds when switching back from a dev fee session.
                                   Example: "eth" for Ethereum, "etc" for Ethereum Classic.
        --nonce-start              [ProgPOW] Starting nonce for the solution search.
        --nonce-range-size         [ProgPOW] Nonce range size for nonce search. The range will be split between all devices.
    -d, --devices                  Comma separated list of CUDA devices to use.
                                   Device IDs start counting from 0.
        --pci-indexing             Sort devices by PCI bus ID. Device IDs start with 0.
        --ab-indexing              Afterburner indexing (same as --pci-indexing but starts from 1).
    -i, --intensity                GPU intensity 8-25 (default: auto).
        --low-load                 Low load mode (default: 0). 1 - enabled, 0 - disabled.
                                   Reduces the load on the GPUs if possible. Can be set to a comma separated string to enable
                                   the mode for a subset of the GPU list (eg: --low-load 0,0,1,0)
        --kernel                   Choose kernel for Ethash (default: 0). Range from 0 to 5.
                                   Set to 0 to enable auto-tuning: the miner will benchmark each kernel and select the fastest.
                                   Can be set to a comma separated list to apply different values to different cards.
                                   (eg: --kernel 2,1,1,3)
                                   The support for this parameter may later be extended to cover other algorithms.
        --gpu-init-mode            Enables DAG sequential initialization (default: 0).
                                   0 - all GPUs are initialized in parallel
                                   1 - fully sequential initialization, one GPU at a time
                                   2 - two GPUs at a time
                                   etc.

    -o, --url                      URL of the mining pool in the following format: <scheme>://<host>:<port>
                                   Supported schemes: stratum+tcp
                                                      stratum+ssl
                                                      stratum2+tcp
                                                      stratum2+ssl
                                   stratum2 is normally used by Nicehash, MiningPoolHub and other similar mining pools
                                   Example: stratum+tcp://eu1.ethermine.org:4444
                                            stratum+ssl://zcoin.mintpond.com:3005
                                            stratum2+tcp://daggerhashimoto.hk.nicehash.com:3353
    -u, --user                     Username for mining server.
                                   If the username contains a dot (.) it is used as a separator between the wallet
                                   address and the worker name. If this behaviour is undesirable, set the worker name
                                   explicitly using "--worker" parameter. In that case the username will not be parsed,
                                   and will be sent to the pool as it is.
    -p, --pass                     Password for mining server.
    -w, --worker                   Worker name.
                                   Can be useful in case the username contains a dot (.) that should not be treated
                                   as a separator.
    -r, --retries                  Number of times to retry if a network call fails.
    -R, --retry-pause              Pause in seconds between retries.
    -T, --timeout                  Network timeout, in seconds (default: 300)
        --time-limit               Miner shutdown interval in seconds. (default: 0 - disabled)

        --temperature-color        Set temperature color for GPUs stat. Example: 55,65 - it means that
                                   temperatures above 55 will have yellow color, above 65 - red color. (default: 67,77)
        --temperature-limit        GPU shutdown temperature. (default: 0 - disabled)
        --temperature-start        GPU temperature to enable card after disable. (default: 0 - disabled)

    -b, --api-bind-telnet          IP:port for the miner API via telnet (default: 0.0.0.0:4068). Set to 0 to disable.
        --api-bind-http            IP:port for the miner API via HTTP (default: 0.0.0.0:4067). Set to 0 to disable.
    -J  --json-response            Telnet API server will make json responses.

    -N, --hashrate-avr             Sliding window length in seconds used to compute average hashrate (default: 60).
        --sharerate-avr            Sliding window length in seconds used to compute sharerate (default: 600).
        --gpu-report-interval      GPU stats report frequency. (default: 5. every 5th share)
    -q, --quiet                    Quiet mode. No GPU stats at all.
        --hide-date                Don't show date in console.
        --no-color                 Disable color output for console.
        --no-nvml                  Disable NVML GPU stats.
        --no-watchdog              Disable built-in watchdog.
        --watchdog-exit-mode       Specifies the action "A" the watchdog should take if the miner gets restarted "N" times
                                   within "M" minutes.
                                   Format: N:M:A. Valid values:
                                                  N: any positive integer,
                                                  M: any positive integer,
                                                  A: r(system reboot), s(system shutdown), e(miner exit)
                                   Actions "r" and "s" require running the miner with administrative privileges.
                                   Examples:
                                   20:10:s - watchdog will shutdown the system if the miner gets restarted 20 times
                                             within any 10 minute interval
                                   5:7:r   - watchdog will reboot the system if the miner gets restarted 5 times
                                             within any 7 minute interval

    -B, --benchmark                Benchmark mode.
        --benchmark-epoch          Epoch number used during benchmark (only for algorithms that generate DAG).
    -P, --protocol-dump            User protocol logging.
    -c, --config                   Load a JSON-format configuration file.
    -l, --log-path                 Full path of the log file.
        --cpu-priority             Set process priority (default: 2) 0 idle, 2 normal to 5 highest.

        --autoupdate               Perform auto update whenever a newer version of the miner is available.
        --back-to-main-pool-sec    Forces miner to switch back to main pool in case working with failover pool.
                                   Parameter is set in seconds. (default: 600)
        --exit-on-cuda-error       Forces miner to immediately exit on CUDA error.
        --exit-on-connection-lost  Forces miner to immediately exit on connection lost.
        --reconnect-on-fail-shares Forces miner to immediately reconnect to pool on N successively failed shares (default: 10).

        --fork-at                  Forces miner to change algorithm on predefined condition.
                                   Time condition: algo_name=YYYY-MM-DDTHH:MM:SS (eg: --fork-at x16rv2=2019-10-01T16:00:00).
                                   Time must be set in UTC+0.
                                   Block condition: algo_name=integer_block_number (eg: --fork-at x16rv2=6526421).
                                   To change main pool port you must write it right after algo: algo_name:integer_port_number
                                   (eg: --fork-at x16rv2:4081=2019-10-01T16:00:00).

        --mt                       Memory tweak mode (default: 0 - disabled). Range from 0 to 6. General recommendation
                                   is to start with 1, and then increase only if the GPU is stable.
                                   The effect is similar to that of ETHlargementPill.
                                   Supported on graphics cards with GDDR5 or GDDR5X memory only.
                                   Requires running the miner with administrative privileges.
                                   Can be set to a comma separated list to apply different values to different cards.
                                   Example: --mt 4 (applies tweak mode #4 to all cards that support this functionality)
                                            --mt 3,3,3,0 (applies tweak mode #3 to all cards except the last one)

        --version                  Display version information and exit.
    -h, --help                     Display this help text and exit.
```

## JSON config file

To start T-Rex with config file `config.txt` type in the console: `t-rex -c config.txt`. Use `config_example` file as a starting point to create your own config. You can also use environment variables: simply put `%YOUR_ENV_VAR%` anywhere in your config file and it will get automatically substituted with the value of `YOUR_ENV_VAR` variable at run-time.

## Watchdog

Watchdog is intended to observe miner state and restart T-Rex if it crashes or hangs for any reason.
Also, watchdog can optionally perform auto updates if a newer version is available.
We recommend using the watchdog to avoid any downtime in mining and make sure your GPUs are busy 24/7.
If you do need to disable the watchdog, you can do so using `--no-watchdog` parameter.

## HTTP API

By default HTTP API server binds to `0.0.0.0:4067`. It means that you can access your miner via both external and internal network interfaces.
Common example of request structure: `http://your_ip:your_port/handler_name`

* **trex** - Shows miner control monitoring page in your web browser. You can see miner stats realtime and also change miner parameters and config on the fly. Also here you will see updates in case it appears.
To activate the handler navigate to `http://127.0.0.1:4067/trex` using a web browser.

* **log** Displays the contents of the log file (if configured).
To activate the handler navigate to `http://127.0.0.1:4067/log` using a web browser.

* **config** - Changes your config on HDD and also change some miner parameters on the fly.
You can change multiple parameters with one request. GET and POST requests supported.
If you use config (started miner like this: t-rex.exe -c config_file) then any action with handler "config" will be saved into selected config_file.
You can use this handler for automatization purposes like changing config at runtime, shutting down the miner via API and then restarting it with new parameters applied.

  GET usage examples:
  1) `http://127.0.0.1:4067/config?protocol-dump=true` - Enables protocol dump on the fly and write it into config_file

  2) `http://127.0.0.1:4067/config?algo=x16r&devices=0,1&intensity=20,21` - Will write the following config settings into the config file: `algo=x16r`, `devices=0,1`, `intensity=20,21`

  3) `http://127.0.0.1:4067/config?algo=x16r&devices=0,1&intensity=20,21&config=test.conf` - Saves settings into the file `test.conf` which will be created in the folder where the miner resides. `algo=x16r`, `devices=0,1`, `intensity=20,21`

  4) `http://127.0.0.1:4067/config?config=test.conf` - Saves your current miner settings into `test.conf` file

  5) `http://127.0.0.1:4067/config` - Shows the current config state

  6) `http://127.0.0.1:4067/config?hashrate_avr=10&temperature-limit=70&temperature-start=40` - Sets the following parameters on the fly: `hashrate_avr=10`, `temperature-limit=70`, `temperature-start=40`

  For POST requests you must use correct json object with parameters you want to change.
  
  URL: `http://127.0.0.1:4067/config`.
  Payload: `{"hashrate_avr": 10, "temperature-limit": 70, "temperature-start": 40}`.
  Parameters' names and types in json are identical to config json you normally use.

* **summary** Shows all information about current mining process.
To activate the handler navigate to `http://127.0.0.1:4067/summary` using a web browser.

Response example with comments:
``` json5
{
  // Number of accepted shares count
  "accepted_count": 6,
  
  // Information about the pool your miner is currently connected to
  "active_pool":
  {
    // Current pool difficulty
    "difficulty": 5,
    
    // Pool latency
    "ping": 97,
    
    // Number of connection attempts in case of connection loss
    "retries": 0,
    
    // Pool connection string
    "url": "stratum+tcp://...",
    
    // Usually your wallet address
    "user": "..."
  },
  
  // Algorithm which was set in config
  "algorithm": "x16r",

  // HTTP API protocol version   
  "api": "1.2",

  // CUDA toolkit version used to built the miner
  "cuda": "9.10",

  // Software description
  "description": "T-Rex NVIDIA GPU miner",
  
  // Current network difficulty
  "difficulty": 31968.245093004043,

  // Total number of GPUs installed in your system
  "gpu_total": 1,
  
  // List of all currently working GPUs in your system with its stats
  "gpus": [{
    // Internal device id, useful for devs
    "device_id": 0,                        

    // Fan blades rotation speed in % of the max speed
    "fan_speed": 66,                       

    // User defined device id in config
    "gpu_id": 0,                        

    // Average hashrate per N sec defined in config
    "hashrate": 4529054,                   

    // Average hashrate per day
    "hashrate_day": 5023728,    

    // Average hashrate per hour
    "hashrate_hour": 0,          

    // Average hashrate per minute
    "hashrate_minute": 4671930,    

    // User defined intensity
    "intensity": 21.5,        

    // Current device name.
    "name": "GeForce GTX 1050",

    // Current device temperature.
    "temperature": 80,            

    // Current device vendor.
    "vendor": "Gigabyte", 

    // Device state. Might appear if device reached heat limit. (set by --temperature-limit)
    "disabled":true,                       

    // Device temperature at disable. Might appear if device reached heat limit.
    "disabled_at_temperature": 77
  }],
  
  // Total average sum of hashrates for all active devices per N sec defined in config.
  "hashrate": 4529054,                       

  // Total average sum of hashrates for a day.
  "hashrate_day": 5023728,                   

  // Total average sum of hashrates for an hour.
  "hashrate_hour": 0,                        

  // Total average sum of hashrates for a minute.
  "hashrate_minute": 4671930,                

  // Application name
  "name": "t-rex",

  // Operating system
  "os": "linux",

  // This is number of rejected shares count.
  "rejected_count": 0,                       

  // This is number of found blocks.
  "solved_count": 0,                

  // Current time in sec from the beginning of the epoch. (ref: https://www.epochconverter.com)
  "ts": 1537095257,                          

  // Uptime in sec. This shows how long the miner has been running for.
  "uptime": 108,                             

  // Miner version.
  "version": "0.6.5",

  // Information about available update. Appears in case update is available.
  "updates":{                                

    // Url of file archive to download.
    "url": "https://fileurl",          

    // Signature of update pack (md5).
    "md5sum": "md5...",               

    // T-Rex version in update.
    "version": "0.8.0",        

    // Short info about changes in update.
    "notes": "short update info",         

    // Whole info about changes in update.
    "notes_full": "full update info",     

    // Information about current update download.
    "download_status":
    {
      // Total bytes downloaded.
      "downloaded_bytes": 1775165,

      // Total bytes to download.
      "total_bytes": 5245345,

      // Last error if download failed.
      "last_error":"",

      // Time elapsed since first byte downloaded.
      "time_elapsed_sec": 2.887111,

      // Download service state.
      "update_in_progress": true,

      // Download service named state. ("started", "downloading", "finished", "error", "idle")
      "update_state": "downloading",

      // Url of file in operation.
      "url": "https://fileurl"
    },
  }
}
```

* **do-update** Activate download update service.
To start T-Rex update type in the following into your browser address bar and hit Enter:
`http://127.0.0.1:4067/do-update`.
To stop update type in the following into your browser address bar and hit Enter:
`http://127.0.0.1:4067/do-update?stop=true`

* **control** Real time configuration of T-Rex miner.
As of API 1.3 version there are following commands supported.

  _shutdown_ - Shuts down your miner. Usage: `http://127.0.0.1:4067/control?command=shutdown`. If you prefer POST set the request body to `{"command": "shutdown"}`.

  _hashrate-avr_ - Changes sliding window size in real time. Usage: `http://127.0.0.1:4067/control?hashrate-avr=1`.
It will set sliding window of size 1 sec. If you prefer POST set the request body to `{"hashrate-avr": 1}`.

  _gpu-report-interval_ - Changes frequency of GPUs reports appearance in log. Usage: `http://127.0.0.1:4067/control?gpu-report-interval=10`. Now you will see GPUs stats every 10th share. If you prefer POST set the request body to `{"gpu-report-interval": 10}`. Btw, you can disable stats (enter quiet mode) by setting gpu-report-interval to 0.

  _no-color_ - Disables color output to console. Usage: `http://127.0.0.1:4067/control?no-color=true`. To enable: `http://127.0.0.1:4067/control?no-color=false`.
If you prefer POST set the request body to `{"no-color": true}`.

  _protocol-dump_ - Enables user protocol dump into console/log. To enable: `http://127.0.0.1:4067/control?protocol-dump=true`. To disable: `http://127.0.0.1:4067/control?protocol-dump=false`.
If you prefer POST set the request body to `{"protocol-dump": true}`.

  _time-limit_ - Sets time limit in seconds for miner (it will shutdown after timeout). Usage: `http://127.0.0.1:4067/control?time-limit=120`. It will shutdown your miner 120 seconds after this request.
To disable: `http://127.0.0.1:4067/control?time-limit=0`. If you prefer POST set the request body to `{"time-limit": 120}`.

* **file** Creates a file in directory with the miner. This may be useful for external monitoring utilities or scripts which can take commands from this file and do some sort of useful stuff like overclock or system reconfiguration.

  `http://127.0.0.1:4067/file?name=test.txt&data=some_sort_of_text_data`

  If you prefer POST set the request body to `{"name": "test.txt", "data": "some sort of text data"}`

## Antivirus alerts

In order to protect the miner from reverse engineering attacks, the binaries are packed using a third-party software which mangles the original machine code. As a result, some antivirus engines may detect certain signatures within the executable that are similar to those that real viruses protected by the same packer have. In any case, it is advisable not to use cryptocurrency miners on the computers where you store your sensitive data (wallets, passwords etc.).

## Tips

In order to maximise the hashrate our software utilises all available GPU resources, so it is important that you review your overclock settings before you start mining. Our general recommendation is to start from GPU stock settings	(no overclock, default power limit), and then after making sure it is stable, slowly increase your overclock to find the "sweet spot" where the miner performs at its best and still does not crash.

## Support

Discord:
https://discord.gg/gj7jcYf

Bitcointalk:
https://bitcointalk.org/index.php?topic=4432704.0
