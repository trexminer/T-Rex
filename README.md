# T-Rex NVIDIA GPU miner (Ethash / Autolykos2 / Kawpow / Octopus / MTP)

## Overview

T-Rex is a versatile cryptocurrency mining software. It supports a variety of algorithms and we, as developers, are trying to do our best to make it as	fast and as convenient to use as possible.

Developer fee is 1% (2% for Octopus and Autolykos2).

## Usage

Full list of command line options:
```
    -a, --algo                     Specify the hash algorithm to use.
                                   autolykos2
                                   etchash
                                   ethash
                                   kawpow
                                   mtp
                                   mtp-tcr
                                   multi
                                   octopus
                                   progpow
                                   progpow-veil
                                   progpow-veriblock
                                   progpowz
                                   tensority
        --coin                     [Ethash, ProgPOW] Set coin name.
                                   Helps avoid DAG rebuilds when switching back from a dev fee session.
                                   Example: "eth" for Ethereum, "zil" for Zilliqa.
        --extra-dag-epoch          Allocate extra DAG at GPU for specified epoch. Can be useful for dual mining
                                   of coins like Zilliqa (ZIL). (eg: --extra-dag-epoch 0)
                                   Can be set for each GPU separately by using comma separated list of values
                                   (set to -1 for the GPUs that should not allocate the extra DAG).
        --nonce-start              [Ethash, ProgPOW] Starting nonce for the solution search.
        --nonce-range-size         [Ethash, ProgPOW] Nonce range size for nonce search. The range will be split between all devices.
    -d, --devices                  Comma separated list of CUDA devices to use.
                                   Device IDs start counting from 0.
        --pci-indexing             Sort devices by PCI bus ID. Device IDs start with 0.
        --ab-indexing              Afterburner indexing (same as --pci-indexing but starts from 1).
    -i, --intensity                GPU intensity 8-25 (default: auto).
                                   Controls the GPU workload size, in other words how many nonces the miner is
                                   processing "in one go": N = 2 ^ intensity
        --low-load                 Low load mode (default: 0). 1 - enabled, 0 - disabled.
                                   Reduces the load on the GPUs if possible. Can be set to a comma separated string to enable
                                   the mode for a subset of the GPU list (eg: --low-load 0,0,1,0)
        --kernel                   [Ethash] Choose CUDA kernel (default: 0). Range from 0 to 5.
                                   Set to 0 to enable auto-tuning: the miner will benchmark each kernel and select the fastest.
                                   Can be set to a comma separated list to apply different values to different cards.
                                   (eg: --kernel 2,1,1,3)
                                   The support for this parameter may later be extended to cover other algorithms.
        --gpu-init-mode            Enables DAG sequential initialization (default: 0).
                                   0 - all GPUs are initialized in parallel
                                   1 - fully sequential initialization, one GPU at a time
                                   2 - two GPUs at a time
                                   etc.
        --dag-build-mode           [Ethash, ProgPOW, Octopus] Controls how DAG is built (default: 0).
                                   0 - auto (miner will choose the most appropriate mode based on the GPU model)
                                   1 - default (suitable for most graphics cards)
                                   2 - recommended for 30xx cards to prevent invalid shares
                                   Can be set to a comma separated list to apply different values to different cards.
                                   (eg: --dag-build-mode 1,1,2,1)
        --keep-gpu-busy            Continue mining even in case of connection loss.

    -o, --url                      URL of the mining pool in the following format: <scheme>://<host>:<port>
                                   Supported schemes: stratum+tcp
                                                      stratum+ssl
                                                      stratum+http
                                                      stratum2+tcp
                                                      stratum2+ssl
                                   stratum2 is normally used by Nicehash, MiningPoolHub and other similar mining pools
                                   Example: stratum+tcp://eu1.ethermine.org:4444
                                            stratum+ssl://zcoin.mintpond.com:3005
                                            stratum2+tcp://daggerhashimoto.hk.nicehash.com:3353
    -u, --user                     Username for mining server.
    -p, --pass                     Password for mining server.
    -w, --worker                   Worker name.

    -r, --retries                  Number of times to retry if a network call fails.
    -R, --retry-pause              Pause in seconds between retries.
    -T, --timeout                  Network timeout, in seconds (default: 300)
        --time-limit               Miner shutdown interval in seconds. (default: 0 - disabled)

        --temperature-color        Set temperature color for GPUs stat. Example: 55,65 - it means that
                                   temperatures above 55 will have yellow color, above 65 - red color. (default: 67,77)
        --temperature-limit        GPU shutdown temperature. (default: 0 - disabled)
        --temperature-start        GPU temperature to enable card after disable. (default: 0 - disabled)

    -b, --api-bind-telnet          IP:port for the miner API via telnet (default: 127.0.0.1:4068). Set to 0 to disable.
                                   For external access set IP to 0.0.0.0, in which case setting "--api-read-only" is
                                   recommended as well.
        --api-bind-http            IP:port for the miner API via HTTP (default: 127.0.0.1:4067). Set to 0 to disable.
                                   For external access set IP to 0.0.0.0, in which case setting "--api-read-only" is
                                   recommended as well.
        --api-read-only            Allow only read operations for API calls.
    -J  --json-response            Telnet API server will make json responses.

    -N, --hashrate-avr             Sliding window length in seconds used to compute average hashrate (default: 60).
        --sharerate-avr            Sliding window length in seconds used to compute sharerate (default: 600).
        --gpu-report-interval      GPU stats report frequency. Minimum is 5 sec. (default: 30 sec)
        --gpu-report-interval-s    GPU stats report frequency in shares. 0 by default (disabled).
    -q, --quiet                    Quiet mode. No GPU stats at all.
        --hide-date                Don't show date in console.
        --send-stales              Don't drop stale shares.
        --validate-shares          Validate shares before sending to a pool. Also enables share diff info.

        --no-color                 Disable color output for console.
        --no-hashrate-report       Disable hashrate report to pool.
        --no-nvml                  Disable NVML GPU stats.
        --no-strict-ssl            Disable certificate validation for SSL connections.
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
        --exit-on-high-power       Forces miner to immediately exit on high power consumption.
                                   (eg: --exit-on-high-power 600 - exit in case of total power consumption exceeds 600W)
        --reconnect-on-fail-shares Forces miner to immediately reconnect to pool on N successively failed shares (default: 10).

        --fork-at                  Forces miner to change algorithm on predefined condition (works only with built-in watchdog enabled)
                                   Epoch condition: <algo_name>=epoch:<epoch_number> (eg: --fork-at etchash=epoch:390).
                                   Block condition: <algo_name>=block:<block_number> (eg: --fork-at x16rv2=block:6526421).
                                   Time condition:  <algo_name>=time:<YYYY-MM-DDTHH:MM:SS>. Time must be set in UTC+0.
                                   (eg: --fork-at x16rv2=time:2019-10-01T16:00:00).
                                   To change main pool port you must write it right after algo: <algo_name>:<port_number>
                                   (eg: --fork-at x16rv2:4081=time:2019-10-01T16:00:00).

        --script-start             Executes user script right after miner start (eg: --script-start path_to_user_script)
        --script-exit              Executes user script right before miner exit.
        --script-epoch-change      Executes user script on epoch change.
        --script-crash             Executes user script in case of miner crash.
        --script-low-hash          Executes user script in case of low hash. Hash threshold is set in MegaHashes/second.
                                   Example: --script-low-hash script_to_activate:50
                                            (activates "script_to_activate" script once total hashrate drops to 50MH/s)

    ------------------ GPU fine tuning (Windows & Linux) ----------------

        --pl                       Sets GPU power limit (Windows - in percent, Linux - in Watts)
                                   Requires running the miner with administrative privileges.
        --lock-cclock              Specifies desired locked GPU core clock speed in MHz. (default: 0 - disabled).
                                   Requires running the miner with administrative privileges.
                                   Example: --lock-cclock 1000 (applies clock 1000Mhz to all cards that support this functionality)
                                            --lock-cclock 1000,1300,0 (applies clock 1000Mhz to GPU #0, 1300MHz to GPU #1, ignore GPU #2)

        --mt                       Memory tweak mode (default: 0 - disabled). Range from 0 to 6. General recommendation
                                   is to start with 1, and then increase only if the GPU is stable.
                                   The effect is similar to that of ETHlargementPill.
                                   Supported on Pascal GPUs with GDDR5 or GDDR5X memory only.
                                   Requires running the miner with administrative privileges.
                                   Can be set to a comma separated list to apply different values to different cards.
                                   Example: --mt 4 (applies tweak mode #4 to all cards that support this functionality)
                                            --mt 3,3,3,0 (applies tweak mode #3 to all cards except the last one)

    ------------------ GPU fine tuning (Windows only) ------------------

                                   All options can be set to a comma separated list to apply different values to
                                   different cards. (default value for all options: 0 - not used)
        --fan                      Sets GPU fan speed in percent or target temperature (auto-fan).
                                   Valid formats:
                                      --fan N    (where N is the fan speed)
                                      --fan t:N  (where N is the target core temperature)
                                      --fan tm:N (where N is the target memory temperature)
                                   Example: --fan 45,t:67,tm:95
                                      GPU #0: set fan speed to 45%
                                      GPU #1: maintain GPU core temperature at 67C
                                      GPU #2: maintain GPU memory temperature at 90C
                                   Note: fan speeds are limited to [5%, 100%] range in auto-fan mode.
        --cclock                   Sets GPU core clock offset in MHz.
                                   Requires running the miner with administrative privileges.
                                   Will be set to 0 on exit and during DAG rebuild.
        --mclock                   Sets GPU memory clock offset in MHz.
                                   Requires running the miner with administrative privileges.
                                   Will be set to 0 on exit and during DAG rebuild.
        --cv                       Sets GPU core voltage in percent. Must be within [0, 100] range.
                                   Use it only in case you know what you are doing!
                                   Requires running the miner with administrative privileges.
        --lock-cv                  Specifies desired GPU core voltage in mV. (default: 0 - disabled).
                                   Requires running the miner with administrative privileges.
        --pstate                   Sets GPU P-state. Valid values: p0.
                                   Requires running the miner with administrative privileges.

    --------------------------------------------------------------------

        --version                  Display version information and exit.
    -h, --help                     Display this help text and exit.


```

