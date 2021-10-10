# <b>Identiy</b>

Phoenix provides a naming system that allows participants to add personal information to their on-chain account and subsequently ask for verification of this information by registrars.

## **Setting an Identity**

Users can set an identity by registering through default fields such as legal name, display name, website, Twitter handle, Riot handle, etc. along with some extra, custom fields for which they would like attestations (see Judgements).

Users must reserve funds in a bond to store their information on chain: 0.033333, and 0.008333 per each field beyond the legal name. These funds are locked, not spent - they are returned when the identity is cleared.

The easierst way to create a on-chain identiy is to click the gear icon next to your account on the [Substrate Explorer App](https://substrate-explorer-testnet.swapdex.network/?rpc=wss%3A%2F%2Fswapdex.starkleytech.com%2Fws#/explorer) and select "Set on-chain identity".

![set-identiy](assets/set-identiy-01.png#center)

A pop-up window will appear, offering the default fields.

![set-identiy](assets/set-identity-02.png#center)
