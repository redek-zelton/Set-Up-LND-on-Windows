# Set-Up-LND-on-Windows
-It's for users in Testnet !

## 1°) Install Bitcoin Core
-[Install Bitcoin Core](https://bitcoin.org/fr/telecharger): Version FR
You need at least 50 GB space on your Disk
- Then Install and Launch BTC Testnet to load all blocks !

## 2°) Install LND
-[Download LND app](https://github.com/lightningnetwork/lnd/releases)
- Choose the latest version on Windows with "amd64"
- Unzip

## 3°) Create Bitcoin.conf
-C:\Users\Username\AppData\Roaming\Bitcoin: In this Path
- create the Bitcoin.conf file
- set this:
```bash
server=1
listen=1
daemon=1
testnet=1
txindex=1
#Config setting for -bind only applied on test network when in [test]section.
#[test]
#bind=127.0.0.1
whitelist=127.0.0.1
rpcuser=foo
rpcpassword=bar
zmqpubrawblock=tcp://127.0.0.1:28332
zmqpubrawtx=tcp://127.0.0.1:28333
```
## 4°) Configure LND
-C:\Users\Username\AppData\Local\Lnd : In this Path
- create the lnd.conf file
- set this:
```bash
[Application Options]
listen=localhost
# Public P2P IP
externalip=localhost
alias=J&PNODE
# choose from: https://www.color-hex.com/
color=#3dc9b3
maxpendingchannels=5
#debuglevel=info
#debuglevel=trace
#debuglevel=debug
#debuglevel=error
#debuglevel=warn
#debuglevel=critical
# Log levels
debuglevel=CNCT=debug,CRTR=debug,HSWC=debug,NTFN=debug,RPCS=debug
profile=9736

# Fee settings - default LND base fee = 1000 (mSat), default LND fee rate = 1 (ppm)
# Forward fee rate in parts per million
bitcoin.basefee=1000
bitcoin.feerate=1

# Minimum channel size (in satoshis, default is 20,000 sats)
minchansize=100000

# Accept AMP (multi-paths) payments, wumbo channels and do not prevent the creation of anchor channel (default value)
accept-amp=true
protocol.wumbo-channels=true
protocol.no-anchors=false

# Save on closing fees
## The target number of blocks in which a cooperative close initiated by a remote peer should be confirmed (default: 10 blocks).
coop-close-target-confs=24

#########################
# Improve startup speed # (from https://www.lightningnode.info/advanced-tools/lnd.conf by Openoms)
#########################
# If true, we'll attempt to garbage collect canceled invoices upon start.
gc-canceled-invoices-on-startup=true
# If true, we'll delete newly canceled invoices on the fly.
gc-canceled-invoices-on-the-fly=true
# Avoid historical graph data sync
ignore-historical-gossip-filters=1
# Enable free list syncing for the default bbolt database. This will decrease
# start up time, but can result in performance degradation for very large
# databases, and also result in higher memory usage. If "free list corruption"
# is detected, then this flag may resolve things.
sync-freelist=true
# Avoid high startup overhead
# If true, will apply a randomized staggering between 0s and 30s when
# reconnecting to persistent peers on startup. The first 10 reconnections will be
# attempted instantly, regardless of the flag's value
stagger-initial-reconnect=true

########################
# Compact the database # (slightly modified from https://www.lightningnode.info/advanced-tools/lnd.conf by Openoms)
########################
# Can be used on demand by commenting in/out the two options below: it can take several minutes
[bolt]
# Whether the databases used within lnd should automatically be compacted on
# every startup (and if the database has the configured minimum age). This is
# disabled by default because it requires additional disk space to be available
# during the compaction that is freed afterwards. In general compaction leads to
# smaller database files.
db.bolt.auto-compact=true
# How long ago the last compaction of a database file must be for it to be
# considered for auto compaction again. Can be set to 0 to compact on every
# startup. (default: 168h; the time unit must be present, i.e. s, m or h, except for 0)
db.bolt.auto-compact-min-age=168h

[Bitcoin]
#bitcoin.mainnet=1
bitcoin.testnet=1
bitcoin.active=1
bitcoin.node=bitcoind

[Bitcoind]
bitcoind.rpcuser=foo
bitcoind.rpcpass=bar
bitcoind.zmqpubrawblock=tcp://127.0.0.1:28332
bitcoind.zmqpubrawtx=tcp://127.0.0.1:28333
```
- create bin folder (mkdir bin)
- C:\Users\Username\AppData\Local\Lnd\bin : In this path
- put these two applications from the lnd zip, in this "lncli.exe and lnd.exe"

## 5°) In Command
-Open Command (1)
- Write 
```bash 
$cd C:\Program Files\Bitcoin
$bitcoin-qt.exe -conf=C:\Users\Username\AppData\Roaming\Bitcoin\bitcoin.conf
```
-Bitcoin Core Testnet Application will open with new configuration
-*************************
-Open Command (2)
-Write
```bash
$cd AppData\Local\Lnd\bin
$lnd.exe --lnddir="C:\Users\Username\AppData\Local\Lnd"
```
-**************************
-Open Command (3)
-write
```bash
# for create a new wallet if not one
$lncli --network=testnet create
# or if already wallet
$lncli --network=testnet unlock
# Then you can write all LND command
$lncli --network=testnet getinfo
```
