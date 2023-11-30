# Phí giao dịch <a id="transaction-fees"></a>
{% hint style="success" %}
NOTE: The transaction fee has changed with the `Kore` hardfork. If you want the previous document, please refer to [previous document](transaction-fees-previous.md).

`Kore` hardfork block numbers are as follows.
* Baobab Testnet: `#111736800`
* Cypress Mainnet: `#119750400`
{% endhint %}

The transaction fee of one transaction is calculated as follows:
```text
Transaction fee := (Gas used) x (GasPrice)
```
As an easy-to-understand analogy in this regard, suppose you're filling up gas at a gas station. The gas price is determined by the refinery every day, and today's price is $2. If you fill 15L up, then you would pay $30 = 15L x $2/1L for it, and the $30 will be paid out of your bank account. Also, the transaction will be recorded in the account book.

Transaction fee works just the same as above. The network determines the gas price for every block. Suppose the gas price for the current block is 30 ston. If a transaction submitted by `from` account was charged 21000 gas, then 630000 ston = (21000 gas * 30 ston) would be paid out of the `from` account. Also, the transaction will be recorded in the block, and it will be applied in the state of all blockchain nodes.

Summing it up again, this calculated transaction fee is subtracted from the sender's or fee payer's account. However, the fee can be deducted from the balance only if the transaction is created by klay_sendTransaction/eth_sendTransaction. Because the other transactions cannot change the state since they cannot be included in the block. They are just a simulation in some way.

This is an overall explanation of the transaction fee, and from this point, we would give a detailed explanation of how gas price is determined and how the gas is calculated.

## Tổng quan về gas và phí cơ sở <a id="gas-and-base-fee-overview"></a>
### Gas <a id="gas"></a>
Mọi hành động làm thay đổi trạng thái của chuỗi khối đều cần đến gas. Khi một nút xử lý giao dịch của người dùng, ví dụ như gửi KLAY, dùng token KIP-7, hoặc thực thi một hợp đồng, người dùng phải trả phí cho việc tính toán và sử dụng dung lượng lưu trữ. Số tiền thanh toán được xác định bằng số `gas` cần dùng.

| Network  | Before BaseFee                                                                                                              | After BaseFee                                                                                                                                                                                                |
|:-------- |:--------------------------------------------------------------------------------------------------------------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| klaytn   | tx parameter gasPrice: network-defined. must be set as the `unitPrice` </br> gasPrice: use the tx parameter gasPrice        | tx parameter gasPrice: user-defined. It means the price the most you can pay </br> (e.g. suggestGasPrice = 2*latestBlock.baseFee ) </br> gasPrice: dynamic gasPrice, `baseFee`, which is defined by network. |
| Ethereum | tx parameter gasPrice: user-defined. it means the price the most you can pay. </br> gasPrice: use the tx parameter gasPrice | tx parameter gasPrice: user-defined. It means the price the most you can pay. </br> gasPrice: dynamic gasPrice, `baseFee+tip`, which is defined by network.                                                  |

### Cơ chế phí gas động <a id="dynamic-gas-fee-mechanism"></a>
Sau khi nâng cấp căn bản Klaytn v1.9.0, một cơ chế phí gas động đã thay thế chính sách phí cố định hiện có. Chính sách phí gas động cung cấp một dịch vụ ổn định cho người dùng bằng cách ngăn chặn các hành vi lạm dụng mạng lưới và chiếm dụng dung lượng lưu trữ. Phí gas thay đổi tùy theo tình hình của mạng. Có bảy tham số ảnh hưởng đến `phí cơ sở (phí gas)`:

