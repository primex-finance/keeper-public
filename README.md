# What is Primex Keeper?

Keepers play a critical role in the Primex ecosystem. In fact, they ensure the protocol’s security through liquidations and the decentralized execution of conditional orders.

By downloading the official client and making a simple configuration, users can now take on the role of Keepers to help secure the protocol and earn rewards in exchange. Becoming a Keeper also presents a unique opportunity to earn Early Primex Token (ePMX) before the upcoming token generation event (TGE), which you will be able to exchange for Primex Token (PMX) after the TGE.

You can see and claim the earned rewards on [this page](https://app.primex.finance/#/keeper-rewards). Keeper will automatically claim the rewards if its balance is lower than the threshold set in config.

# How to use this image

Create an `.env` file in the same directory. Configure .env parameters (see Keeper Configuration section). 

Run with docker compose:

`docker compose up`


# Prerequisites

Docker engine is up and running. See [Docker installation guide](https://docs.docker.com/engine/install/).

`.env` file should be in the same directory as Docker image and properly configured as described below.

Keeper’s should have enough native tokens on the balance in the blockchain that it will monitor to pay for gas. Note that Keeper’s address can be found in Keeper logs, during the start-up sequence.

# Troubleshooting

Linux/MacOS systems may require manual Docker daemon start. See [official docs](https://docs.docker.com/engine/daemon/start/).

Windows may require [WSL Update Package](https://learn.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)
Windows may require [hardware virtualization enabled in the BIOS](https://docs.docker.com/desktop/troubleshoot/topics/#virtualization).

# Keeper Configuration

## Mandatory fields:
`NODE_ADDRESS` - RPC provider to access the blockchain. Only websocket URLs are supported.

`KEY_PATH` - Path to the .json wallet file. The wallet should be password protected.

`PASSWORD_PATH` - Path to the text file with the wallet password. If `KEY_PATH` is set and `PASSWORD_PATH` is not set, the keeper will require to enter the password manually.

`PRIVATE_KEY` - Raw hex encoded private key. `KEY_PATH` and `PASSWORD_PATH` parameters are ignored if `PRIVATE_KEY` is set. 

`ENSO_API_KEY` - API key for Enso swap router. You should generate and use your own Enso API key to have stable access to the routing API independently on the number of active keepers.

## Optional fields:

`PAGINATION_COUNT` - Size of a chunk of items when loading positions or orders from the blockchain. Default value is 100.

`KEEPER_SERVICE_ENDPOINT` - Endpoint for Prometheus service. Listens to incoming connections from Prometheus-like services if set. 

`HEALTH_CHECK_DELAY` - Delay in seconds between keeper health checks or keep-alive requests. Default value is 10.

`BALANCE_THRESHOLD` - Minimal balance (in native tokens) before trying to withdraw funds from the keeper's rewards. Default value is 0.

`SNAPSHOT_STORAGE_DEPTH` - Store this amount of blockchain data copies. Allows to process blockchain reorgs up to this depth. Default value is 32.

`ROUTERS` - Priority list of token swap routers. Keeper will switch to the next router in the list if previous router fails to estimate an exchange price. Default value is enso, paraswap.

`PYTH_WS_URL` - Pyth oracle prices websocket URL. Default value is `wss://hermes.pyth.network/ws`.

`PYTH_HTTP_URL`  - Pyth oracle HTTP URL for obtaining VAAs. Default value is `https://hermes.pyth.network/v2/updates/price/latest`.

`NO_LOG_TIME` - Disable timestamps in console logs. Default value is false.

`ARTIFACTS_PATH` - Path to the artifacts with contract ABIs. Default value is `./contracts/primex_artifacts/abis`.
