https://www.youtube.com/watch?v=Wfy9KO3URE8

Description
Streamswap aims to provide a fully-fledged, streaming-exchange protocol built on Superfluid that works similarly to Uniswap or Balancer.

Currently in finance, all transactions are discrete. We believe this is an archaic restriction brought forth from the old systems in use today and that DeFi should break free of this convention. This would bring benefits in many applications such as subscription payments or individuals' salaries. Superfluid has forged the path in the this new frontier, however there are currently no accepted means by which to fully utilize the power of streamed funds.

We believe streaming tokens are a powerful application of DeFi and we seek to bring a way to exchange tokens continuously with a familiar balancer/uniswap-style AMM protocol. This will allow the creation of fully fledged exchanges where users can trade one incoming stream of tokens for another, or invest into a portfolio on a truly continuous basis, and much more. This will help facilitate the exchange of funds and unlock the full potential of streams.

Streamswap showcase

How it's made
We wanted this tool to require **no** backend infrastructure or maintenance whatsoever. Therefore, we developed a system which relies on arbitrage trading to adjust the rate, and the outgoing streams are adjusted to match the proportional output you would receive from the underlying pool. The system consists of 3 components: Solidity contracts, React web UI, and a Subgraph.

Superfluid, particularly the Super App hooks and the Constant Flow Agreement, are used for the streaming of funds. We created a Solidity contract as a Superfluid app. When the app receives a stream (within the same transaction) the contract returns the equivalent value of the requested token. The rate is adjusted to true market rates by arbitrage/instant trades with the pool.

In its current implementation, Streamswap can only handle a maximum of ~25 streams. This limitation is because the rates of all affected streams must be updated after each instant swap. Consequently, we are forced to have `O(n)` gas cost efficiency—placing a limit on the number of streams before hitting the block gas limit. Before the project can be fully realized, `ConstantFlowAgreementV1` in the core Superfluid protocol will need to be augmented to support managing the flow rate of *grouped outgoing streams* instead of individual alone. If this problem were resolved, the protocol would be able to scale to an unlimited number of streams.

The pool is a modified Balancer pool which retains the same ABI, but includes Superfluid App hooks and a few internal function modifications and a few new events. By utilizing the Balancer ABI, arbitrage systems are able to easily integrate with the pool and trade as if it were any other Balancer pool.

The main reasons for using a forked version of Balancer are gas costs and code-complexity during the hackathon. In the future, it may be possible to use the official Balancer protocol in conjunction with a `ConfigurableRightsPool`.

For the dapp web-UI, we use a Next.js React application hosted on Surge. Transactions are executed through Ethers and the Superfluid SDK. Data is populated entirely using a bespoke subgraph. Our subgraph, like the ones for many other trading protocols, measures individual and aggregate streams, individual and aggregate instant swaps, and bucketed statistics over time. There is no backend or management daemon outside of the subgraph and Infura endpoint.



# Streamswap

To learn more about the project, please visit the submodule folders:

* `packages/core`: Includes smart contracts for super app, tests, deployment automation
* `packages/ui`: Contains the demo frontend UI in react
* `packages/subgraph`: Subgraph code for UI and analytics

## Building
```bash
npm install
npx lerna bootstrap
npx lerna run build
```
