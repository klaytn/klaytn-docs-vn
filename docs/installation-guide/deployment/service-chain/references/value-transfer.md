As explained in the Klaytn design section, Service Chain supports value (KLAY, ERC-20, and ERC-721) transfer between parent chain & child chain. This page shows how to enable the value-transfer feature in SCN.

After setting up the EN and SCN, the following procedure is required to enable value-transfer between chains.

1. Check the addresses of the bridge operator accounts and add KLAY to the bridge operator accounts.
2. Deploy the bridge contract to the parent/child chains.
3. Deploy a token (ERC-20 or 721) contract to the parent/child chains. (If you just need KLAY-transfer, you can skip step 3 & 4.)
4. Register the token contracts with the bridge contracts on the parent/child chains.
5. Subscribe to the bridge contracts on the parent/child chains.

Before we follow the steps, let's take a look at the high-level system architecture to understand the behind of the mechanism.

# System Architecture <a id="system-architecture"></a>
Figure 1 shows the system architecture of the Service Chain with bridge/token contracts and bridge nodes.

Below contracts communicate with each other via main/sub-bridge to process user's value transfer requests.
- Bridge contract
- ERC-20 contract (if needed)
- ERC-721 contract (if needed)

![Figure 1. Service chain architecture](../images/sc_arch.png)

# Bridge Operator Account <a id="bridge-operator-account"></a>
For ServiceChain, there are two operator accounts: parent chain bridge operator account, service chain bridge operator account. Each operator account is used to sign transactions. If the transaction moves the value to the parent chain, the parent chain bridge operator account signs the transaction. To the child chain, the child chain bridge operator account is used. If a user submits a "request value transfer" transaction, the Sub-bridge creates a "handle value transfer" transaction signed by the bridge operator account. Therefore, the parent chain bridge operator needs enough KLAY in their balance to pay the transaction fee to the parent chain. If the service chain's gas price is set to non-zero, the service chain bridge operator should have KLAY in their balance as well.

## Keystore and Password file <a id="keystore-and-password-file"></a>
When SCN is booted, the keystore files and password files for the parent/child operators are automatically generated if their keys don't exist. If you want to use a specific account as an operator, you can provide the key. Place the below files in the designated path before booting the SCN. The password file should have a password string of the keystore file. The password file name should be the account address of the corresponding keystore file.

**files**
- keystore file : `UTC--2019-10-21T04-05-41.493850000Z--2ed72a9d7fe5da7672fd21567e07302431649b0b`
- password file : `0x2eD72a9D7fe5da7672fD21567e07302431649B0B`

**file path**
- Parent chain bridge operator : $datadir/parent_bridge_account
- Child chain bridge operator : $datadir/child_bridge_account

```javascript
> pwd
/$dataDIR/child_bridge_account

> ls
0x2eD72a9D7fe5da7672fD21567e07302431649B0B
UTC--2019-10-21T04-05-41.493850000Z--2ed72a9d7fe5da7672fd21567e07302431649b0b

> cat 0x2eD72a9D7fe5da7672fD21567e07302431649B0B
%S~f5qqM38cB47jL%

> cat UTC--2019-10-21T04-05-41.493850000Z--2ed72a9d7fe5da7672fd21567e07302431649b0b
{"address":"2ed72a9d7fe5da7672fd21567e07302431649b0b","crypto":{"cipher":"aes-128-ctr","ciphertext":"6486509e8158bf4984608cbc5562cf2c9a27cd988a98e543731b39251144e633","cipherparams":{"iv":"96d7e5b6a936278c0797faae6cb3d903"},"kdf":"scrypt","kdfparams":{"dklen":32,"n":262144,"p":1,"r":8,"salt":"8928ba41b8228af19390ec881c51452fa3ea973ad2c253ca0f5bc9197a8b24c4"},"mac":"9c8ec63694c20a473e0ea33840e7d16e9f1a20afc52b3244b703a3ac0a66cfa3"},"id":"9ae10527-7fd3-4aae-a4eb-316af211494e","version":3}
```
## Check Bridge Operator Addresses <a id="check-bridge-operator-addresses"></a>
If you run SCN successfully, you can check the parent/child chain bridge operator address using RPC API like the following.

```
$ kscn attach ~/kscnd_home/klay.ipc
Welcome to the Klaytn JavaScript console!

instance: Klaytn/vvX.X.X/XXXX-XXXX/goX.X.X

 datadir: ~/kscnd_home
 modules: admin:1.0 subbridge:1.0 debug:1.0 governance:1.0 istanbul:1.0 klay:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 servicechain:1.0 txpool:1.0

> subbridge.parentOperator
"0xA057995175B93Ee0D1bdfA54f078Ad0F0116130b"
> subbridge.childOperator
"0x5C1C757a6Cb6c6FcEFE398674D8209FDA2A74Df4"
```

