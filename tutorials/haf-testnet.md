# Setting up a custom HAF testnet

This tutorial will walk you through the steps to set up a custom HAF testnet.

## Prerequisites

- A Linux server with at least 8GB of RAM

## Install dependencies

```bash 
sudo apt update

sudo apt install -y postgresql-server-dev-14 \
    autoconf \
    automake \
    cmake \
    g++ \
    git \
    zlib1g-dev \
    libbz2-dev \
    libsnappy-dev \
    libssl-dev \
    libtool \
    make \
    ninja-build \
    pkg-config \
    doxygen \
    libncurses5-dev \
    libreadline-dev \
    perl \
    python3 \
    python3-jinja2 \
    libboost-chrono-dev \
    libboost-context-dev \
    libboost-coroutine-dev \
    libboost-date-time-dev \
    libboost-filesystem-dev \
    libboost-iostreams-dev \
    libboost-locale-dev \
    libboost-program-options-dev \
    libboost-serialization-dev \
    libboost-system-dev \
    libboost-test-dev \
    libboost-thread-dev \
    libssl-dev \
    libreadline-dev \
    libpqxx-dev \
    postgresql-14
```

## Clone the repository

```bash
git clone https://gitlab.syncad.com/hive/haf.git

cd haf

git submodule update --init --recursive
```
## Build

You can build with Ninja or Make. Below are the instructions for both.

### Build with Ninja

```bash
mkdir build
cd build

cmake \\n  -DCMAKE_BUILD_TYPE=Release \\n  -DBUILD_HIVE_TESTNET=ON \\n  -DCLEAR_VOTES=ON \\n  -DSKIP_BY_TX_ID=ON \\n  -DHIVE_LINT_LEVEL=OFF \\n  .. -GNinja

ninja
```

### Build with Make

```bash
mkdir build
cd build

cmake \\n  -DCMAKE_BUILD_TYPE=Release \\n  -DBUILD_HIVE_TESTNET=ON \\n  -DCLEAR_VOTES=ON \\n  -DSKIP_BY_TX_ID=ON \\n  -DHIVE_LINT_LEVEL=OFF \\n  ..

make -j$(nproc)
```

## Create and setup a new database

```psql
CREATE DATABASE haf;

\c haf

```

Copy the contents of [haf/scripts/haf_builtin_roles.sql](https://gitlab.syncad.com/hive/haf/-/blob/develop/scripts/haf_builtin_roles.sql) into the psql prompt and execute it.

Create the custom roles:

```psql
   CREATE ROLE my_hived LOGIN PASSWORD 'hivedpass' INHERIT IN ROLE hived_group;
   CREATE ROLE my_application LOGIN PASSWORD 'applicationpass' INHERIT IN ROLE hive_applications_group;
```

Add the `hive_fork_manager` extension to the database:

```psql
CREATE EXTENSION hive_fork_manager CASCADE;
```

Afterwards, you can exit the psql prompt by typing `\q`.

## Setup the testnet

- Create the `config.ini` file, for example:

```
p2p-endpoint = 0.0.0.0:2541
webserver-http-endpoint = 127.0.0.2:8751
webserver-ws-endpoint = 127.0.0.1:8090
rc-account-whitelist = initminer,my_account,my_account_two

plugin = account_by_key account_by_key_api wallet_bridge_api block_api chain_api condenser_api database_api market_history_api network_broadcast_api network_node_api rc_api reputation_api rewards_api transaction_status_api sql_serializer

psql-url = dbname=haf user=postgres password=your_password hostaddr=127.0.0.1 port=5432
psql-enable-filter = true

# name of witness controlled by this node (e.g. initwitness )
witness = "initminer"

# WIF PRIVATE KEY to be used by one or more witnesses or miners
private-key = 

# witnesses
witness = "init-0"
witness = "init-1"
witness = "init-2"
witness = "init-3"
witness = "init-4"
witness = "init-5"
witness = "init-6"
witness = "init-7"
witness = "init-8"
witness = "init-9"
witness = "init-10"
witness = "init-11"
witness = "init-12"
witness = "init-13"
witness = "init-14"
witness = "init-15"
witness = "init-16"
witness = "init-17"
witness = "init-18"
witness = "init-19"
witness = "init-20"
witness = "init-21"

private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
private-key = 
```

- Add the initial witnesses to the new `config.ini` file by generating private keys for the initminer and the 22 initial witnesses.

```bash
./programs/hived/hived --generate-witness-key
```

## Start the testnet

```bash
./hived --replay-blockchain -d /path_to_data_dir
```

## Creating new Hive accounts

You can create new accounts on your testnet using any of the methods below:

- Using the CLI wallet on hived
- Using the beem Python library


