# T-Rex NVIDIA GPU miner (Ethash / Autolykos2 / Kawpow / Blake3 / Octopus / Firopow / MTP)

## Overview

T-Rex is a versatile cryptocurrency mining software. It supports a variety of algorithms and we, as developers, are trying to do our best to make it as fast and as convenient to use as possible.

Developer fee is 1% (2% for Octopus, Autolykos2, and their dual mining modes).

## FAQ

See https://github.com/trexminer/T-Rex/wiki/FAQ

## Usage

Full list of command line options:
```
    -a, --algo                     Specify the hash algorithm to use.
                                   autolykos2
                                   blake3
                                   etchash
                                   ethash
                                   firopow
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
                                   Example: "eth" for Ethereum, "eth+zil" for Ethereum+Zilliqa mining.
        --extra-dag-epoch          Allocate extra DAG at GPU for specified epoch. Can be useful for dual mining
                                   of coins like Zilliqa (ZIL). (eg: --extra-dag-epoch 0)
                                   Can be set for each GPU separately by using comma separated list of values
                                   (set to -1 for the GPUs that should not allocate the extra DAG).
        --nonce-start              [Ethash, ProgPOW] Starting nonce for the solution search.
        --nonce-range-size         [Ethash, ProgPOW] Nonce range size for nonce search. The range will be split between all devices.
    -d, --devices                  Comma separated list of CUDA devices to use.
                                   Device IDs start counting from 0.
        --ab-indexing              Afterburner indexing (same as default but starts from 1).
        --devices-info             Print list of available CUDA compatible devices in the system.
    -i, --intensity                GPU intensity 8-25 (default: auto).
                                   Controls the GPU workload size, in other words how many nonces the miner is
                                   processing "in one go": N = 2 ^ intensity
        --low-load                 Low load mode (default: 0). 1 - enabled, 0 - disabled.
                                   Reduces the load on the GPUs if possible. Can be set to a comma separated string to enable
                                   the mode for a subset of the GPU list (eg: --low-load 0,0,1,0)
        --dual-algo                Second algorithm to use in dual mining mode.
        --dual-algo-mode           Controls GPU behaviour in dual mining mode (default: a12).
                                   Format: <algo>:<tuning coefficient>
                                   "<algo>" can be one of: "a1" (first algorithm), "a2" (second algorithm), "a12" (both - dual mining)
                                   "<tuning coefficient>" is optional, and can be either
                                         "rXX" ("dual ratio" coefficient set to XX), or
                                         "rXX:lrYY" ("dual ratio" coefficient set to XX, "LHR dual ratio" coefficient set to YY), or
                                         "hXX" (primary algo hashrate percentage XX).
                                   See dual mining guide for more details.
        --profit-per-mh            Estimated profitability of the algorithms in dual mining mode per 1MH of hashrate.
                                   Used for fine-tuning algo1/algo2 proportion, the miner will try to maximise the earnings.
                                   Format: <profit_algo1>:<profit_algo2>.
                                   Example: --profit-per-mh 0.0516:0.0012
        --lhr-tune                 [Ethash, Autolykos2] LHR tuning value that indicates the percentage of the full speed the miner
                                   tries to achieve for LHR cards (default: -1). Range from 10 to 95.
                                   -1 - auto-mode (LHR tune is set to 74 (or 68 in low power mode) for LHR cards and 0 for non-LHR)
                                    0 - disabled (use for non-LHR cards)
                                   30 - recommended starting value for most LHR cards in LHR unlock dual mining mode (see --dual-algo)
                                   68 - recommended starting value for most LHR cards in low power mode (see --lhr-low-power)
                                   74 - recommended starting value for most LHR cards
                                   Can be set for each GPU separately, e.g.
                                   "lhr-tune": "0,0,74.5,0" - this will set LHR tuning value to 74.5 for the third GPU.
        --lhr-autotune-mode        [Ethash, Autolykos2] LHR auto-tune mode (default: full). Valid values:
                                   off  - auto-tune is disabled. LHR tune value is fixed during mining, and will not change
                                          no matter how often LHR lock is detected
                                   down - LHR tune value will decrease if the miner detects LHR lock
                                   full - same as "down" but additionally miner will be trying to increase LHR tune
                                          value if it's stable on the current LHR tune level
        --lhr-autotune-step-size   LHR auto-tune step size (default: 0.5).
                                   Indicates by how much LHR tune value is changed by the LHR auto-tuner.
        --lhr-autotune-interval    LHR auto-tune time interval in minutes (default: 20).
                                   Amount of time the GPU must be mining without hitting LHR locks before the miner
                                   increases LHR tune value.
                                   Also, if the GPU has triggered an LHR lock, LHR tune value will not decrease if
                                   the previous LHR lock was detected more than "--lhr-autotune-interval" minutes ago.
        --lhr-low-power            [Ethash, Autolykos2] Reduces power consumption in LHR mode at a cost of a slightly lower hashrate.
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
        --dataset-mode             [Autolykos2] Dataset mode. (default: 2). Valid values:
                                   0 - auto (defaults to 2)
                                   1 - single (each ERGO block the GPU stops hashing until it generates the dataset)
                                   2 - double (the miner generates the dataset for the next ERGO block before it arrives
                                       with a small penalty to reported hashrate, and when the next block does arrive, it
                                       immediately starts hashing without losing 1-4 seconds on creating the dataset).
                                       Memory requirements are doubled in this mode as the GPU has to hold two memory buffers.
                                       If there is not enough memory on the GPU for two datasets, the miner will fall back
                                       to single buffer mode.
        --keep-gpu-busy            Continue mining even in case of pool connection loss.
                                   Useful when a GPU crashes during start/stop cycle that occurs when internet
                                   connection goes down.
  
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
        --url2                     URL of a second mining pool used for second algo in dual mining mode.
    -u, --user                     Username for mining server.
        --user2                    Username for mining server used for second algo in dual mining mode.
    -p, --pass                     Password for mining server.
        --pass2                    Password for mining server used for second algo in dual mining mode.
    -w, --worker                   Worker name.
        --worker2                  Worker name for mining server used for second algo in dual mining mode.
        --proxy                    IP:port for connection via SOCKS5 proxy server.
                                   Domain names are resolved through the proxy too.
        --dns-https-server         IP:port for DNS server working via DNS-over-HTTPS.

    -r, --retries                  Number of times to retry if a network call fails.
    -R, --retry-pause              Pause in seconds between retries.
    -T, --timeout                  Network timeout, in seconds (default: 300)
        --time-limit               Miner shutdown interval in seconds. (default: 0 - disabled)

        --temperature-color        Set core temperature color for GPUs stat. Example: 55,65 - it means that
                                   temperatures above 55 will have yellow color, above 65 - red color. (default: 67,77)
        --temperature-color-mem    Set memory temperature color for GPUs stat. (default: 80,100)
        --temperature-limit        GPU shutdown core temperature. (default: 0 - disabled)
        --temperature-start        GPU core temperature to enable card after disable. (default: 0 - disabled)

        --api-bind-http            IP:port for the miner API via HTTP (default: 127.0.0.1:4067). Set to 0 to disable.
                                   For external access set IP to 0.0.0.0, in which case setting "--api-read-only" is
                                   recommended as well.
        --api-https                Enable https protocol for API calls.
        --api-key                  Pre-generated authorization key for API calls. Use "--api-generate-key" to generate it.
                                   Mandatory for allowing modify/update API calls.
        --api-read-only            Allow only read operations for API calls.
                                   Enabled by default if "--api-key" isn't set.
        --api-generate-key         Generate API key from user-defined password. The output key is used as a value for "--api-key".
                                   Use this option along with "-c" to write api-key directly into your config file.
        --api-webserver-cert       Full path to API web server certificate file.
        --api-webserver-pkey       Full path to API web server private key file.

    -N, --hashrate-avr             Sliding window length in seconds used to compute average hashrate (default: 60, max: 86400).
        --sharerate-avr            Sliding window length in seconds used to compute sharerate (default: 600).
        --gpu-report-interval      GPU stats report frequency. Minimum is 5 sec. (default: 30 sec)
        --gpu-report-interval-s    GPU stats report frequency in shares. 0 by default (disabled).
    -q, --quiet                    Quiet mode. No GPU stats at all.
        --hide-date                Don't show date in console.
        --send-stales              Don't drop stale shares.
        --validate-shares          Validate shares before sending to a pool. Also enables share diff info.

        --no-color                 Disable color output for console.
        --no-hashrate-report       Disable hashrate report to pool.
        --no-new-block-info        Don't print new block info in console.
        --no-nvml                  Disable NVML GPU stats.
        --no-strict-ssl            Disable certificate validation for SSL connections.
        --no-sni                   Disable setting SNI header for SSL connections.
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
        --benchmark-epoch          Epoch number used during benchmark (default: 0).
                                   Only applicable to DAG based algorithms except ProgPOW family.
        --benchmark-block          Block number used during benchmark (default: 0).
                                   Only applicable for ProgPOW family of algorithms.

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
                                   (eg: --fork-at firopow=time:2021-10-26T06:00:00).
                                   To change main pool port you must write it right after algo: <algo_name>:<port_number>
                                   (eg: --fork-at x16rv2:4081=time:2019-10-01T16:00:00).

    --------------------- Arbitrary script execution ------------------------
    -- (scripts must be located in the same directory as t-rex executable) --
    -------------------------------------------------------------------------

        --script-start             Executes user script right after miner start (eg: --script-start script_filename)
        --script-exit              Executes user script right before miner exit.
        --script-epoch-change      Executes user script on epoch change.
        --script-crash             Executes user script in case of miner crash.
        --script-low-hash          Executes user script in case of low hash. Hash threshold is set in MegaHashes/second.
                                   Example: --script-low-hash script_to_activate:50
                                            (activates "script_to_activate" script once total hashrate drops to 50MH/s)

    -------------------- GPU fine tuning (Windows & Linux) ------------------

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

    -------------------- GPU fine tuning (Windows only) ---------------------

                                   All options can be set to a comma separated list to apply different values to
                                   different cards. (default value for all options: 0 - not used)
        --fan                      Sets GPU fan speed in percent or target temperature (auto-fan).
                                   Valid formats:
                                      --fan N           (where N is the fan speed)
                                      --fan t:N         (where N is the target core temperature)
                                      --fan t:N[F1-F2]  (same as above, but with the fan speed constrained by [F1%, F2%] range)
                                      --fan tm:N        (where N is the target memory temperature)
                                      --fan tm:N[F1-F2] (same as above, but with the fan speed constrained by [F1%, F2%] range)
                                   Example: --fan 45,t:67,tm:95,t:69[45-100],tm:90[50-95]
                                   which translates to
                                      GPU #0: set fan speed to 45%
                                      GPU #1: maintain GPU core temperature at 67C
                                      GPU #2: maintain GPU memory temperature at 90C
                                      GPU #3: maintain GPU core temperature at 69C
                                              with the fan speed limited to [45%, 100%] range
                                      GPU #4: maintain GPU memory temperature at 90C
                                              with the fan speed limited to [50%, 95%] range
                                   Note: fan speeds are limited to [0%, 100%] range in auto-fan mode by default.
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

    -------------------------------------------------------------------------

        --version                  Display version information and exit.
    -h, --help                     Display this help text and exit.


```

