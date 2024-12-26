# What is Primex Keeper?

Keepers play a critical role in the Primex ecosystem. In fact, they ensure the protocol’s security through liquidations and the decentralized execution of conditional orders.

By downloading the official client and making a simple configuration, users can now take on the role of Keepers to help secure the protocol and earn rewards in exchange. Becoming a Keeper also presents a unique opportunity to earn Early Primex Token (ePMX) before the upcoming token generation event (TGE), which you will be able to exchange for Primex Token (PMX) after the TGE.

You can see and claim the earned rewards on [this page](https://app.primex.finance/#/keeper-rewards). Keeper will automatically claim the rewards if its balance is lower than the threshold set in config.

# Prerequisites

Docker engine is up and running, you can use Docker Desktop or Docker CLI. See Docker Desktop installation guides for [Linux](https://docs.docker.com/desktop/setup/install/linux/), [MacOS](https://docs.docker.com/desktop/setup/install/mac-install/), [Windows](https://docs.docker.com/desktop/setup/install/windows-install/).

`.env` file should be in the same directory as Docker image and properly configured as described below.

Keeper’s should have enough native tokens on the balance in the blockchain that it will monitor to pay for gas. Note that Keeper’s address can be found in Keeper logs, during the start-up sequence.

# How to use this image

Create an `.env` file in the project directory. Configure `.env` parameters (see [Keeper Configuration section](#keeper-configuration)). 

Get the latest image and run with docker compose:

```
docker compose pull
docker compose up
```

# Troubleshooting

Linux/MacOS systems may require manual Docker daemon start. See [official docs](https://docs.docker.com/engine/daemon/start/).

Windows may require [WSL Update Package](https://learn.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)
Windows may require [hardware virtualization enabled in the BIOS](https://docs.docker.com/desktop/troubleshoot/topics/#virtualization).

# Keeper Configuration

## Mandatory fields:
`NODE_ADDRESS` - RPC provider to access the blockchain. Only websocket URLs are supported. Currently supported blockchains: Ethereum, Arbitrum, Polygon. One keeper client can monitor only one network, to monitor multiple networks, you should launch multiple keepers. 

Note that not all RPCs are suitable for hosting Keepers. For example, most free public RPCs have strict restrictions in place that are not enough for the thoroughput that Keeper expects. We recommend using private paid RPCs with high limits.


-----

There are two ways to set your private key:

1) Private key in raw format
   
`PRIVATE_KEY` - Raw hex encoded private key. `KEY_PATH` and `PASSWORD_PATH` parameters are ignored if `PRIVATE_KEY` is set. 

2) Private key protected with password

`KEY_PATH` - Path to the .json wallet file. The wallet should be password protected. See the [key generation guide](https://geth.ethereum.org/docs/fundamentals/account-management) to learn more.

`PASSWORD_PATH` - Path to the text file with the wallet password. If `KEY_PATH` is set and `PASSWORD_PATH` is not set, the keeper will require to enter the password manually.

----

`ENSO_API_KEY` - API key for Enso swap router. Generate your API key [here](https://shortcuts.enso.finance/developers).

## Optional fields:

`PAGINATION_COUNT` - Size of a chunk of items when loading positions or orders from the blockchain. Default value is 100.

`KEEPER_SERVICE_ENDPOINT` - Endpoint for Prometheus service. Listens to incoming connections from Prometheus-like services if set. 

`HEALTH_CHECK_DELAY` - Delay in seconds between keeper health checks or keep-alive requests. Default value is 10.

`BALANCE_THRESHOLD` - Minimal balance (in native tokens) before trying to withdraw funds from the keeper's rewards. Default value is 0.

`SNAPSHOT_STORAGE_DEPTH` - Store this amount of blockchain data copies. Allows to process blockchain reorgs up to this depth. Default value is 32.

`ROUTERS` - Priority list of token swap routers. Keeper will switch to the next router in the list if previous router fails to estimate an exchange price. Default value is `enso,paraswap`.

`PYTH_WS_URL` - Pyth oracle prices websocket URL. Default value is `wss://hermes.pyth.network/ws`.

`PYTH_HTTP_URL`  - Pyth oracle HTTP URL for obtaining VAAs. Default value is `https://hermes.pyth.network/v2/updates/price/latest`.

`NO_LOG_TIME` - Disable timestamps in console logs. Default value is false.

`ARTIFACTS_PATH` - Path to the artifacts with contract ABIs. Default value is `./contracts/primex_artifacts/abis`.

`SUPRA_URL` - Supra oracle URL. Default value is `https://rpc-mainnet-dora-2.supra.com/get_proof`.

`SUPRA_POLLING_INTERVAL_SEC` - Supra oracle polling interval in seconds. Default value is 45.