1. PREVIOUS_BASE_FEE: Phí cơ sở của khối trước đó
2. GAS_USED_FOR_THE_PREVIOUS_BLOCK: Lượng gas dùng để xử lý tất cả các giao dịch của khối trước đó
3. GAS_TARGET: Lượng gas quyết định việc tăng hoặc giảm phí cơ sở (hiện tại là 30 triệu)
4. MAX_BLOCK_GAS_USED_FOR_BASE_FEE: Hạn mức gas cho khối ẩn để thực thi tỷ lệ thay đổi phí cơ sở (hiện tại là 60 triệu)
5. BASE_FEE_DELTA_REDUCING_DENOMINATOR: Giá trị để đặt thay đổi phí cơ sở tối đa thành 5% mỗi khối (hiện tại là 20, có thể được nhóm quản trị thay đổi sau)
6. UPPER_BOUND_BASE_FEE: Giá trị tối đa cho phí cơ sở (hiện tại là 750 ston, có thể được nhóm quản trị thay đổi sau)
7. LOWER_BOUND_BASE_FEE: Giá trị tối thiểu cho phí cơ sở (hiện tại là 25 ston, có thể được nhóm quản trị thay đổi sau)

### Base Fee <a id="base-fee"></a>
The basic idea of this algorithm is that the `base fee` would go up if the gas used exceeds the base gas and vice versa. It is closely related to the number of transactions in the network and the gas used in the process. There is an upper and lower limit for the `base fee` to prevent the fee from increasing or decreasing indefinitely. There is also a cap for the gas and an adjustment value for the fluctuation to prevent abrupt changes in the `base fee`. The values can be changed by governance.

```text
(BASE_FEE_CHANGE_RATE) = (GAS_USED_FOR_THE_PREVIOUS_BLOCK - GAS_TARGET)
(ADJUSTED_BASE_FEE_CHANGE_RATE) = (BASE_FEE_CHANGE_RATE) / (GAS_TARGET) / (BASE_FEE_DELTA_REDUCING_DENOMINATOR)
(BASE_FEE_CHANGE_RANGE) = (PREVIOUS_BASE_FEE) * (ADJUSTED_BASE_FEE_CHANGE_RATE)
(BASE_FEE) = (PREVIOUS_BASE_FEE) + (BASE_FEE_CHANGE_RANGE) 
```

The `base fee` is calculated for every block; there could be changes every second. Transactions from a single block use the same `base fee` to calculate transaction fees. Only transactions with a gas price higher than the block `base fee` can be included in the block. Half of the transaction fee for each block is burned (BURN_RATIO = 0.5, cannot be changed by governance).

> LƯU Ý: Một tính năng quan trọng khiến Klaytn trở nên khác biệt với EIP-1559 của Ethereum là nó không có phí trả thêm. Klaytn tuân theo nguyên tắc “Ai đến trước thì được phục vụ trước” (FCFS) đối với các giao dịch của mình.

## Gas Overview <a id="gas-overview"></a>
Every action that changes the state of the blockchain requires gas. While processing the transactions in a block, such as sending KLAY, using KIP-7 tokens, or executing a contract, the user has to pay for the computation and storage usage. The payment amount is decided by the amount of `gas` required.

Klaytn hiện không cung cấp phương pháp thay thế giao dịch bằng đơn giá, nhưng có thể hỗ trợ các phương pháp thay thế giao dịch khác trong tương lai. Xin lưu ý rằng trong Ethereum, một giao dịch với một số dùng một lần nhất định có thể được thay thế bằng một giao dịch mới với giá gas cao hơn.

## Biểu giá gas của Klaytn  <a id="klaytns-gas-table"></a>

Về cơ bản, Klaytn luôn duy trì khả năng tương thích với Ethereum. Vì thế, biểu giá gas của Klaytn cũng khá tương đồng với biểu giá của Ethereum. Tuy nhiên, có một số tính năng chỉ Klaytn mới có và cần một vài hằng số mới.

{% hint style="success" %}
LƯU Ý: Bảng gas đã thay đổi cùng với việc nâng cấp giao thức `IstanbulEVM` hay còn gọi là "nâng cấp căn bản". Nếu bạn muốn đọc tài liệu trước đây, vui lòng tham khảo phần [tài liệu trước đây](transaction-fees-previous.md).