### Examples
* **LHR-unlock-dual-ETH+ERGO**</br>
```
t-rex -a ethash --dual-algo autolykos2 -o stratum+tcp://eu1.ethermine.org:4444 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0 --url2 stratum+tcp://pool.woolypooly.com:3100 --user2 9gpNWA3LVic14cMmWHmKGZyiGqrxPaSEvGsdyt7jt2DDAWDQyc9.rig0 --pass2 x
```

* **LHR-unlock-dual-ETH+FIRO**</br>
```
t-rex -a ethash --dual-algo firopow -o stratum+tcp://pool.woolypooly.com:3096 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0 --url2 stratum+tcp://firo.2miners.com:8181 --user2 aBR3GY8eBKvEwjrVgNgSWZsteJPpFDqm6U.rig0 --pass2 x
```

* **LHR-unlock-dual-ETH+RVN**</br>
```
t-rex -a ethash --dual-algo kawpow -o stratum+tcp://eth.2miners.com:2020 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0 --url2 stratum+tcp://rvn.2miners.com:6060 --user2 RBX1G6nYDMHVtyaZiQWySMZw1Bb2DEDpT8.rig0 --pass2 x
```

* **LHR-unlock-dual-ETH+CFX**</br>
```
t-rex -a ethash --dual-algo octopus -o stratum+ssl://eth-us-east.flexpool.io:5555 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0 --url2 stratum+tcp://pool.woolypooly.com:3094 --user2 cfx:aajauymfc0cpd4aj91wmfyd150avfg3fmym9j2xrh8.rig0 --pass2 x
```

