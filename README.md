A **Clarity smart contract** that powers a decentralized monthly raffle for **open-source pull request contributors**.  
Admins can fund the reward pool and trigger random winner selection.  
Participants register for each round and claim their prizes trustlessly on-chain.

---

## 📜 Overview

| Feature | Description |
|----------|--------------|
| 🎟️ **Monthly Raffle** | Each round represents a month of open-source contributions. |
| 👥 **Participant Registry** | Contributors register once per round to enter. |
| 💰 **Reward Pool** | Admin funds a STX pool that the winner can claim. |
| 🎲 **Random Selection** | Winner chosen using on-chain block hash entropy *(to be implemented)*. |
| 🔐 **Fully On-Chain** | Registration, selection, and payouts are handled by the contract. |

---


---

## 📂 Data Model

| Variable | Type | Description |
|-----------|------|-------------|
| `admin` | `principal` | Contract administrator |
| `current-round` | `uint` | ID of the active raffle round |
| `participant-count` | `uint` | Total participants in the current round |
| `reward-pool` | `uint` | STX reward balance for the round |
| `round-winner-drawn` | `map uint → bool` | Tracks if a winner was drawn for a round |
| `winners` | `map uint → principal` | Stores the winner’s address per round |
| `participants` | `map {round, user} → uint` | Registers participants by user per round |
| `participant-index` | `map {round, index} → principal` | Reverse lookup by index |

---

## 🧩 Error Codes

| Code | Constant | Meaning |
|------|-----------|---------|
| `u100` | `ERR_UNAUTHORIZED` | Action requires admin privileges |
| `u101` | `ERR_ROUND_NOT_ACTIVE` | Round is not currently active |
| `u102` | `ERR_ALREADY_REGISTERED` | User already registered this round |
| `u103` | `ERR_NO_PARTICIPANTS` | No participants available |
| `u104` | `ERR_NOT_WINNER` | Caller is not the winner |
| `u105` | `ERR_ALREADY_DRAWN` | Winner already selected for this round |
| `u106` | `ERR_NO_WINNER` | No winner recorded for given round |

---

## Public Functions
```
### `register-entry()`
Registers the sender for the current raffle round.  
Returns their participant index.
```
```clarity
(contract-call? .pr-raffle register-entry)
```
`claim-reward(round uint)`
Allows the round’s winner to claim their reward from the pool.
Transfers the full reward amount in STX.

```clarity
(contract-call? .pr-raffle claim-reward u1)
```
🔍 Read-Only Functions
Function	Description
`get-current-round-id()`	Returns the current round number
`get-participant-count()`	Total number of participants in the current round
`is-registered(user)`	Checks if a specific user is registered
`get-winner(round)`	Returns the winner’s principal for a round
`get-reward-pool()`	Returns the current STX pool balance

🧠 Internal Helpers
```clarity
(get-current-round)
Private helper to retrieve the current round ID for internal calls.
```

🔒 Admin Responsibilities
Fund the reward pool with STX

Start and close raffle rounds

Trigger random winner selection (to be implemented)

⚠️ Only the admin can perform lifecycle operations. Unauthorized calls return ERR_UNAUTHORIZED.

🧪 Future Enhancements
 Add `admin`-only function to fund new rounds

 Implement random winner selection using block hash entropy

 Add event emissions for registration and reward claim

 Integrate with off-chain verification (GitHub PR validation)

 Write Clarity test suite with Clarinet

🧰 Development Setup
Prerequisites
Clarinet

Stacks CLI

Node.js ≥ 18

Run Tests
bash
clarinet test
Simulate Contract
bash
clarinet console