Số khối nâng cấp giao thức `IstanbulEVM` như sau.
* Mạng thử nghiệm Baobab: `#75373312`
* Mạng chính thức Cypress: `#86816005`
{% endhint %}

### Phí chung <a id="common-fee"></a>

| Mục               | Gas   | Mô tả                                                                                                                  |
|:----------------- |:----- |:---------------------------------------------------------------------------------------------------------------------- |
| G\_zero         | 0     | Không cần thanh toán cho các hoạt động của bộ Wzero                                                                    |
| G\_base         | 2     | Lượng gas phải trả cho các hoạt động của bộ Wbase                                                                      |
| G\_verylow      | 3     | Lượng gas phải trả cho các hoạt động của bộ Wverylow                                                                   |
| G\_low          | 5     | Lượng gas phải trả cho các hoạt động của bộ Wlow                                                                       |
| G\_mid          | 8     | Lượng gas phải trả cho các hoạt động của bộ Wmid                                                                       |
| G\_high         | 10    | Lượng gas phải trả cho các hoạt động của bộ Whigh                                                                      |
| G\_blockhash    | 20    | Khoản thanh toán cho hoạt động BLOCKHASH                                                                               |
| G\_extcode      | 700   | Lượng gas phải trả cho các hoạt động của bộ Wextcode                                                                   |
| G\_balance      | 700   | Lượng gas phải trả cho một hoạt động BALANCE                                                                           |
| G\_sload        | 800   | Được trả cho một hoạt động SLOAD                                                                                       |
| G\_jumpdest     | 1     | Được trả cho một hoạt động JUMPDEST                                                                                    |
| G\_sset         | 20000 | Được trả cho một hoạt động SSTORE khi giá trị lưu trữ được đặt từ số 0 sang số khác 0                                  |
| G\_sreset       | 5000  | Được trả cho một hoạt động SSTORE khi giá trị bằng không của giá trị không đổi, hoặc được đặt thành số 0               |
| G\_sclear       | 15000 | Khoản hoàn tiền đã thực hiện \(được thêm vào bộ đếm hoàn tiền\) khi giá trị lưu trữ được đặt từ số khác 0 thành số 0 |
| R\_selfdestruct | 24000 | Khoản hoàn tiền đã thực hiện \(được thêm vào bộ đếm hoàn tiền\) cho việc tự hủy một tài khoản                        |
| G\_selfdestruct | 5000  | Lượng gas phải trả cho một hoạt động SELFDESTRUCT                                                                      |
| G\_create       | 32000 | Được trả cho một hoạt động CREATE                                                                                      |
| G\_codedeposit  | 200   | Được trả theo byte cho hoạt động CREATE để thành công trong việc đặt mã vào trạng thái                                 |
| G\_call         | 700   | Được trả cho một hoạt động CALL                                                                                        |
| G\_callvalue    | 9000  | Được trả cho một giao dịch chuyển giá trị khác 0 như một phần của hoạt động CALL                                       |
| G\_callstipend  | 2300  | Khoản trợ cấp cho hợp đồng được gọi ra, được trừ khỏi Gcallvalue đối với giao dịch chuyển giá trị khác 0               |
| G\_newtài khoản | 25000 | Được trả cho hoạt động CALL hoặc SELFDESTRUCT để tạo tài khoản                                                         |
| G\_exp          | 10    | Khoản thanh toán một phần cho hoạt động EXP                                                                            |
| G\_expbyte      | 50    | Khoản thanh toán một phần khi nhân với dlog256\(exponent\)e cho hoạt động EXP                                        |
| G\_memory       | 3     | Được trả cho mỗi một từ bổ sung khi mở rộng bộ nhớ                                                                     |
| G\_txcreate     | 32000 | Được trả bởi tất cả các giao dịch tạo hợp đồng                                                                         |
| G\_transaction  | 21000 | Được trả cho mọi giao dịch                                                                                             |
| G\_log          | 375   | Khoản thanh toán một phần cho hoạt động LOG                                                                            |
| G\_logdata      | 8     | Được trả cho mỗi byte trong dữ liệu của hoạt động LOG                                                                  |
| G\_logtopic     | 375   | Được trả cho từng chủ đề của hoạt động LOG                                                                             |
| G\_sha3         | 30    | Được trả cho mỗi hoạt động SHA3                                                                                        |
| G\_sha3word     | 6     | Được trả cho từng từ \(được làm tròn\) cho dữ liệu nhập vào hoạt động SHA3                                           |
| G\_copy         | 3     | Thanh toán một phần cho các hoạt động \*COPY, nhân lên theo số từ được sao chép, được làm tròn                       |
| G\_blockhash    | 20    | Khoản thanh toán cho hoạt động BLOCKHASH                                                                               |
| G\_extcodehash  | 700   | Được trả cho việc nhận hàm băm keccak256 của mã hợp đồng                                                               |
| G\_create2      | 32000 | Được trả cho mã vận hành CREATE2, hoạt động giống hệt như CREATE nhưng dùng những đối số khác                          |

