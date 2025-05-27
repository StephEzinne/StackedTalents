

# StackedTalent - Auction Marketplace Smart Contract

A decentralized talent marketplace powered by smart contracts on the Stacks blockchain. This contract allows verified talents to auction their services, while users can bid using STX tokens. Designed with safety, transparency, and decentralization in mind.

---
## 🚀 Features

* **Talent Registration**: Talents register once, becoming verified participants in the marketplace.
* **Auction Creation**: Verified talents can create auctions specifying service details, pricing, and duration.
* **Bidding System**: Participants can place bids on active auctions. A minimum bid increment ensures fair competition.
* **Auction Completion**: Once the auction ends, the winning bid is transferred to the talent, and relevant stats are updated.
* **Revenue Model**: A platform fee (5%) is collected on each successful bid and stored for the contract owner.
* **Error Handling**: Extensive validations and clear error codes for robustness.
* **Read-only Insights**: Access to auction details, talent data, and global marketplace stats.

---

## 🛠 Contract Structure

### Constants

* `FEE-RATE`: 5% platform fee.
* `MIN/MAX-AUCTION-DURATION`: Ensures auctions last between 1 to 30 days.
* `MIN/MAX-PRICE`: Auction prices between 1 STX and 1,000,000 STX.
* `CONTRACT-OWNER`: Set to `tx-sender` upon deployment.

### Errors

The contract defines specific error codes for:

* Unauthorized access
* Invalid auction states
* Missing data
* Price/duration constraints
* Bid validation issues

Refer to the contract for the full list of errors like `ERR-NOT-AUTHORIZED`, `ERR-ALREADY-REGISTERED`, etc.

### Data Structures

* `talents`: Maps a principal to their profile (verification, ratings, earnings).
* `auctions`: Maps an auction ID to details including title, description, price, end time, bids, and status.

---

## 📦 Public Functions

### `register-talent`

Registers the caller as a verified talent. Fails if already registered.

### `create-auction (title, description, category, price, blocks)`

Creates a new auction. Enforces constraints on string fields, duration, and pricing.

### `place-bid (auction-id, amount)`

Places a bid on an active auction. Validates sender, bid amount, and auction state. Refunds previous highest bidder.

### `complete-auction (auction-id)`

Completes the auction once its duration is over. Transfers STX to the talent and updates stats.

---

## 🔍 Read-Only Functions

* `get-auction (auction-id)`: Fetch auction data.
* `get-talent-info (address)`: Fetch talent profile.
* `get-contract-stats`: Fetch overall marketplace stats (auctions completed, fees collected).
* `is-registered (address)`: Check if an address is a registered talent.
* `can-complete-auction (auction-id)`: Check if an auction can be finalized.

---

## 💼 Admin / Revenue

The contract owner receives a 5% fee from every successful bid. These fees are stored in `total-fees-collected`.

---

## ✅ Validation Highlights

* String fields cannot be empty.
* Bidder cannot be the talent.
* Bids must be 5% higher than the last.
* Auctions must be in an active state and within valid timeframes.
* Prevents double registrations.

---

## 🔐 Security Considerations

* Use of `try!`, `asserts!`, and `unwrap!` for strict error handling.
* Refund logic before state mutation.
* Ensures contract balances are handled properly during bidding and payout.

---

## 🔧 Deployment Notes

Upon deployment:

* `tx-sender` becomes the initial `CONTRACT-OWNER`.
* All constants and fees are enforced as declared.
* Make sure deployment happens from a trusted wallet.

---

## 📈 Future Improvements

* Add a reputation system (upvotes, reviews).
* Support for multiple bid types (e.g., sealed, reserve price).
* NFT receipt for auction winners.
* Admin panel or DAO integration.

---

## 🧪 Testing

Use **Clarinet** to write unit tests for:

* Talent registration
* Auction lifecycle (create, bid, complete)
* Fee calculation and balance checks
* Edge cases (invalid durations, empty strings, etc.)

---
