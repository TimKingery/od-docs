---
description: The what and how of the GEB framework
---

# Introduction to GEB

GEB is a framework for deploying systems that can issue [reflex index](https://medium.com/reflexer-labs/stability-without-pegs-8c6a1cbc7fbd)[es](https://medium.com/reflexer-labs/stability-without-pegs-8c6a1cbc7fbd). Reflex indexes are assets that dampen the volatility of their underlying collateral. They are useful as more "stable" collateral for other DeFi protocols \(compared to ETH or BTC\) or as stable assets with embedded interest rates that supercharge other DeFi protocols.  
  
As an example, if a reflex index was backed by ETH and one day ETH would significantly drop in price, the index would delay this shock and spread it over a longer period of time. The length of the "spread" depends on how the feedback mechanism is set up and on how reflex index users react to the mechanism's incentives \(or even to the mere expectation that the mechanism will change user incentives in a certain way\).  
  
This documentation is meant to explain all the components behind GEB. Before diving in the docs, we recommend reading our original [whitepaper](https://github.com/reflexer-labs/whitepapers/blob/master/English/rai-english.pdf).  
  
GEB is a modified fork of [MCD](https://github.com/makerdao/dss) that has several core differences:

* Variable names you [can actually understand](https://docs.reflexer.finance/contract-translation/naming-transition)
* An autonomous feedback mechanism that changes the incentives of system participants
* The possibility to add insurance for SAFEs
* Fixed and increasing discount auctions \(instead of English auctions\) used to sell off collateral
* Automatic adjustment of several parameters in the system
* A set of contracts that bound control over parameters that are governed in the long run
* The possibility to send stability fees at once to multiple addresses
* The possibility to switch between surplus auctions and other types of strategies meant to remove surplus from the system
* Two prices for each `CollateralType`: one used for generating debt, the other one used exclusively when liquidating SAFEs
* A stability fee treasury that can pay for oracle calls, market making or teams who onboard collateral and take care of the system

### GEB Overview Diagram

Explore the diagram in detail [here](https://viewer.diagrams.net/?target=blank&highlight=0000ff&layers=1&nav=1&title=GEB_overview.drawio#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1nIcaY8N8StVCfyAL_ztbmETJX2bvY3a9%26export%3Ddownload).

![](.gitbook/assets/geb_overview.svg)

{% file src=".gitbook/assets/geb\_overview.svg" caption="Download the Diagram" %}