### Hợp đồng đã lập trước <a id="precompiled-contracts"></a>

Hợp đồng đã lập trước là loại hợp đồng đặc biệt, thường thực hiện các phép tính toán mã hóa phức tạp và được khởi tạo bởi những hợp đồng khác.

| Mục                     | Gas                 | Mô tả                                                          |
|:----------------------- |:------------------- |:-------------------------------------------------------------- |
| EcrecoverGas            | 3000                | Thực hiện hoạt động ECRecover                                  |
| Sha256BaseGas           | 60                  | Thực hiện hoạt động hàm băm sha256                             |
| Sha256PerWordGas        | 12                  |                                                                |
| Ripemd160BaseGas        | 600                 | Thực hiện hoạt động Ripemd160                                  |
| Ripemd160PerWordGas     | 120                 |                                                                |
| IdentityBaseGas         | 15                  |                                                                |
| IdentityPerWordGas      | 3                   |                                                                |
| ModExpQuadCoeffDiv      | 20                  |                                                                |
| Bn256AddGas             | 150                 | Thực hiện hoạt động đường cong elliptic Bn256                  |
| Bn256ScalarMulGas       | 6000                |                                                                |
| Bn256PairingBaseGas     | 45000               |                                                                |
| Bn256PairingPerPointGas | 34000               |                                                                |
| VMLogBaseGas            | 100                 | Ghi bản ghi vào tập tin bản ghi của nút - chỉ dành cho Klaytn  |
| VMLogPerByteGas         | 20                  | Chỉ dành cho Klaytn                                            |
| FeePayerGas             | 300                 | Nhận địa chỉ của feePayer - chỉ dành cho Klaytn                |
| ValidateSenderGas       | 5000 cho mỗi chữ ký | Xác thực địa chỉ và chữ ký của người gửi - chỉ dành cho Klaytn |

Tổng lượng gas của các mục có XXXBaseGas và XXXPerWordGas \(ví dụ: Sha256BaseGas, Sha256PerWordGas\) được tính như sau

```text
TotalGas = XXXBaseGas + (số từ * XXXPerWordGas)
```
IntrinsicGasCost = KeyCreationGas + KeyValidationGas + PayloadGas + TxTypedGas
```
* `PayloadGas` is calculated based on the size of the data field in the tx.
* `KeyCreationGas` is calculated when the transaction registers new keys. Only applicable in `accountUpdate` transaction.
* `KeyValidationGas` is calculated based on the number of signatures.
* `TxTypedGas` is defined based on the transaction types.

Before we get into the detail, keep in mind that not all key types apply the keyGases (`KeyCreationGas` and `KeyValidationGas`).

| Key Type  | Are those keyGases applicable?     |
|:--------- |:---------------------------------- |
| Nil       | No                                 |
| Legacy    | No                                 |
| Fail      | No                                 |
| Public    | Yes                                |
| MultiSig  | Yes                                |
| RoleBased | Depending on key types in the role |

### KeyCreationGas <a id="keyCreationGas"></a>
The KeyCreationGas is calculated as `(number of registering keys) x TxAccountCreationGasPerKey (20000)`. </br>Please Keep in mind that Public key type always has only one registering key, so the gas would be always 20000.

### KeyValidationGas <a id="keyValidationGas"></a>
The KeyValidationGas is calculated as `(number of signatures - 1) x TxValidationGasPerKey(15000)`. </br>Please keep in mind that Public key type always has only one signature key, so the gas would be always zero.

A Klaytn transaction can also have a feePayer, so the total KeyValidationGas is like this.
```