### Examples
* **ERGO-nanopool**</br>
```
t-rex -a autolykos2 -o stratum+tcp://ergo-eu1.nanopool.org:11111 -u 9gpNWA3LVic14cMmWHmKGZyiGqrxPaSEvGsdyt7jt2DDAWDQyc9.rig0 -p x
```

* **ERGO-herominers**</br>
```
t-rex -a autolykos2 -o stratum+tcp://ergo.herominers.com:10250 -u 9gpNWA3LVic14cMmWHmKGZyiGqrxPaSEvGsdyt7jt2DDAWDQyc9.rig0 -p x
```

* **ERGO-woolypooly**</br>
```
t-rex -a autolykos2 -o stratum+tcp://pool.eu.woolypooly.com:3100 -u 9gpNWA3LVic14cMmWHmKGZyiGqrxPaSEvGsdyt7jt2DDAWDQyc9.rig0 -p x
```

* **ERGO-2miners**</br>
```
t-rex -a autolykos2 -o stratum+tcp://erg.2miners.com:8888 -u 9gpNWA3LVic14cMmWHmKGZyiGqrxPaSEvGsdyt7jt2DDAWDQyc9.rig0 -p x
```

* **ETH+ZIL-shardpool**</br>
```
t-rex -a ethash -o stratum+tcp://eu1-zil.shardpool.io:3333 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p zil1yn92lnkkfsn0s2hlvfdmz6y2yhpqm98vng38s9@eu1.ethermine.org:4444 -w rig0 --extra-dag-epoch 0
```

