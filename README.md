# SYNOPSIS

[![Build Status](https://travis-ci.org/susy-js/sophonjs-common.svg?branch=master)](https://travis-ci.org/susy-js/sophonjs-common)
[![Coverage Status](https://img.shields.io/coveralls/susy-js/sophonjs-common.svg?style=flat-square)](https://coveralls.io/r/susy-js/sophonjs-common)
[![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)
[![Gitter](https://img.shields.io/gitter/room/sophon/sophonjs.svg?style=flat-square)]()

# SOPHONJS-COMMON

Resources common to all Sophon implementations

Succeeds the old [sophon/common](https://octonion.institute/susy-js/common/) library.

# INSTALL

`npm install sophonjs-common`

# USAGE

All parameters can be accessed through the `Common` class which can be required through the
main package and instantiated either with just the `chain` (e.g. 'mainnet') or the `chain`
together with a specific `hardfork` provided.

Here are some simple usage examples:

```javascript
const Common = require('sophonjs-common')

// Instantiate with only the chain
let c = new Common('ropsten')
c.param('gasPrices', 'ecAddGas', 'byzantium') // 500

// Chain and hardfork provided
c = new Common('ropsten', 'byzantium')
c.param('pow', 'minerReward') // 3000000000000000000

// Access genesis data for Ropsten network
c.genesis().hash // 0x41941023680923e0fe4d74a34bdac8141f2540e3ae90623718e47d66d1ca4a2d

// Get bootstrap nodes for chain/network
c.bootstrapNodes() // Array with current nodes
```

It is encouraged to also explicitly set the `supportedHardforks` if the initializing library
only supports a certain range of `hardforks`:

```javascript
let c = new Common('ropsten', null, ['byzantium', 'constantinople', 'petersburg'])
```

This will e.g. throw an error when a param is requested for an unsupported hardfork and
like this prevents unpredicted behaviour.

# API

See the API documentation for a full list of functions for accessing specific chain and
depending hardfork parameters. There are also additional helper functions like
`paramByBlock (topic, name, blockNumber)` or `hardforkIsActiveOnBlock (hardfork, blockNumber)`
to ease `blockNumber` based access to parameters.

- [API Docs](./docs/README.md)

# Hardfork Params

There are currently parameter changes by the following past and future hardfork by the
library supported:

- `chainstart`
- `homestead`
- `dao`
- `tangerineWhistle`
- `spuriousDragon`
- `byzantium`
- `constantinople`
- `petersburg` (aka `constantinopleFix`, apply together with `constantinople`)
- `istanbul` (`DRAFT`)

For hardfork-specific parameter access with the `param()` and `paramByBlock()` functions
you can use the following `topics`:

- `gasConfig`
- `gasPrices`
- `vm`
- `pow`
- `casper`
- `sharding`

See one of the hardfork files like `byzantium.json` in the `hardforks` directory
for an overview. For consistency, the chain start (`chainstart`) is considered an own
hardfork.

The hardfork-specific json files only contain the deltas from `chainstart` and
shouldn't be accessed directly until you have a specific reason for it.

Note: The list of `gasPrices` and gas price changes on hardforks is consistent
but not complete, so there are currently gas price values missing (PRs welcome!).

# Chain Params

Supported chains:

- `mainnet`
- `ropsten`
- `rinkeby`
- `kovan`
- `goerli` (final configuration since `v1.1.0`)
- Private/custom chain parameters

The following chain-specific parameters are provided:

- `name`
- `chainId`
- `networkId`
- `genesis` block header values
- `hardforks` block numbers
- `bootstrapNodes` list

To get an overview of the different parameters have a look at one of the chain-specifc
files like `mainnet.json` in the `chains` directory.

If you want to set up a common instance with parameters for a **private/custom chain** you can pass a
dictionary - conforming to the parameter format described above - with your custom values in
the constructor or the `setChain()` method for the `chain` parameter.

# Bootstrap Nodes

There is no separate config file for bootstrap nodes like in the old `sophon-common` library.
Instead use the `common.bootstrapNodes()` function to get nodes for a specific chain/network.

# Genesis States

Network-specific genesis files are located in the `genesisStates` folder.

Due to the large file sizes genesis states are not directly included in the `index.js` file
but have to be accessed directly, e.g.:

```javascript
const mainnetGenesisState = require('sophonjs-common/dist/genesisStates/mainnet')
```

Or by accessing dynamically:

```javascript
const genesisStates = require('sophonjs-common/dist/genesisStates')
const mainnetGenesisState = genesisStates.genesisStateByName('mainnet')
const mainnetGenesisState = genesisStates.genesisStateById(1) // alternative via network Id
```

# SophonJS

See our organizational [documentation](https://sophonjs.readthedocs.io) for an introduction to `SophonJS` as well as information on current standards and best practices.

If you want to join for work or do improvements on the libraries have a look at our [contribution guidelines](https://sophonjs.readthedocs.io/en/latest/contributing.html).

# LICENSE

[MIT](https://opensource.org/licenses/MIT)
