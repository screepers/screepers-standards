# marketdata protocol v1.0.0

This protocol lets AIs provide data about current market orders to other players.

## Serialization Method

This protocol uses json for serialization.

## Format

The protocol message is an object with the following keys:

* `version`: Version of the marketdata protocol this data conforms to.
* `update`: Tick in which this market data was produced.
* `description` (optional): A human-readable, verbal overview of the market data provided.
* `orders`: An object containing one or more sets of order data. Key names are arbitrary, but should meaningfully represent the contained orders (i.e. "L" for information on buy/sell orders for L). Each order set object contains:
    * `description` (optional): A human-readable, verbal description of this order set.
    * `buyers`: An ordered list of objects representing buy orders in the Screeps market.
        * An order object can contain any subset of the information for an order returned by `Game.market.getAllOrders()`, but should always provide `id`, `amount`, `price`, as well as `roomName` if the order is not for subscription tokens.
    * `sellers`: An ordered list of objects representing sell orders in the Screeps market.
    * `filters`: An object containing information on how the orders in this set were selected. This object contains:
        * `resources` (optional): A list of resource identifiers. Orders in this set must match one of the listed resources.
        * `minAmount` (optional): The minimum `amount` value for an order to be included.
        * `minBuyerPrice` (optional): The price per resource of buy orders must be at or above this value to be included.
        * `maxSellerPrice` (optional): The price per resource of sell orders must be at or below this value to be included.
        * `maxOrders` (optional): No more than this number of orders will be included (limit applies separately to `buyers` and `sellers` list, and results are influenced by `sorting` parameters)
        * `origin` (optional): Room name used for `maxDistance` filter.
        * `maxDistance` (optional): Orders must be at or below this distance from `origin` to be included. Assumes wrapping version of distance calculation is used.
        * `roomWhitelist` (optional): A list of room names that orders must originate from to be included.
        * `roomBlacklist` (optional): A list of room names that orders cannot originate from to be included.
    * `sorting`: An object containing information on how the orders in this set are sorted. This object contains:
        * `sortRule`: Keyword describing sort rule. Options:
            * `bestDeal`: Lowest price for sellers / highest price for buyers comes first.
            * `lowestDistance`: Orders closer to `origin` come first.
        * `origin` (optional): Room name used for distance / transaction cost calculations in sorting.
        * `transactionEnergyValue` (optional): The assumed value in credits per unit of energy. Requires an `origin` value. If provided, then the transaction energy cost (in credits) per resource transferred between `origin` and the order will be added to `price` for sell orders, or subtracted from `price` for buy orders, for sorting purposes.
    * `stats` (optional): An object containing historical data on how often the orders in this order set meaningfully differ from the results of `Game.market.getAllOrders` on the following tick, with the given filters and sorting applied. Since this data is provided in a public segment, it will always be at least 1 tick behind what is available from `getAllOrders`. This `stats` data is intended to help the consumer decide whether to use the orders provided in this segment or run `getAllOrders` themselves. This object contains:
        * `processedCount`: The number of ticks of statistical data included.
        * `buyers`: An object containing the following counts for orders in the `buyers` list:
            * `different` (optional): The number of ticks in the statistical data in which there is any difference in the order IDs presented between the previously generated segment and the the current results. Changing amounts or prices do not matter in this case, but any change in presence or ordering of order IDs does.
            * `betterDeal` (optional): The number of ticks in the statistical data in which the first order in the current results scored better for sorting than the first order in the previously generated segment. This means a better price if sorted by `bestDeal`, or closer to `origin` if sorted by `lowestDistance`.
            * `missing` (optional): The number of ticks in the statistical data in which the first order in the previously generated segment data either does not exist, or no longer meets the filter criteria in the current results (ignoring limits placed by maxOrders).
        * `sellers`: An object containing `different`, `betterDeal`, and/or `missing` values for orders in the `sellers` list.

## Full Example

```json
{
  "version": "v1.0.0",
  "update": 19939494,
  "description": "Just some order data on XGH2O and energy I'm producing for my friends.",
  "orders": {
    "XGH2O": {
      "buyers": [
        {"id": "5967c300505aa41734441f80", "amount": 1000, "price": 1.552, "roomName": "W53N83"},
        {"id": "59567c34d88da946917b50b1", "amount": 2166, "price": 1.551, "roomName": "E37N69"},
        {"id": "592ac1be04cde7f1761a85df", "amount": 100000, "price": 0.14, "roomName": "W21N91"}
      ],
      "sellers": [
        {"id": "58ca3a59e4cd57bb3ebb563a", "amount": 1000, "price": 1.872, "roomName": "E47N35"},
        {"id": "58ca3adbe4cd57bb3ebb78ce", "amount": 1000, "price": 1.872, "roomName": "W54N53"},
        {"id": "594b876a1a556a4d3bb3c92c", "amount": 5000, "price": 2, "roomName": "E13S56"}
      ],
      "filters": {
        "resources": ["XGH2O"],
        "minAmount": 10,
        "maxOrders": 3
      },
      "sorting": {
        "sortRule": "bestDeal"
      },
      "stats": {
        "processedCount": 100,
        "buyers": {
          "betterDeal": 0
        },
        "sellers": {
          "betterDeal": 2
        }
      }
    },
    "localEnergy": {
      "description": "Energy orders close to E35N68 (except for E35N63, I hate that guy)",
      "buyers": [
        {"id": "5967c300505aa41734441f81", "amount": 800, "price": 0.1, "roomName": "E37N68"},
        {"id": "597045b650a4f73ceb1d52c8", "amount": 30000, "price": 0.016, "roomName": "E36N68"},
        {"id": "592ac1be04cde7f1761a85de", "amount": 2000, "price": 0.014, "roomName": "E34N68"},
        {"id": "592ac1be04cde7f1761a85dd", "amount": 4500, "price": 0.015, "roomName": "E41N69"}
      ],
      "sellers": [
        {"id": "58ca3a59e4cd57bb3ebb563b", "amount": 25000, "price": 0.011, "roomName": "E28N72"}
      ],
      "filters": {
        "resources": ["energy"],
        "minAmount": 100,
        "maxSellerPrice": 0.015,
        "maxOrders": 4,
        "origin": "E35N68",
        "maxDistance": 10,
        "roomBlacklist": ["E35N63"]
      },
      "sorting": {
        "sortRule": "bestDeal",
        "origin": "E35N68",
        "transactionEnergyValue": 0.01
      },
      "stats": {
        "processedCount": 100,
        "buyers": {
          "different": 96,
          "betterDeal": 48,
          "missing": 48
        },
        "sellers": {
          "different": 7,
          "betterDeal": 6,
          "missing": 1
        }
      }
    }
  }
}
```