* **ETC-2miners**</br>
```
t-rex -a etchash -o stratum+tcp://etc.2miners.com:1010 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0
```

* **ETC-woolypooly**</br>
```
t-rex -a etchash -o stratum+tcp://etc.woolypooly.com:35000 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0
```

* **ETH-2miners**</br>
```
t-rex -a ethash -o stratum+tcp://eth.2miners.com:2020 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0
```

* **ETH-ethproxy**</br>
```
t-rex -a ethash -o stratum+http://127.0.0.1:8080
```

* **ETH-nanopool**</br>
```
t-rex -a ethash -o stratum+tcp://eth-eu1.nanopool.org:9999 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd.rig0/some@email.org -p x
```

* **ETH-ethermine**</br>
```
t-rex -a ethash -o stratum+tcp://eu1.ethermine.org:4444 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0
```

* **ETH-miningpoolhub**</br>
```
t-rex -a ethash -o stratum2+tcp://europe.ethash-hub.miningpoolhub.com:20535 -u somaton.gtx1080 -p x
```

* **ETH-miningrigrentals**</br>
```
t-rex -a ethash -o stratum+tcp://eu-ru01.miningrigrentals.com:3344 -u wasya89.165854 -p x
```

* **ETH-woolypooly**</br>
```
t-rex -a ethash -o stratum+tcp://eth.woolypooly.com:3096 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0
```

* **ETH-flexpool**</br>
```
t-rex -a ethash -o stratum+ssl://eth-us-east.flexpool.io:5555 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0
```