* **ETH+ALPH**</br>
```
t-rex -a ethash --dual-algo blake3 -o stratum+tcp://eth.2miners.com:2020 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0 --url2 stratum+tcp://de.alephium.herominers.com:1199 --user2 1qUuxVuXN2Pk4nnYTbL4qihjLWyRkVMQVYQDAajCcuPq --pass2 x
```

* **ERGO-nanopool**</br>
```
t-rex -a autolykos2 -o stratum+tcp://ergo-eu1.nanopool.org:11111 -u 9gpNWA3LVic14cMmWHmKGZyiGqrxPaSEvGsdyt7jt2DDAWDQyc9.rig0/your@email.org -p x
```

* **ERGO-herominers**</br>
```
t-rex -a autolykos2 -o stratum+tcp://de.ergo.herominers.com:1180 -u 9gpNWA3LVic14cMmWHmKGZyiGqrxPaSEvGsdyt7jt2DDAWDQyc9.rig0 -p x
```

* **ERGO-woolypooly**</br>
```
t-rex -a autolykos2 -o stratum+tcp://pool.woolypooly.com:3100 -u 9gpNWA3LVic14cMmWHmKGZyiGqrxPaSEvGsdyt7jt2DDAWDQyc9.rig0 -p x
```

* **ERGO-2miners**</br>
```
t-rex -a autolykos2 -o stratum+tcp://erg.2miners.com:8888 -u 9gpNWA3LVic14cMmWHmKGZyiGqrxPaSEvGsdyt7jt2DDAWDQyc9.rig0 -p x
```

