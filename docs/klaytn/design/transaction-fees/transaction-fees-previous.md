# Phí giao dịch <a id="transaction-fees"></a>

{% hint style="success" %}
NOTE: This document contains the transaction fee used before the activation of the protocol upgrade. If you want the latest document, please refer to [latest document](transaction-fees.md).
{% endhint %}

The transaction fee of one transaction is calculated as follows:
```text
Transaction fee := (Gas used) x (GasPrice)
```
As an easy-to-understand analogy in this regard, suppose you're filling up gas at a gas station. The gas price is determined by the refinery every day, and today's price is $2. If you fill 15L up, then you would pay $30 = 15L x $2/1L for it, and the $30 will be paid out of your bank account. Also, the transaction will be recorded in the account book.

Transaction fee works just the same as above. The network determines the gas price for every block. Suppose the gas price for the current block is 30 ston. If a transaction submitted by `from` account was charged 21000 gas, then 630000 ston = (21000 gas * 30 ston) would be paid out of the `from` account. Also, the transaction will be recorded in the block, and it will be applied in the state of all blockchain nodes.

Summing it up again, this calculated transaction fee is subtracted from the sender's or fee payer's account. However, the fee can be deducted from the balance only if the transaction is created by klay_sendTransaction/eth_sendTransaction. Because the other transactions cannot change the state since they cannot be included in the block. They are just a simulation in some way.

This is an overall explanation of the transaction fee, and from this point, we would give a detailed explanation of how gas price is determined and how the gas is calculated.

## Unit Price Overview <a id="unit-price-overview"></a>

Mọi hành động làm thay đổi trạng thái của chuỗi khối đều cần đến gas. Khi một nút xử lý giao dịch của người dùng, ví dụ như gửi KLAY, dùng token ERC-20, hoặc thực thi một hợp đồng, người dùng phải trả phí cho việc tính toán và sử dụng dung lượng lưu trữ. Số tiền thanh toán được xác định bằng số `gas` cần dùng.

In Ethereum, users set the gas price for each transaction, and miners choose which transactions to be included in their block to maximize their reward. It is something like bidding for limited resources. This approach has been working because it is market-based. However, the transaction cost fluctuates and often becomes too high to guarantee the execution.

To solve the problem, Klaytn is using a fixed unit price and the price can be adjusted by the governance council. This policy ensures that every transaction will be handled equally and be guaranteed to be executed. Therefore, users do not need to struggle to determine the right unit price.

### Transaction Validation against Unit Price <a id="transaction-validation-against-unit-price"></a>

Klaytn only accepts transactions with gas prices, which can be set by the user, that are equal to the unit price of Klaytn; it rejects transactions with gas prices that are different from the unit price in Klaytn.

### Unit Price Error <a id="unit-price-error"></a>

The error message `invalid unit price` is returned when the gas price of a transaction is not equal to the unit price of Klaytn.

### Thay thế giao dịch <a id="transaction-replacement"></a>

Klaytn hiện không cung cấp phương pháp thay thế giao dịch bằng đơn giá, nhưng có thể hỗ trợ các phương pháp thay thế giao dịch khác trong tương lai. Xin lưu ý rằng trong Ethereum, một giao dịch với một số dùng một lần nhất định có thể được thay thế bằng một giao dịch mới với giá gas cao hơn.

## Biểu giá gas của Klaytn  <a id="klaytns-gas-table"></a>

Về cơ bản, Klaytn luôn duy trì khả năng tương thích với Ethereum. Vì thế, biểu giá gas của Klaytn cũng khá tương đồng với biểu giá của Ethereum. Tuy nhiên, do sự tồn tại của những tính năng độc đáo của Klaytn nên sẽ có một số hằng số mới cho những tính năng đó.

{% hint style="success" %}
LƯU Ý: Tài liệu này chứa biểu giá gas được sử dụng trước khi kích hoạt nâng cấp giao thức. Nếu bạn muốn nhận tài liệu mới nhất, vui lòng tham khảo [tài liệu mới nhất](transaction-fees.md).
{% endhint %}

Coming back to `IntrinsicGas`, a transaction's `intrinsicGas` can be calculated by adding up the next four factors.
```
IntrinsicGasCost = KeyCreationGas + KeyValidationGas + PayloadGas + TxTypedGas
```
* `PayloadGas` is calculated based on the size of the data field in the tx.
* `KeyCreationGas` is calculated when the transaction registers new keys. Only applicable in `accountUpdate` transaction.
* `KeyValidationGas` is calculated based on the number of signatures.
* `TxTypedGas` is defined based on the transaction types.

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
| G\_balance      | 400   | Lượng gas phải trả cho một hoạt động BALANCE                                                                           |
| G\_sload        | 200   | Được trả cho một hoạt động SLOAD                                                                                       |
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
| G\_extcodehash  | 400   | Được trả cho việc nhận hàm băm keccak256 của mã hợp đồng                                                               |
| G\_create2      | 32000 | Được trả cho mã vận hành CREATE2, hoạt động giống hệt như CREATE nhưng dùng những đối số khác                          |

| Key Type  | Are those keyGases applicable?     |
|:--------- |:---------------------------------- |
| Nil       | No                                 |
| Legacy    | No                                 |
| Fail      | No                                 |
| Public    | Yes                                |
| MultiSig  | Yes                                |
| RoleBased | Depending on key types in the role |

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
| Bn256AddGas             | 500                 | Thực hiện hoạt động đường cong elliptic Bn256                  |
| Bn256ScalarMulGas       | 40000               |                                                                |
| Bn256PairingBaseGas     | 100000              |                                                                |
| Bn256PairingPerPointGas | 80000               |                                                                |
| VMLogBaseGas            | 100                 | Ghi bản ghi vào tập tin bản ghi của nút - chỉ dành cho Klaytn  |
| VMLogPerByteGas         | 20                  | Chỉ dành cho Klaytn                                            |
| FeePayerGas             | 300                 | Nhận địa chỉ của feePayer - chỉ dành cho Klaytn                |
| ValidateSenderGas       | 5000 cho mỗi chữ ký | Xác thực địa chỉ và chữ ký của người gửi - chỉ dành cho Klaytn |

A Klaytn transaction can also have a feePayer, so the total KeyValidationGas is like this.
```
KeyValidationGas =  (KeyValidationGas for a sender) + (KeyValidationGas for a feePayer)
```

### PayloadGas <a id="payloadGas"></a>
`PayloadGas` is calculated as below.

```
# legacy-typed transaction
PayloadGas = number_of_zero_bytes x TxDataZeroGas (4) + number_of_nonzero_bytes x TxDataNonZeroGas (68)`

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

| Loại khóa | Gas                                                                                                                                                                                                                               |
|:--------- |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Nil       | Không có                                                                                                                                                                                                                          |
| Legacy    | 0                                                                                                                                                                                                                                 |
| Fail      | 0                                                                                                                                                                                                                                 |
| Public    | GasCreationPerKey \(20000\)                                                                                                                                                                                                     |
| MultiSig  | \(khóa\) \* GasCreationPerKey                                                                                                                                                                                                 |
| RoleBased | Phí gas được tính toán dựa trên các khóa trong từng vai trò. ví dụ: GasRoleTransaction = \(khóa\) _GasCreationPerKey_ _GasRoleAccountUpdate = \(khóa\)_ GasCreationPerKey GasRoleFeePayer = \(khóa\) \* GasCreationPerKey |