* **CFX-woolypooly**</br>
```
t-rex -a octopus -o stratum+tcp://cfx.woolypooly.com:3094 -u cfx:aajauymfc0cpd4aj91wmfyd150avfg3fmym9j2xrh8.rig0 -p x
```

* **CFX-nanopool**</br>
```
t-rex -a octopus -o stratum+tcp://cfx-eu1.nanopool.org:17777 -u cfx:aajauymfc0cpd4aj91wmfyd150avfg3fmym9j2xrh8.rig0/some@email.org -p x
```

* **RVN-2miners**</br>
```
t-rex -a kawpow -o stratum+tcp://rvn.2miners.com:6060 -u RBX1G6nYDMHVtyaZiQWySMZw1Bb2DEDpT8.rig -p x
```

* **RVN-ravenminer**</br>
```
t-rex -a kawpow -o stratum+tcp://stratum.ravenminer.com:3838 -u RBX1G6nYDMHVtyaZiQWySMZw1Bb2DEDpT8.rig -p x
```

* **RVN-woolypooly**</br>
```
t-rex -a kawpow -o stratum+tcp://rvn.woolypooly.com:55555 -u RBX1G6nYDMHVtyaZiQWySMZw1Bb2DEDpT8.rig -p x
```

* **SERO-woolypooly**</br>
```
t-rex -a progpow --coin sero -o stratum+tcp://sero.woolypooly.com:8008 -u JCbZnEb8XtWV814QWRpDcDxpQpXZXw4ARneAtwXNYdd3reuo4xQDcuZivopA761QnQyfMermHR9Mpi156F5n7ez9tv75Wt7vWbHXtuyZsQVWLbKNHnZgwcXbR2yZmbw89WT -p x -w rig0
```

* **VEIL-woolypooly**</br>
```
t-rex -a progpow-veil -o stratum+tcp://veil.woolypooly.com:3098 -u bv1qzftz0vuqa82zy29avylv8sclskweqsrwysgrkg -p x -w rig0
```

* **FIRO-2miners**</br>
```
t-rex -a mtp -o stratum+tcp://firo.2miners.com:8181 -u aBR3GY8eBKvEwjrVgNgSWZsteJPpFDqm6U.rig0 -p x
```

* **FIRO-mintpond**</br>
```
t-rex -a mtp -o stratum+ssl://firo.mintpond.com:3005 -u aBR3GY8eBKvEwjrVgNgSWZsteJPpFDqm6U.rig0 -p x
```

* **FIRO-woolypooly**</br>
```
t-rex -a mtp -o stratum+tcp://zcoin.woolypooly.com:3080 -u aBR3GY8eBKvEwjrVgNgSWZsteJPpFDqm6U.rig0 -p x
```



## JSON config file

To start T-Rex with config file `config.txt` type in the console: `t-rex -c config.txt`.
Use `config_example` file as a starting point to create your own config.</br>
If a parameter is set in the config file and also via cmd line, the latter takes precedence,
for example: `t-rex -c config.txt -w <worker_name_to_override_the_one_in_config_file>` </br>
You can also use environment variables: simply put `%YOUR_ENV_VAR%` anywhere in your config file and it will get automatically substituted with the value of `YOUR_ENV_VAR` variable at run-time.

## Watchdog

Watchdog is intended to observe miner state and restart T-Rex if it crashes or hangs for any reason.
Also, watchdog can optionally perform auto updates if a newer version is available.
We recommend using the watchdog to avoid any downtime in mining and make sure your GPUs are busy 24/7.
If you do need to disable the watchdog, you can do so using `--no-watchdog` parameter.

## HTTP API

By default HTTP API server binds to `127.0.0.1:4067`. It means that you can only access your miner from localhost.
See `--api-bind-http` parameter on how to change it.
Common example of request structure: `http://<ip>:<port>/<handler_name>`

Handlers:
* **trex** - Shows miner control monitoring page in your web browser.<br/>
  You can see miner stats in real time and also change miner parameters and config on the fly. Aside from that you will also see any available updates.


* **log** - Displays the contents of the log file (if configured).


