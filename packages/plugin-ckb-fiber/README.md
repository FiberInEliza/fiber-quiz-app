# CKB Fiber Plugin
This PR introduces a new plugin: the CKB Fiber Plugin, enabling the Eliza agent to interact with the Fiber Network by managing a Fiber node. We implemented a standalone Fiber Network RPC client and developed a series of actions within the Eliza framework. These actions leverage the RPC client to send RPC requests and facilitate communication with the Fiber node.

# Configuration
The plugin requires the following environment variables:
``` env
# CKB Fiber Configuration
FIBER_ENABLE=true

FIBER_RPC_URL=  # RPC url of your Fiber node, anyone can control the Fiber node through this RPC, so make sure that the RPC url cannot be accessed externally. Default is http://127.0.0.1:8227
FIBER_RPC_HEADERS=  # (Optional) The headers of the Fiber RPC (JSON string). It is used to authenticate the Fiber RPC if needed

FIBER_DEFAULT_PEER_ID=QmSqSsbjkQG6aMBNEkG6hMLKQpMjLW3eaBmUdCghpRWqwJ  # The default peer id of the Fiber node, it is used to connect to the Fiber network, you can get it from https://testnet.explorer.nervos.org/fiber/graph/nodes
FIBER_DEFAULT_PEER_ADDRESS=/ip4/43.198.162.23/tcp/8228/p2p/QmSqSsbjkQG6aMBNEkG6hMLKQpMjLW3eaBmUdCghpRWqwJ  # The default peer address of the Fiber node, it is used to connect to the Fiber network, you can get it from https://testnet.explorer.nervos.org/fiber/graph/nodes

FIBER_CKB_FUNDING_AMOUNT=  # (Optional) The default funding amount when open the default channel
FIBER_UDT_FUNDING_AMOUNTS=  # (Optional) The default funding udt amounts (JSON string) when open the default channel
```
You can check the default values in `src/constants.ts`

# Available Actions

1. **GET_INFO**: Retrieves the current node information from the agent-controlled Fiber node.

2. **LIST_CHANNELS**: Lists all the opened channels of the agent-controlled Fiber node.

3. **SEND_PAYMENT**: Given an invoice, payment amount, and asset type (CKB, USDI, or RUSD) in the context, the agent-controlled Fiber node will verify and send the payment.

4. **GET_PAYMENT**: Given a specific payment hash, retrieves the payment details.

5. **NEW_INVOICE**: Given an amount and asset type (CKB, USDI, or RUSD), the agent generates a new invoice, enabling others to send funds to the agent-controlled Fiber node.

# Benefits
- **Fiber Network Integration:** The plugin enables the Eliza agent to interact with the Fiber Network, facilitating cross-chain asset transfers and payments.

- **Payment Management:** The agent can send and receive payments, manage channels, and generate invoices using the Fiber Network.

- **Fiber RPC Client:** The plugin includes a standalone Fiber Network RPC client, enabling the agents / developers to interact with the Fiber node more easily. 

# Cautions
- **Network Access Control:** If the Fiber node’s RPC URL is exposed to the external network without proper access control, there could be potential unauthorized access. Ensure the RPC URL is secured or only accessible within the local network to avoid unintended exposure.

- **Sensitive Actions:** Some actions, such as transfers, involve handling sensitive operations. These actions should be properly secured to prevent users from directly triggering them in inappropriate scenarios (e.g., unauthorized transfers). Ensuring proper access control and validation for these actions is critical.

- **Channel and Network Connectivity:** If the Fiber node is not connected to the network or doesn't have enough channels opened, SEND_PAYMENT action will fail. This could affect the overall functionality and cause disruptions in the intended operations of the Fiber node and Eliza Agent interactions.

# Roadmap

- [x] **Payment Actions:** Implement actions to send and receive payments using the Fiber Network.
- [ ] **Channel Management:** Implement actions to manage channels, including opening, closing, and updating channels.
- [ ] **CKB functionality:** Implement actions to interact with CKB, such as querying balances, sending CKB transactions, and managing UDTs.
- [ ] **Payment History:** Implement actions to retrieve payment history and details for the agent-controlled Fiber node.
- [ ] **Authorization and Access Control:** Because some actions involve sensitive operations, implementing proper authorization and access control mechanisms is essential.

# Testing
``` bash
cd packages/plugin-ckb-fiber
pnpm test
```

# Usage
