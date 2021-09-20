# LHR guide

T-Rex has partial LHR unlock functionality for 30xx GPUs mining `ethash` (~70% of full hashrate).</br>

* In order to enable LHR mode, you need to explicitly tell the miner which GPUs are LHR and which are not.
This is done by specifying `--lhr-tune` parameter which takes values from 0 to 8:</br>
`0` - disabled (use for non-LHR cards)</br>
`1` - lowest hashrate, low chance of LHR lock</br>
`4` - recommended starting value for most LHR cards</br>
`8` - highest hashrate, high chance of LHR lock</br>
It can also be set for each GPU separately, e.g. `--lhr-tune 0,0,4,0` - this will tell the miner that the third GPU
is LHR and that it needs to start with the tuning value of `4`, while the rest of the cards are non-LHR.</br>
HiveOS equivalent would be `"lhr-tune": "0,0,4,0"`

* It's important **not** to set intensity values manually as that may interfere with the unlocking algorithm.

* The miner will try to achieve reasonable hashrate levels using the provided settings, and if that's not possible, 
will start aiming lower. During this process the reported hashrate will fluctuate and mostly stay on the lower side
because every LHR lock will cause the miner to pause for 20 seconds to unlock the GPU, so you may need to wait till
it finds stable settings.

* Don't change your overclock settings too much while the miner is running, particularly be careful with the changes
that may result in a rapid hashrate drop, like reducing memory overclock or decreasing power limit significantly
(all during mining) - this can trick the miner into thinking the LHR has kicked in.

* 3060 rev.1 cards are not given any special treatment, and you're better off running them with 470.05 driver as before.