### Bảng gas liên quan đến tài khoản <a id="account-related-gas-table"></a>

| Mục                        | Gas   | Mô tả                                                            |
|:-------------------------- |:----- |:---------------------------------------------------------------- |
| TxAccountCreationGasPerKey | 20000 | Lượng gas cần thiết để tạo một cặp khóa                          |
| TxValidationGasPerKey      | 15000 | Lượng gas cần thiết để xác thực khóa                             |
| TxGasAccountUpdate         | 21000 | Lượng gas cần thiết để cập nhật một tài khoản                    |
| TxGasFeeDelegated          | 10000 | Lượng gas cần thiết cho một lượt ủy thác phí                     |
| TxGasFeeDelegatedWithRatio | 15000 | Lượng gas cần thiết để ủy thác phí kèm tỷ lệ                     |
| TxGasCancel                | 21000 | Lượng gas cần thiết để hủy một giao dịch có cùng số dùng một lần |
| TxGasValueTransfer         | 21000 | Lượng gas cần thiết để chuyển KLAY                               |
| TxGasContractExecution     | 21000 | Lượng gas cơ sở để thực thi hợp đồng                             |
| TxDataGas                  | 100   | Lượng gas cần cho mỗi byte đơn lẻ của giao dịch                  |

Lượng gas cần cho dữ liệu tải tin được tính toán như dưới đây

```text
GasPayload = number_of_bytes * TxDataGas
```

### PayloadGas <a id="payloadGas"></a>
Calculating `PayloadGas` is simple. It is calculated as `(number_of_bytes_of_tx_input) x TxDataGas(100)`

### TxTypedGas <a id="txTypedGas"></a>
There are three types of transactions in klaytn; `base`, `feeDelegated`, and `feeDelegatedWithFeeRatio`.

For example,
* TxTypeValueTransfer is the `base` type of the valueTransaction transaction.
* TxTypeFeeDelegatedValueTransfer is a `feeDelegated` type of the valueTransfer transaction.
* TxTypeFeeDelegatedValueTransferWithRatio is a `feeDelegatedWithRatio` type of the valueTransfer transaction.

This is important when calculating TxTypedGas:
* First, check the TxType is `feeDelegated` or `feeDelegatedWithFeeRatio`.
  * If the TxType is `feeDelegated`, add `TxGasFeeDelegated(10000)` to TxTypedGas
  * If the TxType is `feeDelegatedWithFeeRatio`, add `TxGasFeeDelegatedWithRatio (15000)` to TxTypedGas
* Second, check the transaction creates contract or not.
  * If the transaction creates contract, add `TxGasContractCreation (53000)` to TxTypedGas.
  * Otherwise, add `TxGas (21000)` to TxTypedGas.

For example,
* If it's legacyTransaction and creates contract, the TxTypedGas would be `0 + TxGasContractCreation(53000)`.
* If it's TxTypeFeeDelegatedValueTransfer, the TxTypedGas would be `TxGasFeeDelegated(10000) + TxGas (21000)`
* If it's TxTypeFeeDelegatedSmartContractDeployWithRatio, the TxTypedGas would be `TxGasFeeDelegatedWithRatio (15000) + TxGasContractCreation (53000)`