* **config** - Changes your config on HDD and also change some miner parameters on the fly.<br/>
  You can change multiple parameters with one request. Both GET and POST requests supported.<br/>
  If you use a config file (e.g. `t-rex.exe -c config_file`), then any action with handler `config` will be saved into `config_file`.<br/>
  You can use this handler for automation like changing config file at run-time, shutting down the miner via API and then restarting it with new parameters applied.

  GET usage examples:
    * `http://127.0.0.1:4067/config?protocol-dump=true` <br/>
      Enables protocol dump on the fly and write it into config_file
    * `http://127.0.0.1:4067/config?algo=x16r&devices=0,1&intensity=20,21` <br/>
      Writes the following config settings into the config file: `algo=x16r`, `devices=0,1`, `intensity=20,21`
    * `http://127.0.0.1:4067/config?algo=x16r&devices=0,1&intensity=20,21&config=test.conf` <br/>
      Saves settings into the file `test.conf` which will be created in the folder where the miner resides: `algo=x16r`, `devices=0,1`, `intensity=20,21`
    * `http://127.0.0.1:4067/config?config=test.conf` <br/>
      Saves your current miner settings into `test.conf` file
    * `http://127.0.0.1:4067/config` <br/>
      Shows the current config state
    * `http://127.0.0.1:4067/config?hashrate_avr=10&temperature-limit=70&temperature-start=40` <br/>
      Sets the following parameters on the fly: `hashrate_avr=10`, `temperature-limit=70`, `temperature-start=40`

  For POST requests you must use correct json object with parameters you want to change:<br/>
  URL: `http://127.0.0.1:4067/config`. <br/>
  Payload: `{"hashrate_avr": 10, "temperature-limit": 70, "temperature-start": 40}`.<br/>
  Parameter names and types in json are identical to config json you normally use.


* **summary** - Shows all information about current mining process.

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
    "gpu_user_id": 0,                        

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
    "disabled_at_temperature": 77,
    
    // Shares stat for the device.
    "shares": {
        "accepted_count": 3,
        "invalid_count": 0,
        "rejected_count": 0,
        "solved_count": 0
    }
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

* **do-update** - Updates the miner.<br/>
  Navigate to `http://127.0.0.1:4067/do-update` to start T-Rex update.<br/>
  Navigate to `http://127.0.0.1:4067/do-update?stop=true` to stop the update.


* **control** - Real time configuration of T-Rex miner.<br/>
  As of API 1.3 version the following commands are supported:

  * _shutdown_ - Shuts down your miner. Usage: `http://127.0.0.1:4067/control?command=shutdown`. If you prefer POST set the request body to `{"command": "shutdown"}`.
  
  * _pause_ - Stops your miner. Usage: `http://127.0.0.1:4067/control?pause=true` ; to resume use: `http://127.0.0.1:4067/control?pause=false`. <br/>
  To pause certain GPUs: `http://127.0.0.1:4067/control?pause=true:0,2,3` ; to resume use: `http://127.0.0.1:4067/control?pause=false:0,2,3`.

  * _hashrate-avr_ - Changes sliding window size in real time. Usage: `http://127.0.0.1:4067/control?hashrate-avr=1`.
  It will set sliding window of size 1 sec. If you prefer POST set the request body to `{"hashrate-avr": 1}`.

  * _no-color_ - Disables color output to console. Usage: `http://127.0.0.1:4067/control?no-color=true`. To enable: `http://127.0.0.1:4067/control?no-color=false`.
  If you prefer POST set the request body to `{"no-color": true}`.

  * _protocol-dump_ - Enables user protocol dump into console/log. To enable: `http://127.0.0.1:4067/control?protocol-dump=true`. To disable: `http://127.0.0.1:4067/control?protocol-dump=false`.
  If you prefer POST set the request body to `{"protocol-dump": true}`.

  * _time-limit_ - Sets time limit in seconds for miner (it will shutdown after timeout). Usage: `http://127.0.0.1:4067/control?time-limit=120`. It will shutdown your miner 120 seconds after this request.
  To disable: `http://127.0.0.1:4067/control?time-limit=0`. If you prefer POST set the request body to `{"time-limit": 120}`.


## Antivirus alerts

In order to protect the miner from reverse engineering attacks, the binaries are packed using a third-party software which mangles the original machine code. As a result, some antivirus engines may detect certain signatures within the executable that are similar to those that real viruses protected by the same packer have. In any case, it is advisable not to use cryptocurrency miners on the computers where you store your sensitive data (wallets, passwords etc.).

## Tips

In order to maximise the hashrate our software utilises all available GPU resources, so it is important that you review your overclock settings before you start mining. Our general recommendation is to start from GPU stock settings	(no overclock, default power limit), and then after making sure it is stable, slowly increase your overclock to find the "sweet spot" where the miner performs at its best and still does not crash.

## Support

Discord:
https://discord.gg/gj7jcYf

Bitcointalk:
https://bitcointalk.org/index.php?topic=4432704.0