* **ETH+ZIL-ezil**</br>
```
t-rex -a ethash --coin eth+zil -o stratum+tcp://eth.2miners.com:2020 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd --url2 stratum+tcp://eu.ezil.me:5555 --user2 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd.zil1yn92lnkkfsn0s2hlvfdmz6y2yhpqm98vng38s9.WORKER --extra-dag-epoch 0
```

* **ETC-2miners**</br>
```
t-rex -a etchash -o stratum+tcp://etc.2miners.com:1010 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0
```

* **ETC-woolypooly**</br>
```
t-rex -a etchash -o stratum+tcp://pool.woolypooly.com:35000 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0
```

* **ETH-2miners**</br>
```
t-rex -a ethash -o stratum+tcp://eth.2miners.com:2020 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0
```

* **ETH-ISP-hidden-mode**</br>
```
t-rex -a ethash -o stratum+ssl://eth-us-east.flexpool.io:5555 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0 --no-sni --dns-https-server 1.1.1.1
```

* **ETH-ethproxy**</br>
```
t-rex -a ethash -o stratum+http://127.0.0.1:8080
```

* **ETH-nanopool**</br>
```
t-rex -a ethash -o stratum+tcp://eth-eu1.nanopool.org:9999 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd.rig0/your@email.org -p x
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
t-rex -a ethash -o stratum+tcp://pool.woolypooly.com:3096 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0
```

* **ETH-flexpool**</br>
```
t-rex -a ethash -o stratum+ssl://eth-us-east.flexpool.io:5555 -u 0x1f75eccd8fbddf057495b96669ac15f8e296c2cd -p x -w rig0
```

* **CFX-woolypooly**</br>
```
t-rex -a octopus -o stratum+tcp://pool.woolypooly.com:3094 -u cfx:aajauymfc0cpd4aj91wmfyd150avfg3fmym9j2xrh8.rig0 -p x
```

