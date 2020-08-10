RMPL

RMPL is an elastic supply ERC20 Token with randomized rebasing.

RMPL is a fork of Ampleforth uFragments repository. Credits to
Ampleforth team for implementation of rebasing on the ethereum network.

RMPL inherits appropriate licenses from upstream project.

RMPL implementation consists of 2 contracts.

RMPL.sol

-   Basic ERC20 Token with a rebase function, callable by the contract
    owner.
-   Owner can be transferred to allow upgrades to the onchain random
    rebasing implementation.
-   Once onchain random rebasing solution has been finalized, contract
    owner will be locked to ensure no party has control and the
    implementation is completely self governed.

Rebaser.sol:

-   Parent contract to RMPL.sol.
-   Rebasing at random times within the rebase window (48hrs)
-   Applying a randomized lag factor based on the current supply, with a
    function to accelerate increase in supply between 10M and 100M
    RMPLs.
-   Rebasing to a price target of \$1.00 USD with a CPI adjustment.
-   Retrieve RMPL/USD price from Uniswap v2 onchain interface.

Rebaser.sol is still in active development. Current tasks being worked
on are described below.

Please refer to https://github.com/rmpldefi for latest code updates.

Onchain Random rebasing implementation:

(as of version 0.8.0)

-   Rebaser.sol contains a publicly callable rebase() function.
-   Can only be called once per block.
-   Will check upto a maximum of the last 250 blocks or the number of
    block since the last call.
-   A random number will be generated between 0 and average number of
    blocks for 48hr period.
-   Average blocks for 48hr period will be recalculated to account for
    changes in the rate of blocks mined on the network.
-   Random number will be compared against number of blocks since last
    rebase.
-   As the number of blocks since last rebase increases, the probability
    of the random generated number triggering a rebase will increase.
-   The probability distribution function used will result in an average
    rebase of 24hrs over a large sample.
-   If there is a rebase trigger, price will be retrieved from Uniswap.
-   If price is within current CPI adjusted threshold (eg. +- 5%) no
    rebase will occur.
-   If price is outside threshold, supply delta will be calculated based
    on the ratio of current price to target price.
-   A random lag factor will be generated and applied to delta:
    -   A range will be calculated based on current total supply.
    -   If current supply is between 10M and 100M tokens a function will
        be applied to accelerate supply increase to 100M
-   New supply delta will now be applied to RMPL token.
-   New rebase stats are updated. (These are accessible via a public
    call to Rebaser contract)

Probability Distribution Function:

[Details from pdf]

Current work in progress BEFORE deploying:

-   Enhanced onchain RNG implementation, current R&D into:
    -   Combination of 1st layer blockhash and 2nd layer oracle call to
        improve randomness, whilst optimizing the process for gas costs.
    -   Reviewing oracle services such as Chainlink's VRF, Randao etc
    -   Ethereum 2.0 network update to include a random number every x
        blocks.
-   R&D of enhanced probability distribution function, to average rebase
    to 24hrs with reasonable deviations over smaller samples.
-   Calculate a weighted average price over time or volume for Uniswap
    pricing.
-   Oraclize US CPI inflation figure for price target.
-   Gas optimizations.
-   Code audit of Rebaser versions before deploying.

As we are in the early stages of development for the Rebaser contract,
the team have decided to perform random rebasing offchain until a
suitable, well reviewed version is ready to be deployed. We are making
the source public to begin this process, with complete transparency.

Onchain randomized rebasing is the priority for RMPL team, but we must
protect all RMPL holders in this short interim period, by only deploying
onchain when we are sure the solution is resistant from any manipulation
or exploitation.
