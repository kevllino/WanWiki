# How to deploy smart contracts on Wanchain 

## Requirements

1. Get the [latest](https://github.com/wanchain/go-wanchain) version of the Wanchain client
2. Open the [Remix editor](https://remix.ethereum.org), which is an amazing online smart contract development IDE
3. [Create](Create-a-WAN-account.md) a Wanchain account on the test network and get some Wancoins using the [faucet](Getting-WANs-from-Wanchain's-Testnet-Faucet.md)
4. Unlock your account: 
    - Run a Wanchain client: `cd ${GO_WANCHAIN_ROOT_FOLDER} && build/bin/gwan --testnet console`
    - Run in the Javascript console the following, if successful, `true` should be returned: `personal.unlockAccount(wanAddress, "wanPassword")` 

## Steps

### 1 - Go to remix, copy and paste the following smart contracts code, make static syntax analysis, and compile them:

**Disclaimer: The following smart contracts code is only an example and is NOT to be used in Production systems**

**Create a SafeMath.sol file and paste the following code in it:**
```solidity
pragma solidity ^0.4.23;

/**
 * Math operations with safety checks
 */
library SafeMath {
    function mul(uint a, uint b) internal pure returns (uint) {
        uint c = a * b;
        assert(a == 0 || c / a == b);
        return c;
    }

    function div(uint a, uint b) internal pure returns (uint) {
        assert(b > 0);
        uint c = a / b;
        assert(a == b * c + a % b);
        return c;
    }

    function sub(uint a, uint b) internal pure returns (uint) {
        assert(b <= a);
        return a - b;
    }

    function add(uint a, uint b) internal pure returns (uint) {
        uint c = a + b;
        assert(c >= a);
        return c;
    }

    function max64(uint64 a, uint64 b) internal pure returns (uint64) {
        return a >= b ? a : b;
    }

    function min64(uint64 a, uint64 b) internal pure returns (uint64) {
        return a < b ? a : b;
    }

    function max256(uint256 a, uint256 b) internal pure returns (uint256) {
        return a >= b ? a : b;
    }

    function min256(uint256 a, uint256 b) internal pure returns (uint256) {
        return a < b ? a : b;
    }
}
```
**Create a ERC20Protocol.sol file and paste the following code in it:**
```solidity
pragma solidity ^0.4.23;

contract ERC20Protocol {
    /* This is a slight change to the ERC20 base standard.
    function totalSupply() constant returns (uint supply);
    is replaced with:
    uint public totalSupply;
    This automatically creates a getter function for the totalSupply.
    This is moved to the base contract since public getter functions are not
    currently recognised as an implementation of the matching abstract
    function by the compiler.
    */
    /// total amount of tokens
    uint public totalSupply;

    /// @param _owner The address from which the balance will be retrieved
    /// @return The balance
    function balanceOf(address _owner) public constant returns (uint balance);

    /// @notice send `_value` token to `_to` from `msg.sender`
    /// @param _to The address of the recipient
    /// @param _value The amount of token to be transferred
    /// @return Whether the transfer was successful or not
    function transfer(address _to, uint _value) public returns (bool success);

    /// @notice send `_value` token to `_to` from `_from` on the condition it is approved by `_from`
    /// @param _from The address of the sender
    /// @param _to The address of the recipient
    /// @param _value The amount of token to be transferred
    /// @return Whether the transfer was successful or not
    function transferFrom(address _from, address _to, uint _value) public returns (bool success);
    
    /// @notice send `_value` token to `_to` from `msg.sender`
    /// @param _to The address of the recipient
    /// @param _toKey the ota pubkey 
    /// @param _value The amount of token to be transferred
    /// @return Whether the transfer was successful or not  
    function otatransfer(address _to, bytes _toKey, uint256 _value) public returns (string);    
    
    /// @param _owner The address from which the ota balance will be retrieved
    /// @return The balance
    function otabalanceOf(address _owner) public constant returns (uint256 balance);

    /// @notice `msg.sender` approves `_spender` to spend `_value` tokens
    /// @param _spender The address of the account able to transfer the tokens
    /// @param _value The amount of tokens to be approved for transfer
    /// @return Whether the approval was successful or not
    function approve(address _spender, uint _value) public returns (bool success);

    /// @param _owner The address of the account owning tokens
    /// @param _spender The address of the account able to transfer the tokens
    /// @return Amount of remaining tokens allowed to spent
    function allowance(address _owner, address _spender) public constant returns (uint remaining);

    event Transfer(address indexed _from, address indexed _to, uint _value);
    event Approval(address indexed _owner, address indexed _spender, uint _value);
}
```

**Create a EIP20.sol file and paste the following code in it:**
```solidity
pragma solidity ^0.4.23;

import './ERC20Protocol.sol';

contract EIP20 is ERC20Protocol {
    
    uint256 constant MAX_UINT256 = 2**256 - 1;
    mapping (address => uint256) public balances;
    mapping (address => mapping (address => uint256)) public allowed;
    
    string public name; 
    uint8 public decimals; 
    string public symbol;
    
    constructor (
        uint256 _initialAmount, 
        string _tokenName, 
        uint8 _decimalUnits, 
        string _tokenSymbol
    ) public {
        balances[msg.sender] = _initialAmount; 
        totalSupply = _initialAmount;
        name = _tokenName;
        decimals = _decimalUnits;
        symbol = _tokenSymbol;
}
}
```

**Create a StandardToken.sol file and paste the following code in it:**
```solidity
pragma solidity ^0.4.23;

import './ERC20Protocol.sol';
import './SafeMath.sol';

//the contract implements ERC20Protocol interface with privacy transaction
contract StandardToken is ERC20Protocol {

    using SafeMath for uint;
    string public constant name = "WanToken-Beta";
    string public constant symbol = "WanToken";
    uint public constant decimals = 18;
    mapping(address => uint) balances;
    mapping(address => mapping(address => uint)) allowed;

    function transfer(address _to, uint _value) public returns (bool success) {

        if (balances[msg.sender] >= _value) {
            balances[msg.sender] -= _value;
            balances[_to] += _value;
            emit Transfer(msg.sender, _to, _value);
            return true;
        } else {return false;}
    }

    function transferFrom(address _from, address _to, uint _value) public returns (bool success) {

        if (balances[_from] >= _value && allowed[_from][msg.sender] >= _value) {
            balances[_to] += _value;
            balances[_from] -= _value;
            allowed[_from][msg.sender] -= _value;
            emit Transfer(_from, _to, _value);
            return true;
        } else {return false;}
    }

    function balanceOf(address _owner) public constant returns (uint balance) {
        return balances[_owner];
    }

    function approve(address _spender, uint _value) public returns (bool success) {

        assert((_value == 0) || (allowed[msg.sender][_spender] == 0));

        allowed[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function allowance(address _owner, address _spender) public constant returns (uint remaining) {
        return allowed[_owner][_spender];
    }

    // privacy balance, bytes for public key 
    mapping(address => uint256) public privacyBalance;
    mapping(address => bytes) public otaKey;

    //this only for initialize, only for test to mint token to one wan address
    function initPrivacyAsset(address initialBase, bytes baseKeyBytes, uint256 value) public {
        privacyBalance[initialBase] = value;
        otaKey[initialBase] = baseKeyBytes;
    }

    // return string just for debug
    function otatransfer(address _to, bytes _toKey, uint256 _value) public returns (string) {
        if (privacyBalance[msg.sender] < _value) return "sender token too low";

        privacyBalance[msg.sender] -= _value;
        privacyBalance[_to] += _value;
        otaKey[_to] = _toKey;
        return "success";
    }

    //check privacy balance
    function otabalanceOf(address _owner) public view returns (uint256 balance) {
        return privacyBalance[_owner];
    }
}
```
**Note:**
- Privacy transaction function is "otatransfer" in the ERC20 Protocol, the contract with privacy transaction need to implement ERC20 Protocol
- Privacy balance is stored in the map privacyBalance, function otabalanceOf can get this balance


### 2 - Click Details on the right panel of remix, copy all of the following code from the WEB3DEPLOY section:

```javascript
var erc20simple_contract = web3.eth.contract([{"constant":true,"inputs":[],"name":"name","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_spender","type":"address"},{"name":"_value","type":"uint256"}],"name":"approve","outputs":[{"name":"success","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"totalSupply","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_to","type":"address"},{"name":"_toKey","type":"bytes"},{"name":"_value","type":"uint256"}],"name":"otatransfer","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"name":"_from","type":"address"},{"name":"_to","type":"address"},{"name":"_value","type":"uint256"}],"name":"transferFrom","outputs":[{"name":"success","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"decimals","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"}],"name":"privacyBalance","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"_owner","type":"address"}],"name":"balanceOf","outputs":[{"name":"balance","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"symbol","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"initialBase","type":"address"},{"name":"baseKeyBytes","type":"bytes"},{"name":"value","type":"uint256"}],"name":"initPrivacyAsset","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"name":"_to","type":"address"},{"name":"_value","type":"uint256"}],"name":"transfer","outputs":[{"name":"success","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"_owner","type":"address"}],"name":"otabalanceOf","outputs":[{"name":"balance","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"_owner","type":"address"},{"name":"_spender","type":"address"}],"name":"allowance","outputs":[{"name":"remaining","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"}],"name":"otaKey","outputs":[{"name":"","type":"bytes"}],"payable":false,"stateMutability":"view","type":"function"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_from","type":"address"},{"indexed":true,"name":"_to","type":"address"},{"indexed":false,"name":"_value","type":"uint256"}],"name":"Transfer","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_owner","type":"address"},{"indexed":true,"name":"_spender","type":"address"},{"indexed":false,"name":"_value","type":"uint256"}],"name":"Approval","type":"event"}]);


var erc20simple = erc20simple_contract.new(
   {
     from: web3.eth.accounts[0], 
     data: '0x6060604052341561000f57600080fd5b6111e38061001e6000396000f3006060604052600436106100d0576000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806306fdde03146100d5578063095ea7b31461016357806318160ddd146101bd578063209194e6146101e657806323b872dd146102e4578063313ce5671461035d57806341267ca21461038657806370a08231146103d357806395d89b4114610420578063a3796c15146104ae578063a9059cbb14610533578063ce6ebd3d1461058d578063dd62ed3e146105da578063f8a5b33514610646575b600080fd5b34156100e057600080fd5b6100e86106f8565b6040518080602001828103825283818151815260200191508051906020019080838360005b8381101561012857808201518184015260208101905061010d565b50505050905090810190601f1680156101555780820380516001836020036101000a031916815260200191505b509250505060405180910390f35b341561016e57600080fd5b6101a3600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091908035906020019091905050610731565b604051808215151515815260200191505060405180910390f35b34156101c857600080fd5b6101d06108b5565b6040518082815260200191505060405180910390f35b34156101f157600080fd5b610269600480803573ffffffffffffffffffffffffffffffffffffffff1690602001909190803590602001908201803590602001908080601f016020809104026020016040519081016040528093929190818152602001838380828437820191505050505050919080359060200190919050506108bb565b6040518080602001828103825283818151815260200191508051906020019080838360005b838110156102a957808201518184015260208101905061028e565b50505050905090810190601f1680156102d65780820380516001836020036101000a031916815260200191505b509250505060405180910390f35b34156102ef57600080fd5b610343600480803573ffffffffffffffffffffffffffffffffffffffff1690602001909190803573ffffffffffffffffffffffffffffffffffffffff16906020019091908035906020019091905050610a75565b604051808215151515815260200191505060405180910390f35b341561036857600080fd5b610370610ce5565b6040518082815260200191505060405180910390f35b341561039157600080fd5b6103bd600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610cea565b6040518082815260200191505060405180910390f35b34156103de57600080fd5b61040a600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610d02565b6040518082815260200191505060405180910390f35b341561042b57600080fd5b610433610d4b565b6040518080602001828103825283818151815260200191508051906020019080838360005b83811015610473578082015181840152602081019050610458565b50505050905090810190601f1680156104a05780820380516001836020036101000a031916815260200191505b509250505060405180910390f35b34156104b957600080fd5b610531600480803573ffffffffffffffffffffffffffffffffffffffff1690602001909190803590602001908201803590602001908080601f01602080910402602001604051908101604052809392919081815260200183838082843782019150505050505091908035906020019091905050610d84565b005b341561053e57600080fd5b610573600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091908035906020019091905050610e21565b604051808215151515815260200191505060405180910390f35b341561059857600080fd5b6105c4600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610f7e565b6040518082815260200191505060405180910390f35b34156105e557600080fd5b610630600480803573ffffffffffffffffffffffffffffffffffffffff1690602001909190803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610fc7565b6040518082815260200191505060405180910390f35b341561065157600080fd5b61067d600480803573ffffffffffffffffffffffffffffffffffffffff1690602001909190505061104e565b6040518080602001828103825283818151815260200191508051906020019080838360005b838110156106bd5780820151818401526020810190506106a2565b50505050905090810190601f1680156106ea5780820380516001836020036101000a031916815260200191505b509250505060405180910390f35b6040805190810160405280600d81526020017f57616e546f6b656e2d426574610000000000000000000000000000000000000081525081565b6000808214806107bd57506000600260003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054145b15156107c557fe5b81600260003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020819055508273ffffffffffffffffffffffffffffffffffffffff163373ffffffffffffffffffffffffffffffffffffffff167f8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925846040518082815260200191505060405180910390a36001905092915050565b60005481565b6108c36110fe565b81600360003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020541015610947576040805190810160405280601481526020017f73656e64657220746f6b656e20746f6f206c6f770000000000000000000000008152509050610a6e565b81600360003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000206000828254039250508190555081600360008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000206000828254019250508190555082600460008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000209080519060200190610a34929190611112565b506040805190810160405280600781526020017f737563636573730000000000000000000000000000000000000000000000000081525090505b9392505050565b600081600160008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000205410158015610b42575081600260008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000205410155b15610cd95781600160008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000206000828254019250508190555081600160008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000206000828254039250508190555081600260008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020600082825403925050819055508273ffffffffffffffffffffffffffffffffffffffff168473ffffffffffffffffffffffffffffffffffffffff167fddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef846040518082815260200191505060405180910390a360019050610cde565b600090505b9392505050565b601281565b60036020528060005260406000206000915090505481565b6000600160008373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020549050919050565b6040805190810160405280600881526020017f57616e546f6b656e00000000000000000000000000000000000000000000000081525081565b80600360008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000208190555081600460008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000209080519060200190610e1b929190611112565b50505050565b600081600160003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054101515610f735781600160003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000206000828254039250508190555081600160008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020600082825401925050819055508273ffffffffffffffffffffffffffffffffffffffff163373ffffffffffffffffffffffffffffffffffffffff167fddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef846040518082815260200191505060405180910390a360019050610f78565b600090505b92915050565b6000600360008373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020549050919050565b6000600260008473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060008373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054905092915050565b60046020528060005260406000206000915090508054600181600116156101000203166002900480601f0160208091040260200160405190810160405280929190818152602001828054600181600116156101000203166002900480156110f65780601f106110cb576101008083540402835291602001916110f6565b820191906000526020600020905b8154815290600101906020018083116110d957829003601f168201915b505050505081565b602060405190810160405280600081525090565b828054600181600116156101000203166002900490600052602060002090601f016020900481019282601f1061115357805160ff1916838001178555611181565b82800160010185558215611181579182015b82811115611180578251825591602001919060010190611165565b5b50905061118e9190611192565b5090565b6111b491905b808211156111b0576000816000905550600101611198565b5090565b905600a165627a7a72305820c7a344a3e72215ba3952e15ab354904e1edd02798099defd384453d37142be270029', 
     gas: '4700000'
   }, function (e, contract){
    console.log(e, contract);
    if (typeof contract.address !== 'undefined') {
         console.log('Contract mined! address: ' + contract.address + ' transactionHash: ' + contract.transactionHash);
    }
 })
```

### 3 - Launch a Wanchain client console, make sure it is connected with either various wanchain public networks, or your private blockchain network
### 4 - Paste and run the copied code in the console
 - Note: If you get an error, it's probably that you don't have a test account in your Wanchain client or that your balance of Wancoins 
 is insufficient to create a transaction. If this is the case, we advise you to go back to [Requirements](#requirements)

### 5 - If successful, The transaction id and contract address (hash values starting with '0x') will be printed out onto the console after few seconds
### 6 - Now, you can play with your Dapp, for example by transacting privately in the example below


# How to invoke privacy transfer

After having deployed the above token contract on Wanchain, you can invoke in the Wanchain console, token privacy transaction according to the following process: 

Suppose there are at least 2 accounts in your Wanchain node, you can create additional accounts from the console by running: `personal.newAccount('password')`

### 1 - Define asset functions and variables

```javascript
var initialPrivateBalance = 10000;
var privateTransactionValue = 888;

var wanBalance = function(addr){
    return web3.fromWin(web3.eth.getBalance(addr));
}

var wanUnlock = function(wanAddress, accountPassword){
    return personal.unlockAccount(wanAddress, accountPassword, 99999);
}

var sendWanFromUnlock = function (From, To , V){
    eth.sendTransaction({from:From, to: To, value: web3.toWin(V)});
}

var wait = function (conditionFunc) {
    var loopLimit = 130;
    var loopTimes = 0;
    while (!conditionFunc()) {
        admin.sleep(2);
        loopTimes++;
        if(loopTimes>=loopLimit){
            throw Error("Wait timeout! conditionFunc:" + conditionFunc)
        }
    }
}

wanUnlock(eth.accounts[0], "password1")
wanUnlock(eth.accounts[1], "password2")
stampBalance = 0.09;
```

### 2 - Buy a stamp for token privacy transaction
```javascript
abiDefStamp = [{"constant":false,"type":"function","stateMutability":"nonpayable","inputs":[{"name":"OtaAddr","type":"string"},{"name":"Value","type":"uint256"}],"name":"buyStamp","outputs":[{"name":"OtaAddr","type":"string"},{"name":"Value","type":"uint256"}]},{"constant":false,"type":"function","inputs":[{"name":"RingSignedData","type":"string"},{"name":"Value","type":"uint256"}],"name":"refundCoin","outputs":[{"name":"RingSignedData","type":"string"},{"name":"Value","type":"uint256"}]},{"constant":false,"type":"function","stateMutability":"nonpayable","inputs":[],"name":"getCoins","outputs":[{"name":"Value","type":"uint256"}]}];

contractDef = eth.contract(abiDefStamp);
stampContractAddr = "0x00000000000000000000000000000000000000c8";
stampContract = contractDef.at(stampContractAddr);

var wanAddr = wan.getWanAddress(eth.accounts[0]);
var otaAddrStamp = wan.generateOneTimeAddress(wanAddr);
txBuyData = stampContract.buyStamp.getData(otaAddrStamp, web3.toWin(stampBalance));

sendTx = eth.sendTransaction({from:eth.accounts[0], to:stampContractAddr, value:web3.toWin(stampBalance), data:txBuyData, gas: 1000000});
wait(function(){return eth.getTransaction(sendTx).blockNumber != null;});

keyPairs = wan.computeOTAPPKeys(eth.accounts[0], otaAddrStamp).split('+');
privateKeyStamp = keyPairs[0];
```
### 3 - Get stamp mix set for ring sign

```javascript
var mixStampAddresses = wan.getOTAMixSet(otaAddrStamp,2);
var mixSetWith0x = []
for (i = 0; i < mixStampAddresses.length; i++){
    mixSetWith0x.push(mixStampAddresses[i])
}
```

### 4 - Define token contract ABI
```javascript
var erc20simple_contract = web3.eth.contract([{"constant":true,"inputs":[],"name":"name","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_spender","type":"address"},{"name":"_value","type":"uint256"}],"name":"approve","outputs":[{"name":"success","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"totalSupply","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_to","type":"address"},{"name":"_toKey","type":"bytes"},{"name":"_value","type":"uint256"}],"name":"otatransfer","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"name":"_from","type":"address"},{"name":"_to","type":"address"},{"name":"_value","type":"uint256"}],"name":"transferFrom","outputs":[{"name":"success","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"decimals","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"}],"name":"privacyBalance","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"_owner","type":"address"}],"name":"balanceOf","outputs":[{"name":"balance","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"symbol","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"initialBase","type":"address"},{"name":"baseKeyBytes","type":"bytes"},{"name":"value","type":"uint256"}],"name":"initPrivacyAsset","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"name":"_to","type":"address"},{"name":"_value","type":"uint256"}],"name":"transfer","outputs":[{"name":"success","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"_owner","type":"address"}],"name":"otabalanceOf","outputs":[{"name":"balance","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"_owner","type":"address"},{"name":"_spender","type":"address"}],"name":"allowance","outputs":[{"name":"remaining","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"}],"name":"otaKey","outputs":[{"name":"","type":"bytes"}],"payable":false,"stateMutability":"view","type":"function"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_from","type":"address"},{"indexed":true,"name":"_to","type":"address"},{"indexed":false,"name":"_value","type":"uint256"}],"name":"Transfer","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_owner","type":"address"},{"indexed":true,"name":"_spender","type":"address"},{"indexed":false,"name":"_value","type":"uint256"}],"name":"Approval","type":"event"}]);

// the following address should be changed according to your contract deployed address
contractAddr = '0xa2e526a3632d225f15aa0592e00bed31a48c953d';
erc20simple = erc20simple_contract.at(contractAddr)

```
### 5 - Create one time address for account1
```javascript
var wanAddr = wan.getWanAddress(eth.accounts[0]);
var otaAddrTokenHolder = wan.generateOneTimeAddress(wanAddr);
keyPairs = wan.computeOTAPPKeys(eth.accounts[0], otaAddrTokenHolder).split('+');
privateKeyTokenHolder = keyPairs[0];
addrTokenHolder = keyPairs[2];
sendTx = erc20simple.initPrivacyAsset.sendTransaction(addrTokenHolder, otaAddrTokenHolder, '0x' + initialPrivateBalance.toString(16), {from:eth.accounts[0], gas:1000000});
wait(function(){return eth.getTransaction(sendTx).blockNumber != null;});

ota1Balance = erc20simple.privacyBalance(addrTokenHolder)
if (ota1Balance != initialPrivateBalance) {
	throw Error('ota1 balance wrong! balance:' + ota1Balance + ', except:' + initialPrivateBalance)
}
```

### 6 - Generate ring sign data
```javascript
var hashMsg = addrTokenHolder
var ringSignData = personal.genRingSignData(hashMsg, privateKeyStamp, mixSetWith0x.join("+"))
```

### 7 - Create one time address for account2
```javascript
var wanAddr = wan.getWanAddress(eth.accounts[1]);
var otaAddr4Account2 = wan.generateOneTimeAddress(wanAddr);
keyPairs = wan.computeOTAPPKeys(eth.accounts[1], otaAddr4Account2).split('+');
privateKeyOtaAcc2 = keyPairs[0];
addrOTAAcc2 = keyPairs[2];
```
### 8 - Generate token privacy transfer data
```javascript
cxtInterfaceCallData = erc20simple.otatransfer.getData(addrOTAAcc2, otaAddr4Account2, privateTransactionValue);
```

### 9 - Generate call token privacy transfer data
```javascript
glueContractDef = eth.contract([{"constant":false,"type":"function","inputs":[{"name":"RingSignedData","type":"string"},{"name":"CxtCallParams","type":"bytes"}],"name":"combine","outputs":[{"name":"RingSignedData","type":"string"},{"name":"CxtCallParams","type":"bytes"}]}]);
glueContract = glueContractDef.at("0x0000000000000000000000000000000000000000")
combinedData = glueContract.combine.getData(ringSignData, cxtInterfaceCallData)
```

### 10 - Send private transaction
```javascript
sendTx = personal.sendPrivacyCxtTransaction({from:addrTokenHolder, to:contractAddr, value:0, data: combinedData, gasprice:'0x' + (200000000000).toString(16)}, privateKeyTokenHolder)
wait(function(){return eth.getTransaction(sendTx).blockNumber != null;});
```

### 11 - Check balance
```javascript
ota2Balance = erc20simple.privacyBalance(addrOTAAcc2)
if (ota2Balance != privateTransactionValue) {
	throw Error("ota2 balance wrong. balance:" + ota2Balance +  ", expect:" + privateTransactionValue)
}

ota1Balance = erc20simple.privacyBalance(addrTokenHolder)
if (ota1Balance != initialPrivateBalance - privateTransactionValue) {
	throw Error("ota2 balance wrong. balance:" + ota1Balance +  ", expect:" + (initialPrivateBalance - privateTransactionValue))
}
```
