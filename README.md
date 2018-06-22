**ERC-777 - Ethereum Token Standard**

    Title: A New Advanced Token Standard
    Author: Ankur Daharwal @an1cu12
    Category: ERC

*ERC(Ethereum Request for Comments)-777*

A New Advanced Token Standard was introduced to establish an evolved Token standard which learned from misconceptions like approve() with a value and the aforementioned send-tokens-to-contract-issue.

**Summary**

Creates a standard interface for a token contract.
The official repository for this standard can be found at https://github.com/jacquesd/ERC777

**Abstract**

This standard defines a new way to interact with a Token Contract. This standard takes advantage of ERC-820 to notify contracts and regular addresses when they receive tokens as well as to be compatible with old contracts.

**Motivation**

This standard tries to improve the widely used ERC-20 token standard. The main advantages of this standard are:
Uses the same philosophy as Ether in that tokens are sent with send(dest, value, data).
A tokensReceived can be defined in any contract and in any regular address in a way that this code is triggered when tokens are received. This avoids the double call needed in the ERC-20 standard.
Both contracts and regular address can control and reject which token they send by registering a tokensToSend function which throws block the send.

Both contracts and regular addresses can control and reject which tokens they receive by registering a tokensReceivedfunction which throws to refuse the reception of tokens.

The token holder can "authorize" and "revoke" operators who can manage their tokens. These operators generally are going to be verified contracts like an exchange, a cheque processor or an automatic charging system.

Every token transaction contains a userData bytes field and a similar operatorData -- in case of an operator transaction -- to be used freely by the user (sender) and the operator respectively to pass data to the recipient.

It can be used in a backwards compatible way with wallets that do not contain the tokensReceived function.

**Specification**

ERC777Token (Token Contract)

    interface ERC777Token {
        function name() public constant returns (string);
        function symbol() public constant returns (string);
        function totalSupply() public constant returns (uint256);
        function granularity() public constant returns (uint256);
        function balanceOf(address owner) public constant returns (uint256);

        function send(address to, uint256 amount) public;
        function send(address to, uint256 amount, bytes userData) public;

        function authorizeOperator(address operator) public;
        function revokeOperator(address operator) public;
        function isOperatorFor(address operator, address tokenHolder) public constant returns (bool);
        function operatorSend(address from, address to, uint256 amount, bytes userData, bytes operatorData) public;

        event Sent(
            address indexed operator,
            address indexed from,
            address indexed to,
            uint256 amount,
            bytes userData,
            bytes operatorData
        );
        event Minted(address indexed operator, address indexed to, uint256 amount, bytes operatorData);
        event Burned(address indexed operator, address indexed from, uint256 amount, bytes userData, bytes operatorData);
        event AuthorizedOperator(address indexed operator, address indexed tokenHolder);
        event RevokedOperator(address indexed operator, address indexed tokenHolder);
    }


The token-contract MUST register the ERC777Token interface via ERC-820.
The basic unit token MUST be 10^18

ERC777TokensSender

Any address (contract or regular account) CAN register a contract (itself or an other) implementing the ERC777TokensSenderinterface via the ERC-820 registry.

    interface ERC777TokensSender {
        function tokensToSend(
            address operator,
            address from,
            address to,
            uint value,
            bytes userData,
            bytes operatorData
        ) public;
    }

**Features of ERC-777**

1) Backwards Compatible
- Makes it compatible with more important functions of ERC20
2) Safer and simpler send to contract
- Introduces a new transfer function that adds a field called bytes where you can add any identifying information to the transfer and it automatically lets the receiving contract know that the transfer occurred.
3) Sets decimal to default 18
4) Adds approved operators

**Reference Links:**

Medium: https://medium.com/coinmonks/erc-777-a-new-advanced-token-standard-c841788ab3cb

Github EIP Issues 777: https://github.com/ethereum/EIPs/issues/777

Github The official repository for this standard: https://github.com/jacquesd/ERC777