* **CFX-nanopool**</br>
```
t-rex -a octopus -o stratum+tcp://cfx-eu1.nanopool.org:17777 -u cfx:aajauymfc0cpd4aj91wmfyd150avfg3fmym9j2xrh8.rig0/your@email.org -p x
```

* **ALPH-woolypooly**</br>
```
t-rex -a blake3 -o stratum+tcp://pool.woolypooly.com:3106 -u 1qUuxVuXN2Pk4nnYTbL4qihjLWyRkVMQVYQDAajCcuPq -p x -w rig0
```

* **ALPH-herominers**</br>
```
t-rex -a blake3 -o stratum+tcp://de.alephium.herominers.com:1199 -u 1qUuxVuXN2Pk4nnYTbL4qihjLWyRkVMQVYQDAajCcuPq -p x -w rig0
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
t-rex -a kawpow -o stratum+tcp://pool.woolypooly.com:55555 -u RBX1G6nYDMHVtyaZiQWySMZw1Bb2DEDpT8.rig -p x
```

* **SERO-serocash**</br>
```
t-rex -a progpow --coin sero -o stratum+tcp://pool2.sero.cash:8808 -u JCbZnEb8XtWV814QWRpDcDxpQpXZXw4ARneAtwXNYdd3reuo4xQDcuZivopA761QnQyfMermHR9Mpi156F5n7ez9tv75Wt7vWbHXtuyZsQVWLbKNHnZgwcXbR2yZmbw89WT -p x -w rig0
```

* **VBK-luckypool**</br>
```
t-rex -a progpow-veriblock -o stratum+tcp://vbk.luckypool.io:9501 -u V5h6udgGe6eL4M9cYGi776WCP75URm -p x -w rig0
```

* **VEIL-woolypooly**</br>
```
t-rex -a progpow-veil -o stratum+tcp://pool.woolypooly.com:3098 -u bv1qzftz0vuqa82zy29avylv8sclskweqsrwysgrkg -p x -w rig0
```

* **ZANO-luckypool**</br>
```
t-rex -a progpowz -o stratum+tcp://zano.luckypool.io:8877 -u iZ2bZfXdeN626rkyy9YsnfeT1Qq1K6XamE4brWm3tzP5hDUAig4dHmKSqe4yyq5dgbSPjmpLbfidqPyDXAuFY2J9544F95vagSF1Xqq3eCUp -p x -w rig0
```

* **FIRO-2miners**</br>
```
t-rex -a firopow -o stratum+tcp://firo.2miners.com:8181 -u aBR3GY8eBKvEwjrVgNgSWZsteJPpFDqm6U.rig0 -p x
```

* **FIRO-mintpond**</br>
```
t-rex -a firopow -o stratum+ssl://firo.mintpond.com:3005 -u aBR3GY8eBKvEwjrVgNgSWZsteJPpFDqm6U.rig0 -p x
```

* **FIRO-woolypooly**</br>
```
t-rex -a firopow -o stratum+tcp://pool.woolypooly.com:3104 -u aBR3GY8eBKvEwjrVgNgSWZsteJPpFDqm6U.rig0 -p x
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

See https://github.com/trexminer/T-Rex/wiki/API

## Antivirus alerts

In order to protect the miner from reverse engineering attacks, the binaries are packed using a third-party software which mangles the original machine code. As a result, some antivirus engines may detect certain signatures within the executable that are similar to those that real viruses protected by the same packer have. In any case, it is advisable not to use cryptocurrency miners on the computers where you store your sensitive data (wallets, passwords etc.).

## Useful links

Mining calculator: https://woolypooly.com/en/calc/what-to-mine-gpu

## Tips

In order to maximise the hashrate our software utilises all available GPU resources, so it is important that you review your overclock settings before you start mining. Our general recommendation is to start from GPU stock settings (no overclock, default power limit), and then after making sure it is stable, slowly increase your overclock to find the "sweet spot" where the miner performs at its best and still does not crash.

## Support

Discord server:
https://discord.gg/gj7jcYf

Bitcoin Talk Forum:
https://bitcointalk.org/index.php?topic=4432704.0

Official website: https://trex-miner.com