You can refer to the [subbridge API](../../../../dapp/json-rpc/api-references/subbridge.md#subbridge_parentOperator) for more details.

## Send KLAY to Bridge Operators <a id="send-klay-to-bridge-operators"></a>
Like anchoring, the parent chain bridge operator needs KLAY to make a value-transfer transaction. If the service chain's gas price is set to non-zero, the service chain bridge operator should have KLAY in their balance as well.

After topping up the operator accounts, you can check their balances like below.

**Parent chain bridge operator**
```
$ kscn attach ~/kscnd_home/klay.ipc
Welcome to the Klaytn JavaScript console!

 instance: Klaytn/vvX.X.X/XXXX-XXXX/goX.X.X
 datadir: ~/kscnd_home
 modules: admin:1.0 subbridge:1.0 debug:1.0 klay:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0

> subbridge.parentOperatorBalance
1e+50
```

**Child chain bridge operator**
```
$ kscn attach ~/kscnd_home/klay.ipc
Welcome to the Klaytn JavaScript console!

 instance: Klaytn/vvX.X.X/XXXX-XXXX/goX.X.X
 datadir: ~/kscnd_home
 modules: admin:1.0 subbridge:1.0 debug:1.0 klay:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0

> subbridge.childOperatorBalance
1e+50
```

# Bridge Contract <a id="bridge-contract"></a>
For the cross-chain value transfer, a bridge contract should be deployed to the parent/child chains. Users can request a KLAY transfer to the bridge contract to send their KLAY to the other chain. Additionally, if token contracts are registered in the bridge contracts, bridge contracts can handle the token transfer between parent and child chains.

## Deployment <a id="deployment"></a>
Sub-bridge provides a bridge contract deployment API. You can deploy bridge contracts to both chains using a single RPC call as below. Trước khi làm vậy, bạn cần kết nối cầu nối chính và cầu nối con. Vui lòng tham chiếu [Cấu hình cầu nối](bridge-configuration.md) để biết hướng dẫn chi tiết.

```javascript
$ kscn attach ~/kscnd_home/klay.ipc
Chào mừng bạn đến với bảng điều khiển Klaytn JavaScript!

instance: Klaytn/vvX.X.X/XXXX-XXXX/goX.X.X

 datadir: ~/kscnd_home
 modules: admin:1.0 subbridge:1.0 debug:1.0 governance:1.0 istanbul:1.0 klay:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 servicechain:1.0 txpool:1.0

> subbridge.deployBridge()
["0x27caeba831d98b5fbb1d81ce0ed20801702f443a", "0x22c41ae528627b790233d2e59ea520be12350eb5"]

> subbridge.listBridge
[{
    localAddress: "0x27caeba831d98b5fbb1d81ce0ed20801702f443a",
    remoteAddress: "0x22c41ae528627b790233d2e59ea520be12350eb5",
    subscribed: false
}]
```
Bạn có thể tham chiếu [API cầu nối con](../../../../dapp/json-rpc/api-references/subbridge.md#subbridge_deployBridge) để biết thêm chi tiết.

`subbridge_listBridge` thể hiện địa chỉ hợp đồng cầu nối và trạng thái đăng ký của chúng. Cầu nối con lưu danh sách các địa chỉ hợp đồng cầu nối trong một tập tin. Khi khởi động lại hệ thống, cầu nối con tải lại danh sách hợp đồng cầu nối từ tập tin đó.

## Đăng ký <a id="subscribing"></a>
Sau khi triển khai hợp đồng cầu nối, bạn nên đăng ký cầu nối con với các hợp đồng cầu nối đã triển khai để kích hoạt tính năng chuyển giá trị. Bạn có thể làm việc này bằng cách sử dụng một lệnh gọi RPC API khác, `subbridge_subscribeBridge`.

```javascript
> subbridge.subscribeBridge("0x27caeba831d98b5fbb1d81ce0ed20801702f443a", "0x22c41ae528627b790233d2e59ea520be12350eb5")
null

> subbridge.listBridge
[{
    localAddress: "0x27caeba831d98b5fbb1d81ce0ed20801702f443a",
    remoteAddress: "0x22c41ae528627b790233d2e59ea520be12350eb5",
    subscribed: true
}]
```

## Kiểm tra trạng thái <a id="checking-status"></a>
Sau khi đăng ký, SCN sẽ tự động xử lý các giao dịch "yêu cầu chuyển giá trị" của người dùng. Phần này giải thích cách để kiểm tra trạng thái hợp đồng cầu nối.

Trong một hợp đồng cầu nối có hai số dùng một lần, `requestNonce` và `handleNonce`. Không giống như các giao dịch theo chuỗi, cầu nối con có thể xử lý yêu cầu số dùng một lần cao hơn trước những yêu cầu số dùng một lần thấp hơn.
- requestNonce : số lượng yêu cầu "chuyển giá trị chuỗi chéo" của người dùng với hợp đồng cầu nối này.
- handleNonce : số dùng một lần cao nhất mà cầu nối con đã xử lý.
- lowerHandleNonce : số dùng một lần thấp nhất mà cầu nối con sẽ xử lý.

Do đó, nếu số dùng một lần được cập nhật như dưới đây, chúng ta có thể nói việc chuyển giá trị chuỗi chéo đang được xử lý chính xác.
- "handleNonce" và "lowerHandleNonce" của hợp đồng cầu nối chuỗi mẹ liên tục tiến dần đến "requestNonce" của hợp đồng cầu nối chuỗi con.
- "handleNonce" và "lowerHandleNonce" liên tục tiến dần đến "requestNonce" của hợp đồng cầu nối chuỗi mẹ.

Nếu "handleNonce" bằng với "requestNonce" của hợp đồng cầu nối đối ứng và "lowerHandleNonce" lớn hơn "handleNonce" 1 đơn vị, khi đó tất cả các yêu cầu của người dùng đều đã được xử lý.

### Nhật ký <a id="log"></a>
Dưới đây là đầu ra nhật ký điển hình từ SCN trong quá trình hoạt động thông thường. Trạng thái của các hợp đồng cầu nối được in ra mỗi giây.
```
INFO[10/16,19:37:40 +09] [45] VT : Parent -> Child Chain                request=8699 handle=4826 lowerHandle=4826 pending=3873
INFO[10/16,19:37:40 +09] [45] VT : Child -> Parent Chain                request=7894 handle=4207 lowerHandle=4207 pending=3687
```
Nhật ký này hiển thị request, handle, lowerHandle, and pending nonces. Mỗi giá trị có ý nghĩa như sau

- request : tổng số dùng một lần yêu cầu chuyển giá trị của tất cả (các) hợp đồng cầu nối đã đăng ký.
- handle : tổng số dùng một lần xử lý tối đa của tất cả (các) hợp đồng cầu nối đã đăng ký.
- lowerHandle : tổng số dùng một lần xử lý tối thiểu của tất cả (các) hợp đồng cầu nối đã đăng ký.
- pending : chênh lệch số lượng giữa `request` và `lowerHandle`.

### RPC API <a id="rpc-api"></a>
Bạn có thể kiểm tra trạng thái của hợp đồng cầu nối như sau. Bạn có thể tham chiếu [API cầu nối con](../../../../dapp/json-rpc/api-references/subbridge.md#subbridge_getBridgeInformation) để biết thêm chi tiết.

```javascript
> subbridge.getBridgeInformation("0x27caeba831d98b5fbb1d81ce0ed20801702f443a")
{
  counterPart: "0x22c41ae528627b790233d2e59ea520be12350eb5",
  handleNonce: 0,
  lowerHandleNonce: 0,
  isRunning: true,
  isSubscribed: true,
  onServiceChain: true,
  pendingEventSize: 0,
  requestNonce: 0
}
```

# Hợp đồng token (ERC-20/721) <a id="token-contract-erc-20-721"></a>
Chuỗi dịch vụ cũng hỗ trợ chuyển giá trị ERC-20/721. Để hỗ trợ, chuỗi dịch vụ tương thích với các hợp đồng token ERC-20/721 nên được triển khai trên cả chuỗi mẹ và chuỗi con. Đối với mã hợp đồng token ERC-20/721, bạn có thể tham chiếu [Tiêu chuẩn token](../../../../smart-contract/token-standard.md).

## Triển khai  <a id="deployment"></a>
SCN hiện chưa hỗ trợ API triển khai các token ERC-20/721. Bạn cần triển khai các token này qua caver-js. Khi bạn triển khai hợp đồng ERC-20/721, bạn nên sử dụng đúng tài khoản toán tử cầu nối. Sử dụng tài khoản toán tử mẹ để triển khai chuỗi chính và tài khoản toán tử con để triển khai chuỗi dịch vụ. Nếu bạn dùng sai tài khoản để triển khai hợp đồng token, việc chuyển giá trị sẽ không hoạt động và bạn cần dùng đúng tài khoản để triển khai lại hợp đồng token.

## Đăng ký  <a id="register"></a>
Sau khi triển khai hợp đồng token, bạn nên đăng ký hợp đồng đó với hợp đồng cầu nối trên các chuỗi mẹ/con như dưới đây.
```javascript
> subbridge.registerToken("0x27caeba831d98b5fbb1d81ce0ed20801702f443a", "0x22c41ae528627b790233d2e59ea520be12350eb5", "0x376b72abe1b29cace831bd3f5acdfa967814c9cd", "0x53160735f7cc6ff75e48619f368bb94daff66a1b")
null
```

This command registers the child chain token ("0x376b72abe1b29cace831bd3f5acdfa967814c9cd") with the child chain bridge contract ("0x27caeba831d98b5fbb1d81ce0ed20801702f443a"). And the parent chain token ("0x53160735f7cc6ff75e48619f368bb94daff66a1b") with the parent chain bridge contract ("0x22c41ae528627b790233d2e59ea520be12350eb5").

You can refer to the [Service Chain API](../../../../dapp/json-rpc/api-references/subbridge.md#subbridge_registerToken) for more details.

# Request Value Transfer <a id="request-value-transfer"></a>
This section explains the contract methods that will be invoked by a user to request a value transfer. Request transaction does not allow zero value (KLAY/ERC-20).

## KLAY transfer <a id="klay-transfer"></a>
Users can make a "request value transfer" transaction to the **bridge contract** using the below methods.

### fallback <a id="fallback"></a>

If a user calls the fallback function of the bridge, this requests a KLAY transfer to the same account address as the requesting user in the counterpart chain.

```solidity
function () external payable;
```

### requestKLAYTransfer <a id="requestklaytransfer"></a>

If a user calls this function with `_to`, this requests a KLAY transfer to `_to` address in the counterpart chain.

```solidity
function requestKLAYTransfer(address _to, uint256 _value, bytes calldata _extraData) external payable
```

## ERC-20 transfer <a id="erc-20-transfer"></a>

### 2-Step request via Bridge contract <a id="2-step-request-via-bridge-contract"></a>
Users can make a "request value transfer" transaction to the Bridge contract using the below method after [approving](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md#approve) the token to the Bridge contract.

```solidity
function requestERC20Transfer(address _tokenAddress, address _to, uint256 _value,uint256 _feeLimit,bytes memory _extraData) external
```

### 1-Step request via ERC-20 contract <a id="1-step-request-via-erc-20-contract"></a>
Users can make a "request value transfer" transaction directly to the **ERC-20 contract** using the below method without approving. The ERC-20 contract should implement the function, then.

```solidity
function requestValueTransfer(uint256 _amount, address _to, uint256 _feeLimit, bytes calldata _extraData) external
```

## ERC-721 transfer <a id="erc-721-transfer"></a>

### 2-Step request via Bridge contract <a id="2-step-request-via-bridge-contract"></a>
Users can make a "request value transfer" transaction to the Bridge contract using the below method after [approving](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md#approve) the token to the Bridge contract.

```solidity
function requestERC721Transfer(address _tokenAddress, address _to, uint256 _tokenId, bytes memory _extraData) external
```

### 1-Step request via ERC-721 contract <a id="1-step-request-via-erc-721-contract"></a>
Users can make a "request value transfer" transaction directly to the **ERC-721 contract** using the below method without approving. The ERC-721 contract should implement the function, then.

```solidity
function requestValueTransfer(uint256 _uid, address _to) external
```

# Value Transfer Recovery
Value transfer request may be fail for a number of reasons. Say you requested KLAY transfer from subbridge to mainbridge or from mainbridge to subbridge. In that case, the bridge contract on the receiver side must have enough KLAY than the requested amount of KLAY. If not, the transfer would fail without error notification in the return value. A feature of value transfer recovery finds unhandled events and insert them into event pool again in a given interval, which means the failed transaction can be succeed again when the counterpart bridge can successfully handle that event. In case of the above example, the failed transaction would be eventually handled by value transfer recovery when the counterpart bridge has enough KLAY. In order to set the value transfer recovery as default, you need to set two properties:
```
SC_VTRECOVERY=1
SC_VTRECOVERY_INTERVAL=5
```
The value transfer recovery runs automatically by set `SC_VTRECOVERY=1`. `SC_VTRECOVERY_INTERVAL` means an interval how often the value transfer recovery is executed.

# Collecting Fee for KLAY/ERC-20 transfer <a id="collecting-fee-for-klay-erc-20-transfer"></a>
In ServiceChain, there is a fee collecting feature for KLAY/ERC-20 transfers.

**To be updated soon.**

# Customizing your Bridge Contract  <a id="customizing-your-bridge-contract"></a>
In ServiceChain, you can use your own customized Bridge contract that inherits from the original Bridge contract for your own unique service. This section explains how to customize the Bridge contract and presents the example code.

**It will be updated soon.**

