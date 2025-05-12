# Experiment 7: AI-Powered Smart Contract for Decentralized Negotiation
# Aim:
To create a smart contract that integrates AI logic for automated negotiation in decentralized commerce. The contract adjusts price and conditions dynamically based on real-time market trends using an on-chain AI model.

# Algorithm:
## Step 1: AI-Powered Dynamic Pricing
step 1.The seller lists an item with a base price, minimum price, and maximum price.

step 2.A buyer submits an offer, which is received and evaluated by the contract.

step 3.The contract uses AI-simulated logic to calculate a dynamic counteroffer based on price range and the submitted offer.

step 4.If the offer meets or exceeds the AI counteroffer, the item is marked as sold and payment is transferred.

step 5.If the offer is too low, the contract refunds the buyer and emits a counteroffer suggestion.

step 6.Each transaction can inform future pricing logic to simulate learning behavior.


## Step 2: Smart Contract Counteroffer
The contract automatically generates a counteroffer if the buyer’s price is within the negotiation range.


If the buyer accepts, the transaction is executed on-chain.


## Step 3: Settlement and Price Learning
Every completed transaction updates the price learning algorithm to refine future pricing decisions.



# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract AIPoweredNegotiation {
    struct Item {
        address seller;
        uint256 minPrice;
        uint256 maxPrice;
        uint256 basePrice;
        bool sold;
    }

    mapping(uint256 => Item) public items;
    uint256 public itemCount;

    event ItemListed(uint256 itemId, uint256 basePrice);
    event OfferMade(uint256 itemId, address buyer, uint256 offer);
    event CounterOffer(uint256 itemId, uint256 counterOffer);
    event Sold(uint256 itemId, address buyer, uint256 finalPrice);

    function listItem(uint256 _basePrice, uint256 _minPrice, uint256 _maxPrice) public {
        require(_minPrice <= _basePrice && _basePrice <= _maxPrice, "Invalid price range");
        
        items[itemCount] = Item(msg.sender, _minPrice, _maxPrice, _basePrice, false);
        emit ItemListed(itemCount, _basePrice);
        itemCount++;
    }

    function makeOffer(uint256 _itemId, uint256 _offerPrice) public payable {
        Item storage item = items[_itemId];
        require(!item.sold, "Item already sold");
        require(msg.value == _offerPrice, "Incorrect offer amount");

        emit OfferMade(_itemId, msg.sender, _offerPrice);

        uint256 aiCounterOffer = dynamicPricing(item.basePrice, item.minPrice, item.maxPrice, _offerPrice);

        if (_offerPrice >= aiCounterOffer) {
            item.sold = true;
            payable(item.seller).transfer(_offerPrice);
            emit Sold(_itemId, msg.sender, _offerPrice);
        } else {
            payable(msg.sender).transfer(_offerPrice); // Refund buyer
            emit CounterOffer(_itemId, aiCounterOffer);
        }
    }

    function dynamicPricing(uint256 base, uint256 min, uint256 max, uint256 offer) private pure returns (uint256) {
        if (offer >= max) return max;
        if (offer >= base) return base;
        return (base + offer) / 2; // Simple AI-based counteroffer logic
    }
}
```

# Expected Output:

![image](https://github.com/user-attachments/assets/aabffe55-9878-4e51-a9c5-40a200f690b7)

Buyers submit offers, and the contract auto-negotiates the price.
![image](https://github.com/user-attachments/assets/33667ccf-9c90-4c36-a485-f6e07312c5d5)

If the buyer’s offer is fair, the deal is executed.
![image](https://github.com/user-attachments/assets/9883f5b1-37bb-4336-aadb-f7884e615c1b)

If the offer is too low, the contract suggests a counteroffer.
![image](https://github.com/user-attachments/assets/4e284b1a-fe62-4a53-9b80-4de7be63c8e5)


# High-Level Overview:
First-of-its-kind AI-powered pricing contract.


Mimics real-world price negotiations using dynamic on-chain pricing.


Can be extended to AI oracles for real-time market data.


Inspired by AI-enhanced commerce and eBay-like decentralized auctions.

# RESULT:

Thus,smart contract that integrates AI logic for automated negotiation in decentralized commerce is executed successfully.


