# caver.contract

Đối tượng `caver.contract` giúp tương tác dễ dàng hơn với các hợp đồng thông minh trên nền tảng chuỗi khối Klaytn. Khi bạn tạo một đối tượng hợp đồng mới, bạn phải cung cấp giao diện JSON cho hợp đồng thông minh đó và caver-js sẽ tự động chuyển đổi tất cả lệnh gọi với đối tượng hợp đồng trong javascript thành lệnh gọi ABI cấp độ thấp qua RPC cho bạn.

Điều này cho phép bạn tương tác với các hợp đồng thông minh như thể chúng là các đối tượng JavaScript.

## caver.contract.create <a href="#caver-contract-create" id="caver-contract-create"></a>

```javascript
caver.contract.create(jsonInterface [, address] [, options])
```

Tạo một phiên bản hợp đồng mới với tất cả các phương thức và sự kiện được xác định trong đối tượng giao diện JSON của hợp đồng đó. Hàm này hoạt động tương tự như [caver.contract mới](caver.contract.md#new-contract).

**LƯU Ý** `caver.contract.create` được hỗ trợ kể từ phiên bản caver-js [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

**Tham số**

Xem [new caver.contract](caver.contract.md#new-contract).

**Giá trị Trả về**

Xem [new caver.contract](caver.contract.md#new-contract).

**Ví dụ**

```javascript
const contract = caver.contract.create([
    {
        constant: true,
        inputs: [{ name: 'interfaceId', type: 'bytes4' }],
        name: 'supportsInterface',
        outputs: [{ name: '', type: 'bool' }],
        payable: false,
        stateMutability: 'view',
        type: 'function',
    },
    ...
  ], '0x{address in hex}')
```

## caver.contract <a href="#new-contract" id="new-contract"></a>

```javascript
new caver.contract(jsonInterface [, address] [, options])
```

Tạo một phiên bản hợp đồng mới với tất cả các phương thức và sự kiện được xác định trong đối tượng giao diện JSON của hợp đồng đó.

**Tham số**

| Tên           | Loại      | Mô tả                                                                                                                          |
| ------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------ |
| jsonInterface | đối tượng | Giao diện JSON để khởi tạo hợp đồng                                                                                            |
| địa chỉ       | chuỗi     | (tùy chọn) Địa chỉ của hợp đồng thông minh để gọi. Có thể thêm sau bằng cách sử dụng `myContract.options.address = '0x1234..'` |
| tùy chọn      | đối tượng | (tùy chọn) Các tùy chọn của hợp đồng. Xem bảng dưới đây để biết chi tiết.                                                      |

Đối tượng tùy chọn chứa các mục sau:

| Tên           | Loại   | Mô tả                                                                                                                                                                                                                                                                            |
| ------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| từ            | chuỗi   | (tùy chọn) Địa chỉ mà các giao dịch sẽ được thực hiện.                                                                                                                                                                                                                           |
| giá gas       | chuỗi   | (tùy chọn) Giá gas tính bằng peb để sử dụng cho các giao dịch.                                                                                                                                                                                                                   |
| gas           | số      | (tùy chọn) Lượng gas tối đa được cung cấp cho một giao dịch (giới hạn gas).                                                                                                                                                                                                      |
| dữ liệu       | chuỗi   | (tùy chọn) Mã byte của hợp đồng. Được sử dụng khi hợp đồng được triển khai.                                                                                                                                                                                                      |
| feeDelegation | boolean | (tùy chọn) Có sử dụng giao dịch ủy thác phí hay không.                                                                                                                                                                                                                           |
| feePayer      | chuỗi   | (tùy chọn) Địa chỉ của người trả phí thanh toán phí giao dịch. Khi `feeDelegation` là `đúng`, giá trị sẽ được đặt thành trường `feePayer` trong giao dịch.                                                                                                                       |
| feeRatio      | chuỗi   | (tùy chọn) Tỷ lệ phí giao dịch mà người trả phí sẽ phải chịu. Nếu `feeDelegation` là `đúng` và `feeRatio` được đặt thành giá trị hợp lệ thì giao dịch ủy thác phí một phần sẽ được sử dụng. Khoảng hợp lệ là từ 1 đến 99. Tỷ lệ không được phép bằng 0 hoặc bằng và cao hơn 100. |

**Giá trị Trả về**

| Loại     | Mô tả                                                            |
| --------- | ---------------------------------------------------------------- |
| đối tượng | Phiên bản hợp đồng với tất cả các phương thức và sự kiện của nó. |

**Ví dụ**

```javascript
const myContract = new caver.contract([...], '0x{address in hex}', { gasPrice: '25000000000' })
```

## myContract.options <a href="#mycontract-options" id="mycontract-options"></a>

```javascript
myContract.options
```

Đối tượng `tùy chọn` cho phiên bản hợp đồng. `từ`, `gas`, `gasPrice`, `feePayer` và `feeRatio` được sử dụng làm giá trị dự phòng khi gửi giao dịch.

**Thuộc tính**

| Tên           | Loại   | Mô tả                                                                                                                                                                                                                                                                            |
| ------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| địa chỉ       | chuỗi   | Địa chỉ triển khai hợp đồng.                                                                                                                                                                                                                                                     |
| jsonInterface | Mảng    | Giao diện JSON của hợp đồng.                                                                                                                                                                                                                                                     |
| từ            | chuỗi   | Địa chỉ mặc định mà giao dịch triển khai/thực thi hợp đồng được gửi đi. Nếu không xác định địa chỉ `gửi đi` khi tạo giao dịch thì `myContract.options.from` sẽ luôn được sử dụng để tạo giao dịch.                                                                               |
| giá gas       | chuỗi   | Giá gas tính bằng peb để sử dụng cho các giao dịch.                                                                                                                                                                                                                              |
| gas           | số      | Lượng gas tối đa được cung cấp cho một giao dịch (giới hạn gas).                                                                                                                                                                                                                 |
| dữ liệu       | chuỗi   | Mã byte của hợp đồng. Được sử dụng khi hợp đồng được triển khai.                                                                                                                                                                                                                 |
| feeDelegation | boolean | (tùy chọn) Có sử dụng giao dịch ủy thác phí hay không.                                                                                                                                                                                                                           |
| feePayer      | chuỗi   | (tùy chọn) Địa chỉ của người trả phí thanh toán phí giao dịch. Khi `feeDelegation` là `đúng`, giá trị sẽ được đặt thành trường `feePayer` trong giao dịch.                                                                                                                       |
| feeRatio      | chuỗi   | (tùy chọn) Tỷ lệ phí giao dịch mà người trả phí sẽ phải chịu. Nếu `feeDelegation` là `đúng` và `feeRatio` được đặt thành giá trị hợp lệ thì giao dịch ủy thác phí một phần sẽ được sử dụng. Khoảng hợp lệ là từ 1 đến 99. Tỷ lệ không được phép bằng 0 hoặc bằng và cao hơn 100. |

**LƯU Ý** `feeDelegation`, `feePayer` và `feeRatio` được hỗ trợ kể từ phiên bản caver-js[v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

**Ví dụ**

```javascript
> myContract.options
{
  address: [Getter/Setter],
  jsonInterface: [Getter/Setter],
  from: [Getter/Setter],
  feePayer: [Getter/Setter],
  feeDelegation: [Getter/Setter],
  feeRatio: [Getter/Setter],
  gasPrice: [Getter/Setter],
  gas: [Getter/Setter],
  data: [Getter/Setter]
}

> myContract.options.from = '0x1234567890123456789012345678901234567891' // default from address
> myContract.options.gasPrice = '25000000000000' // default gas price in peb
> myContract.options.gas = 5000000 // provide as fallback always 5M gas
> myContract.options.feeDelegation = true // use fee delegation transaction
> myContract.options.feePayer = '0x1234567890123456789012345678901234567891' // default fee payer address
> myContract.options.feeRatio = 20 // default fee ratio when send partial fee delegation transaction
```

## myContract.options.address <a href="#mycontract-options-address" id="mycontract-options-address"></a>

```javascript
myContract.options.address
```

Địa chỉ được sử dụng cho phiên bản hợp đồng này `myContract`. Tất cả các giao dịch do caver-js tạo ra từ hợp đồng này sẽ chứa địa chỉ này dưới dạng `nơi đến` của giao dịch.

**Thuộc tính**

| Tên     | Loại    | Mô tả                                                               |
| ------- | -------- | ------------------------------------------------------------------- |
| địa chỉ | chuỗi \ | `null` | Địa chỉ cho hợp đồng này hoặc `null` nếu nó chưa được đặt. |

**Ví dụ**

```javascript
>  myContract.options.address
'0xde0b295669a9fd93d5f28d9ec85e40f4cb697bae'

// đặt địa chỉ hợp đồng
>  myContract.options.address = '0x1234FFDD...'
```

## myContract.options.jsonInterface <a href="#mycontract-options-jsoninterface" id="mycontract-options-jsoninterface"></a>

```javascript
myContract.options.jsonInterface
```

Đối tượng giao diện JSON bắt nguồn từ ABI của hợp đồng này `myContract`.

**Thuộc tính**

| Tên           | Loại | Mô tả                                                                                                           |
| ------------- | ---- | --------------------------------------------------------------------------------------------------------------- |
| jsonInterface | Mảng | Giao diện JSON cho hợp đồng này. Đặt lại điều này sẽ tạo lại các phương thức và sự kiện của phiên bản hợp đồng. |

**Ví dụ**

```javascript
> myContract.options.jsonInterface
[
  {
    constant: true,
    inputs: [ { name: 'interfaceId', type: 'bytes4' } ],
    name: 'supportsInterface',
    outputs: [ { name: '', type: 'bool' } ],
    payable: false,
    stateMutability: 'view',
    type: 'function',
    signature: '0x01ffc9a7',
  },
  ...
  {
    anonymous: false,
    inputs: [
      { indexed: true, name: 'owner', type: 'address' },
      { indexed: true, name: 'spender', type: 'address' },
      { indexed: false, name: 'value', type: 'uint256' }
    ],
    name: 'Approval',
    type: 'event',
    signature: '0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925',
  },
]

// đặt một jsonInterface mới
> myContract.options.jsonInterface = [...]
```

## myContract.clone <a href="#mycontract-clone" id="mycontract-clone"></a>

```javascript
myContract.clone([contractAddress])
```

Sao chép phiên bản hợp đồng hiện tại.

**Tham số**

| Tên             | Loại | Mô tả                                                                                                                                            |
| --------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| contractAddress | Chuỗi | (tùy chọn) Địa chỉ của hợp đồng mới. Nếu bỏ qua, địa chỉ này sẽ được đặt thành địa chỉ trong phiên bản gốc (e.g., `myContract.options.address`). |

**Giá trị Trả về**

| Loại     | Mô tả                            |
| --------- | -------------------------------- |
| đối tượng | Phiên bản hợp đồng nhân bản mới. |

**Ví dụ**

```javascript
> myContract.clone()
Contract {
  currentProvider: [Getter/Setter],
  ...
  _keyrings: KeyringContainer { ... }
}
```

## myContract.deploy <a href="#mycontract-deploy2" id="mycontract-deploy2"></a>

```javascript
myContract.deploy(options, byteCode [, param1 [, param2 [, ...]]])
```

Triển khai hợp đồng cho mạng Klaytn. Sau khi triển khai thành công, promise sẽ được xử lý bằng một phiên bản hợp đồng mới. Không giống như hàm [myContract.deploy](caver.contract.md#mycontract-deploy) hiện có được sử dụng, hàm này gửi giao dịch trực tiếp đến mạng Klaytn. Bạn không cần lệnh gọi `send()` với đối tượng được trả về.

**LƯU Ý** `caver.wallet` phải chứa các phiên bản khóa tương ứng với `địa chỉ gửi` và `feePayer` trong `tùy chọn` hoặc `myContract.options` để tạo chữ ký.

**LƯU Ý** `myContract.deploy` được hỗ trợ kể từ caver-js phiên bản [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

**Tham số**

| Tên               | Loại     | Mô tả                                                                                                                                   |
| ----------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| tùy chọn          | đối tượng | Các tùy chọn được sử dụng để gửi. Xem bảng trong [methods.methodName.send](caver.contract.md#methods-methodname-send) để biết chi tiết. |
| chỉ thị biên dịch | chuỗi     | Mã byte của hợp đồng.                                                                                                                   |
| tham số           | Hỗn hợp   | (tùy chọn) Các tham số được chuyển đến hàm tạo khi triển khai.                                                                          |

**Giá trị Trả về**

`Promise` trả về `PromiEvent`: Promise sẽ được xử lý với phiên bản hợp đồng mới.

| Loại      | Mô tả                                                                                                                                                                                   |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PromiEvent | Một bộ phát hiệu ứng kết hợp promise. Nó sẽ được xử lý khi có biên lai giao dịch. Nếu `send()` được gọi từ `myContract.deploy()`, thì promise sẽ được xử lý với phiên bản hợp đồng mới. |

Đối với PromiEvent, các sự kiện sau đây có sẵn:

* `transactionHash`: nó được kích hoạt ngay sau khi giao dịch được gửi và có sẵn hàm băm giao dịch. Loại của nó là `chuỗi`.
* `biên lai`: Nó được kích hoạt khi có sẵn biên lai giao dịch. Xem [caver.rpc.klay.getTransactionReceipt](caver.rpc/klay.md#caver-rpc-klay-gettransactionreceipt) để biết thêm chi tiết. Loại của nó là `đối tượng`.
* `lỗi`: Nó được kích hoạt nếu xảy ra lỗi trong khi gửi. Ở lỗi hết gas, thông số thứ hai là biên lai. Loại của nó là `Lỗi`.

**Ví dụ**

```javascript
// Triển khai hợp đồng thông minh mà không cần đối số hàm tạo
> myContract.deploy({
      from: '0x{address in hex}',
      gas: 1500000,
  }, '0x{byte code}')
  .on('error', function(error) { ... })
  .on('transactionHash', function(transactionHash) { ... })
  .on('receipt', function(receipt) {
     console.log(receipt.contractAddress) // contains the new contract address
   })
  .then(function(newContractInstance) {
      console.log(newContractInstance.options.address) // instance with the new contract address
  })

// Triển khai một hợp đồng thông minh với các đối số của hàm tạo
> myContract.deploy({
      from: '0x{address in hex}',
      gas: 1500000,
  }, '0x{byte code}', 'keyString', ...)
  .on('error', function(error) { ... })
  .on('transactionHash', function(transactionHash) { ... })
  .on('receipt', function(receipt) {
     console.log(receipt.contractAddress) 
   })
  .then(function(newContractInstance) {
      console.log(newContractInstance.options.address)
  })

// Triển khai hợp đồng thông minh với giao dịch ủy thác phí (TxTypeFeeDelegatedSmartContractDeploy)
> myContract.deploy({
      from: '0x{address in hex}',
      feeDelegation: true,
      feePayer: '0x{address in hex}',
      gas: 1500000,
  }, '0x{byte code}')
  .on('error', function(error) { ... })
  .on('transactionHash', function(transactionHash) { ... })
  .on('receipt', function(receipt) {
     console.log(receipt.contractAddress)
   })
  .then(function(newContractInstance) {
      console.log(newContractInstance.options.address)
  })

// Triển khai hợp đồng thông minh với giao dịch ủy thác phí một phần (TxTypeFeeDelegatedSmartContractDeployWithRatio)
> myContract.deploy({
      from: '0x{address in hex}',
      feeDelegation: true,
      feePayer: '0x{address in hex}',
      feeRatio: 30,
      gas: 1500000,
  }, '0x{byte code}')
  .on('error', function(error) { ... })
  .on('transactionHash', function(transactionHash) { ... })
  .on('receipt', function(receipt) {
     console.log(receipt.contractAddress)
   })
  .then(function(newContractInstance) {
      console.log(newContractInstance.options.address)
  })
```

## myContract.deploy <a href="#mycontract-deploy" id="mycontract-deploy"></a>

```javascript
myContract.deploy(options)
```

Trả về đối tượng được sử dụng khi triển khai hợp đồng thông minh cho Klaytn. Bạn có thể gửi giao dịch triển khai hợp đồng thông minh bằng cách gọi lệnh `myContract.deploy({ data, arguments }).send(options)`. Sau khi triển khai thành công, promise sẽ được xử lý bằng một phiên bản hợp đồng mới.

**Tham số**

| Tên      | Loại     | Mô tả                                                                          |
| -------- | --------- | ------------------------------------------------------------------------------ |
| tùy chọn | đối tượng | Đối tượng tùy chọn được sử dụng để triển khai. Xem bảng dưới đây để tìm mô tả. |

Đối tượng tùy chọn có thể chứa các mục sau:

| Tên     | Loại  | Mô tả                                                         |
| ------- | ----- | ------------------------------------------------------------- |
| dữ liệu | chuỗi | Mã byte của hợp đồng.                                         |
| đối số  | Mảng  | (tùy chọn) Các đối số được chuyển đến hàm tạo khi triển khai. |

**Giá trị Trả về**

| Type   | Description                                                                                                                    |
| ------ | ------------------------------------------------------------------------------------------------------------------------------ |
| object | An object in which arguments and functions for contract distribution are defined. See the below table to find the description. |

The object contains the following:

| Name                                                                  | Type     | Description                                                                                                                                                        |
| --------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| arguments                                                             | Array    | The arguments passed in `options.arguments`.                                                                                                                       |
| [send](caver.contract.md#methods-methodname-send)                     | function | The function that will deploy the contract to the Klaytn. The promise as the result of this function will be resolved with the new contract instance.              |
| [sign](caver.contract.md#methods-methodname-sign)                     | function | The function that will sign a smart contract deploy transaction as a sender. The sign function will return signed transaction.                                     |
| [signAsFeePayer](caver.contract.md#methods-methodname-signasfeepayer) | function | The function that will sign a smart contract deploy transaction as a fee payer. The signAsFeePayer function will return signed transaction.                        |
| [estimateGas](caver.contract.md#methods-methodname-estimategas)       | function | The function that will estimate the gas used for the deployment. The execution of this function does not deploy the contract.                                      |
| [encodeABI](caver.contract.md#methods-methodname-encodeabi)           | function | The function that encodes the ABI of the deployment, which is contract data + constructor parameters. The execution of this function does not deploy the contract. |

**NOTE** `myContract.deploy({ data, arguments }).sign(options)` and `myContract.deploy({ data, arguments }).signAsFeePayer(options)` are supported since caver-js [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

**Example**

```javascript
> myContract.deploy({
      data: '0x12345...',
      arguments: [123, 'My string']
  })
  .send({
      from: '0x1234567890123456789012345678901234567891',
      gas: 1500000,
      value: 0,
  }, function(error, transactionHash) { ... })
  .on('error', function(error) { ... })
  .on('transactionHash', function(transactionHash) { ... })
  .on('receipt', function(receipt) {
     console.log(receipt.contractAddress) // contains the new contract address
   })
  .then(function(newContractInstance) {
      console.log(newContractInstance.options.address) // instance with the new contract address
  })

// When the data is already set as an option to the contract itself
> myContract.options.data = '0x12345...'

> myContract.deploy({
        arguments: [123, 'My string']
  })
  .send({
      from: '0x1234567890123456789012345678901234567891',
      gas: 1500000,
      value: 0,
  })
  .then(function(newContractInstance) {
      console.log(newContractInstance.options.address) // instance with the new contract address
  })

// Simply encoding
> myContract.deploy({
      data: '0x12345...',
      arguments: [123, 'My string']
  })
  .encodeABI()
'0x12345...0000012345678765432'

// Gas estimation
> myContract.deploy({
      data: '0x12345...',
      arguments: [123, 'My string']
  })
  .estimateGas(function(err, gas) {
      console.log(gas)
  })
```

## myContract.send <a href="#mycontract-send" id="mycontract-send"></a>

```javascript
myContract.send(options, methodName [, param1 [, param2 [, ...]]])
```

Submits a transaction to execute the function of the smart contract. This can alter the smart contract state.

The transaction type used for this function depends on the `options` or the value defined in `myContract.options`. If you want to use a fee-delegated transaction through `myContract.send`, `feeDelegation` and `feePayer` should be set properly.

* `feeDelegation` is not defined or defined to `false`: [SmartContractExecution](caver.transaction/basic.md#smartcontractexecution)
* `feeDelegation` is defined to `true`, but `feePayer` is not defined : Throws an error.
* `feeDelegation` is defined to `true` and `feePayer` is defined, but `feeRatio` is not defined: [FeeDelegatedSmartContractExecution](caver.transaction/fee-delegation.md#feedelegatedsmartcontractexecution)
* `feeDelegation` is defined to `true` and `feePayer` and `feeRatio` are defined: [FeeDelegatedSmartContractExecutionWithRatio](caver.transaction/partial-fee-delegation.md#feedelegatedsmartcontractexecutionwithratio)

**NOTE** `caver.wallet` must contains keyring instances corresponding to `from` and `feePayer` in `options` or `myContract.options` to make signatures.

**NOTE** `myContract.send` is supported since caver-js [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

**Parameters**

| Name       | Loại     | Mô tả                                                                                                                                   |
| ---------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| tùy chọn   | đối tượng | Các tùy chọn được sử dụng để gửi. Xem bảng trong [methods.methodName.send](caver.contract.md#methods-methodname-send) để biết chi tiết. |
| methodName | chuỗi     | Tên phương thức của hàm hợp đồng để thực thi.                                                                                           |
| tham số    | Hỗn hợp   | (tùy chọn) Các tham số được chuyển đến hàm hợp đồng thông minh.                                                                         |

**Giá trị Trả về**

`Promise` trả về `PromiEvent`

| Loại      | Mô tả                                                                                                                               |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| PromiEvent | Một bộ phát hiệu ứng kết hợp promise. Nó sẽ được xử lý khi có biên lai giao dịch. Promise sẽ được xử lý với phiên bản hợp đồng mới. |

Đối với PromiEvent, các sự kiện sau đây có sẵn:

* `transactionHash`: Nó được kích hoạt ngay sau khi giao dịch được gửi và có sẵn hàm băm giao dịch. Loại của nó là `chuỗi`.
* `biên lai`: Nó được kích hoạt khi có sẵn biên lai giao dịch. Xem [caver.rpc.klay.getTransactionReceipt](caver.rpc/klay.md#caver-rpc-klay-gettransactionreceipt) để biết thêm chi tiết. Loại của nó là `đối tượng`.
* `lỗi`: Nó được kích hoạt nếu xảy ra lỗi trong khi gửi. Ở lỗi hết gas, thông số thứ hai là biên lai. Loại của nó là `Lỗi`.

**Ví dụ**

```javascript
// Gửi SmartContractExecution và sử dụng promise
> myContract.send({ from: '0x{address in hex}', gas: 1000000 }, 'methodName', 123).then(console.log)
{
  blockHash: '0x294202dcd1d3c422880e2a209b9cd70ce7036300536c78ab74130c5717cb90da',
  blockNumber: 16342,
  contractAddress: null,
  from: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  gas: '0xf4240',
  gasPrice: '0x5d21dba00',
  gasUsed: 47411,
  input: '0x983b2...',
  logsBloom: '0x00800...',
  nonce: '0x1cd',
  senderTxHash: '0xe3f50d2bab2c462ef99379860d2b634d85a0c9fba4e2b189daf1d96bd4bbf8ff',
  signatures: [ { V: '0x4e43', R: '0x2ba27...', S: '0x50d37...' } ],
  status: true,
  to: '0x361870b50834a6afc3358e81a3f7f1b1eb9c7e55',
  transactionHash: '0xe3f50d2bab2c462ef99379860d2b634d85a0c9fba4e2b189daf1d96bd4bbf8ff',
  transactionIndex: 0,
  type: 'TxTypeSmartContractExecution',
  typeInt: 48,
  value: '0x0',
  events: {...}
}

// Gửi SmartContractExecution và sử dụng bộ phát hiệu ứng
> myContract.send({ from: '0x{address in hex}', gas: 1000000 }, 'methodName', 123)
  .on('transactionHash', function(hash) {
    ...
  })
  .on('receipt', function(receipt) {
    console.log(receipt)
  })
  .on('error', console.error)

// Send a FeeDelegatedSmartContractExecution
> myContract.send({
    from: '0x{address in hex}',
    gas: 1000000,
    feeDelegation: true,
    feePayer: '0x{address in hex}',
  }, 'methodName', 123).then(console.log)
{
  blockHash: '0x149e36f279577c306fccb9779a0274e802501c32f7054c951f592778bd5c168a',
  blockNumber: 16458,
  contractAddress: null,
  feePayer: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  feePayerSignatures: [ { V: '0x4e43', R: '0x48c28...', S: '0x18413...' } ],
  from: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  gas: '0xf4240',
  gasPrice: '0x5d21dba00',
  gasUsed: 57411,
  input: '0x983b2d5600000000000000000000000022bb89bd35e7b12bd25bea4165cf0f9330032f8c',
  logsBloom: '0x00800...',
  nonce: '0x1f5',
  senderTxHash: '0x5b06ca5046229e066c11dfc0c74fcbc98509294370981f9b142378a8f2bd5fe8',
  signatures: [ { V: '0x4e44', R: '0xfb707...', S: '0x641c6...' } ],
  status: true,
  to: '0x361870b50834a6afc3358e81a3f7f1b1eb9c7e55',
  transactionHash: '0x0e04be479ad06ec87acbf49abd44f16a56390c736f0a7354860ebc7fc0f92e13',
  transactionIndex: 1,
  type: 'TxTypeFeeDelegatedSmartContractExecution',
  typeInt: 49,
  value: '0x0',
  events: {...}
}

// Gửi FeeDelegatedSmartContractExecutionWithRatio
> myContract.send({
    from: '0x{address in hex}',
    gas: 1000000,
    feeDelegation: true,
    feePayer: '0x{address in hex}',
    feeRatio: 30,
  }, 'methodName', 123).then(console.log)
{
  blockHash: '0x8f0a0137cf7e0fea503c818910140246437db36121871bc54b2ebc688873b3f3',
  blockNumber: 16539,
  contractAddress: null,
  feePayer: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  feePayerSignatures: [ { V: '0x4e43', R: '0x80db0...', S: '0xf8c7c...' } ],
  feeRatio: '0x1e',
  from: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  gas: '0xf4240',
  gasPrice: '0x5d21dba00',
  gasUsed: 62411,
  input: '0x983b2d560000000000000000000000007ad1a538041fa3ba1a721f87203cb1a3822b8eaa',
  logsBloom: '0x00800...',
  nonce: '0x219',
  senderTxHash: '0x14c7b674a0e253b31c85c7be8cbfe4bf9d86e66e940fcae34b854e25eab1ce15',
  signatures: [ { V: '0x4e43', R: '0xd57ef...', S: '0xe14f3...' } ],
  status: true,
  to: '0x361870b50834a6afc3358e81a3f7f1b1eb9c7e55',
  transactionHash: '0xfbf00ec189aeb0941d554384f1660ffdac7768b3af2bb1526bcb3983215c1183',
  transactionIndex: 0,
  type: 'TxTypeFeeDelegatedSmartContractExecutionWithRatio',
  typeInt: 50,
  value: '0x0',
  events: {...}
}
```

## myContract.sign <a href="#mycontract-sign" id="mycontract-sign"></a>

```javascript
myContract.sign(options, methodName [, param1 [, param2 [, ...]]])
```

Ký một giao dịch hợp đồng thông minh với tư cách là người gửi để triển khai hợp đồng thông minh hoặc thực thi hàm của hợp đồng thông minh.

Nếu hợp đồng thông minh được triển khai, 'hàm tạo' có thể được nhập vào tên phương thức, chẳng hạn như `myContract.sign({ from, ... }, 'constructor', byteCode, ...)`.

Loại giao dịch được sử dụng cho hàm này tùy thuộc vào `tùy chọn` hoặc giá trị được xác định trong `myContract.options`. Nếu bạn muốn sử dụng giao dịch có phí ủy thác thông qua `myContract.sign` thì `feeDelegation` phải được xác định là `đúng`.

* `feeDelegation` không được xác định hoặc được xác định là `sai`: [SmartContractDeploy](caver.transaction/basic.md#smartcontractdeploy) / [SmartContractExecution](caver.transaction/basic.md#smartcontractexecution)
* `feeDelegation` được xác định là `đúng` nhưng `feeRatio` không được xác định: [FeeDelegatedSmartContractDeploy](caver.transaction/fee-delegation.md#feedelegatedsmartcontractdeploy) / [FeeDelegatedSmartContractExecution](caver.transaction/fee-delegation.md#feedelegatedsmartcontractexecution)
* `feeDelegation` được xác định là `đúng` và `feeRatio` được xác định: [FeeDelegatedSmartContractDeployWithRatio](caver.transaction/partial-fee-delegation.md#feedelegatedsmartcontractdeploywithratio) / [FeeDelegatedSmartContractExecutionWithRatio](caver.transaction/partial-fee-delegation.md#feedelegatedsmartcontractexecutionwithratio)

**LƯU Ý** `caver.wallet` phải chứa các phiên bản khóa tương ứng với `địa chỉ gửi` trong `tùy chọn` hoặc `myContract.options` để tạo chữ ký.

**LƯU Ý** `myContract.sign` được hỗ trợ kể từ caver-js phiên bản [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

**Parameters**

| Name       | Type   | Description                                                                                                                                                                      |
| ---------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| options    | object | The options used for sending. See the table in [methods.methodName.send](caver.contract.md#methods-methodname-send) for the details.                                             |
| methodName | string | The method name of the contract function to execute. If you want to sign a transaction for deploying the smart contract, use 'constructor' string instead of method name.        |
| parameters | Mixed  | (optional) The parameters that get passed to the smart contract function. If you want to sign a smart contract deploy transaction, pass the byteCode and constructor parameters. |

**Return Value**

`Promise` returning [Transaction](caver.transaction/) - The signed smart contract transaction.

**Example**

```javascript
// Sign a SmartContractDeploy
> myContract.sign({ from: '0x{address in hex}', gas: 1000000 }, 'constructor', byteCode, 123).then(console.log)
SmartContractDeploy {
  _type: 'TxTypeSmartContractDeploy',
  _from: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x4e43', _r: '0xeb6b5...', _s: '0x5e4f9...' } ],
  _to: '0x',
  _value: '0x0',
  _input: '0x60806...',
  _humanReadable: false,
  _codeFormat: '0x0',
  _chainId: '0x2710',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x2a5'
}

// Sign a FeeDelegatedSmartContractDeploy
> myContract.sign({ from: '0x{address in hex}', feeDelegation: true, gas: 1000000 }, 'constructor', byteCode, 123).then(console.log)
FeeDelegatedSmartContractDeploy {
  _type: 'TxTypeFeeDelegatedSmartContractDeploy',
  _from: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x4e43', _r: '0xee0f5...', _s: '0x31cbf...' } ],
  _feePayer: '0x0000000000000000000000000000000000000000',
  _feePayerSignatures: [ SignatureData { _v: '0x01', _r: '0x', _s: '0x' } ],
  _to: '0x',
  _value: '0x0',
  _input: '0x60806...',
  _humanReadable: false,
  _codeFormat: '0x0',
  _chainId: '0x2710',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x320'
}

// Sign a FeeDelegatedSmartContractDeployWithRatio
> myContract.sign({ from: keyring.address, feeDelegation: true, feeRatio: 30, gas: 1000000 }, 'constructor', byteCode, 123).then(console.log)
FeeDelegatedSmartContractDeployWithRatio {
  _type: 'TxTypeFeeDelegatedSmartContractDeployWithRatio',
  _from: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x4e44', _r: '0x4c2b0...', _s: '0x47df8...' } ],
  _feePayer: '0x0000000000000000000000000000000000000000',
  _feePayerSignatures: [ SignatureData { _v: '0x01', _r: '0x', _s: '0x' } ],
  _feeRatio: '0x1e',
  _to: '0x',
  _value: '0x0',
  _input: '0x60806...',
  _humanReadable: false,
  _codeFormat: '0x0',
  _chainId: '0x2710',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x306'
}

// Sign a SmartContractExecution
> myContract.sign({ from: '0x{address in hex}', gas: 1000000 }, 'methodName', 123).then(console.log)
SmartContractExecution {
  _type: 'TxTypeSmartContractExecution',
  _from: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x4e44', _r: '0xb2846...', _s: '0x422c1...' } ],
  _to: '0x361870b50834a6afc3358e81a3f7f1b1eb9c7e55',
  _value: '0x0',
  _input: '0x983b2...',
  _chainId: '0x2710',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x23b'
}

// Sign a FeeDelegatedSmartContractExecution
> myContract.sign({
    from: '0x{address in hex}',
    gas: 1000000,
    feeDelegation: true,
  }, 'methodName', 123).then(console.log)
FeeDelegatedSmartContractExecution {
  _type: 'TxTypeFeeDelegatedSmartContractExecution',
  _from: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x4e43', _r: '0xf7676...', _s: '0x42673...' } ],
  _feePayer: '0x0000000000000000000000000000000000000000',
  _feePayerSignatures: [ SignatureData { _v: '0x01', _r: '0x', _s: '0x' } ],
  _to: '0x361870b50834a6afc3358e81a3f7f1b1eb9c7e55',
  _value: '0x0',
  _input: '0x983b2...',
  _chainId: '0x2710',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x254'
}

// Sign a FeeDelegatedSmartContractExecutionWithRatio
> myContract.sign({
    from: '0x{address in hex}',
    gas: 1000000,
    feeDelegation: true,
    feeRatio: 30,
  }, 'methodName', 123).then(console.log)
FeeDelegatedSmartContractExecutionWithRatio {
  _type: 'TxTypeFeeDelegatedSmartContractExecutionWithRatio',
  _from: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x4e44', _r: '0x58b06...', _s: '0x637ff...' } ],
  _feePayer: '0x0000000000000000000000000000000000000000',
  _feePayerSignatures: [ SignatureData { _v: '0x01', _r: '0x', _s: '0x' } ],
  _feeRatio: '0x1e',
  _to: '0x361870b50834a6afc3358e81a3f7f1b1eb9c7e55',
  _value: '0x0',
  _input: '0x983b2...',
  _chainId: '0x2710',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x262'
}
```

## myContract.signAsFeePayer <a href="#mycontract-signasfeepayer" id="mycontract-signasfeepayer"></a>

```javascript
myContract.signAsFeePayer(options, methodName [, param1 [, param2 [, ...]]])
```

Signs a smart contract transaction as a fee payer to deploy the smart contract or execute the function of the smart contract.

If a smart contract is deployed, 'constructor' can be entered in the methodName, such as `myContract.signAsFeePayer({ from, feeDelegation: true, feePayer, ... }, 'constructor', byteCode, ...)`.

The transaction type used for this function depends on the `options` or the value defined in `myContract.options`. The `signAsFeePayer` is a function that signs as a transaction fee payer, so `feeDelegation` field must be defined as `true`. Also, the address of the fee payer must be defined in the `feePayer` field.

* `feeDelegation` is not defined : Throws an error.
* `feeDelegation` is defined, but `feePayer` is not defined : Throws an error.
* `feeDelegation` is defined to `true` and `feePayer` is defined, but `feeRatio` is not defined: [FeeDelegatedSmartContractDeploy](caver.transaction/fee-delegation.md#feedelegatedsmartcontractdeploy) / [FeeDelegatedSmartContractExecution](caver.transaction/fee-delegation.md#feedelegatedsmartcontractexecution)
* `feeDelegation` is defined to `true` and `feePayer` and `feeRatio` are defined: [FeeDelegatedSmartContractDeployWithRatio](caver.transaction/partial-fee-delegation.md#feedelegatedsmartcontractdeploywithratio) / [FeeDelegatedSmartContractExecutionWithRatio](caver.transaction/partial-fee-delegation.md#feedelegatedsmartcontractexecutionwithratio)

**NOTE** `caver.wallet` must contains keyring instances corresponding to `feePayer` in `options` or `myContract.options` to make signatures.

**NOTE** `myContract.signAsFeePayer` is supported since caver-js [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

**Parameters**

| Name       | Type    | Description                                                                                                                                                                        |
| ---------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| options    | object  | The options used for sending. See the table in [methods.methodName.send](caver.contract.md#methods-methodname-send) for the details.                                               |
| methodName | string  | The method name of the contract function to execute. Nếu bạn muốn ký một giao dịch để triển khai hợp đồng thông minh, hãy sử dụng chuỗi 'hàm tạo' thay vì tên phương thức.         |
| tham số    | Hỗn hợp | (tùy chọn) Các tham số được chuyển đến hàm hợp đồng thông minh. Nếu bạn muốn ký một giao dịch triển khai hợp đồng thông minh, hãy chuyển các chỉ thị biên dịch và tham số hàm tạo. |

**Giá trị Trả về**

`Promise` trả về [Giao dịch](caver.transaction/) - Giao dịch hợp đồng thông minh đã ký.

**Ví dụ**

```javascript
// Ký FeeDelegatedSmartContractDeploy
> myContract.signAsFeePayer({ from: '0x{address in hex}', feeDelegation: true, feePayer: '0x{address in hex}', gas: 1000000 }, 'constructor', byteCode, 123).then(console.log)
FeeDelegatedSmartContractDeploy {
  _type: 'TxTypeFeeDelegatedSmartContractDeploy',
  _from: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x01', _r: '0x', _s: '0x' } ],
  _feePayer: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  _feePayerSignatures: [ SignatureData { _v: '0x4e43', _r: '0xe0641...', _s: '0x1d21e...' } ],
  _to: '0x',
  _value: '0x0',
  _input: '0x60806...',
  _humanReadable: false,
  _codeFormat: '0x0',
  _chainId: '0x2710',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x32a'
}

// Ký FeeDelegatedSmartContractDeployWithRatio
> myContract.signAsFeePayer({ from: keyring.address, feeDelegation: true, feePayer: '0x{address in hex}', feeRatio: 30, gas: 1000000 }, 'constructor', byteCode, 123).then(console.log)
FeeDelegatedSmartContractDeployWithRatio {
  _type: 'TxTypeFeeDelegatedSmartContractDeployWithRatio',
  _from: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x01', _r: '0x', _s: '0x' } ],
  _feePayer: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  _feePayerSignatures: [ SignatureData { _v: '0x4e44', _r: '0x307bd...', _s: '0x75110...' } ],
  _feeRatio: '0x1e',
  _to: '0x',
  _value: '0x0',
  _input: '0x60806...',
  _humanReadable: false,
  _codeFormat: '0x0',
  _chainId: '0x2710',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x359'
}

// Ký FeeDelegatedSmartContractExecution
> myContract.signAsFeePayer({
    from: '0x{address in hex}',
    gas: 1000000,
    feeDelegation: true,
    feePayer: '0x{address in hex}',
  }, 'methodName', 123).then(console.log)
FeeDelegatedSmartContractExecution {
  _type: 'TxTypeFeeDelegatedSmartContractExecution',
  _from: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x01', _r: '0x', _s: '0x' } ],
  _feePayer: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  _feePayerSignatures: [ SignatureData { _v: '0x4e43', _r: '0xc58ba...', _s: '0x76fdb...' } ],
  _to: '0x4a9d979707aede18fa674711f3b2fe110fac4e7e',
  _value: '0x0',
  _input: '0x983b2...',
  _chainId: '0x2710',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x36c'
}

// Ký FeeDelegatedSmartContractExecutionWithRatio
> myContract.signAsFeePayer({
    from: '0x{address in hex}',
    gas: 1000000,
    feeDelegation: true,
    feePayer: '0x{address in hex}',
    feeRatio: 30,
  }, 'methodName', 123).then(console.log)
FeeDelegatedSmartContractExecutionWithRatio {
  _type: 'TxTypeFeeDelegatedSmartContractExecutionWithRatio',
  _from: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x01', _r: '0x', _s: '0x' } ],
  _feePayer: '0x69c3a6e3485446118d8081063dcef2e65b69ae91',
  _feePayerSignatures: [ SignatureData { _v: '0x4e44', _r: '0xeb78d...', _s: '0x2864d...' } ],
  _feeRatio: '0x1e',
  _to: '0x4a9d979707aede18fa674711f3b2fe110fac4e7e',
  _value: '0x0',
  _input: '0x983b2...',
  _chainId: '0x2710',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x37b'
}
```

## myContract.call <a href="#mycontract-call" id="mycontract-call"></a>

```javascript
myContract.call('methodName', [param1 [, param2 [, ...]]])
myContract.call(options, 'methodName', [param1 [, param2 [, ...]]])
```

Sẽ gọi một phương thức hằng số và thực thi phương thức hợp đồng thông minh của nó trong Máy ảo Klaytn mà không gửi bất kỳ giao dịch nào. Lưu ý rằng việc gọi không thể thay đổi trạng thái hợp đồng thông minh.

**LƯU Ý** `myContract.call` được hỗ trợ kể từ caver-js phiên bản [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

**Tham số**

| Tên        | Loại     | Mô tả                                                                                                                                              |
| ---------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| tùy chọn   | đối tượng | (tùy chọn) Các tùy chọn được sử dụng để gọi. Xem bảng trong [methods.methodName.call](caver.contract.md#methods-methodname-call) để biết chi tiết. |
| methodName | chuỗi     | Tên phương thức của hàm hợp đồng để gọi.                                                                                                           |
| tham số    | Hỗn hợp   | (tùy chọn) Các tham số được chuyển đến hàm hợp đồng thông minh.                                                                                    |

**Giá trị Trả về**

`Promise` trả về `Hỗn hợp` - (Các) giá trị trả về của phương thức hợp đồng thông minh. Nếu trả về một giá trị duy nhất, nó sẽ được trả về như cũ. Nếu nó có nhiều giá trị trả về, nó sẽ trả về một đối tượng có thuộc tính và chỉ số.

**Ví dụ**

```javascript
> myContract.call('methodName').then(console.log)
Jasmine

> myContract.call({ from: '0x{address in hex}' }, 'methodName', 123).then(console.log)
Test Result
```

## myContract.decodeFunctionCall <a href="#mycontract-decodefunctioncall" id="mycontract-decodefunctioncall"></a>

```javascript
myContract.decodeFunctionCall(functionCall)
```

Giải mã lệnh gọi hàm và trả về tham số.

**LƯU Ý** `myContract.decodeFunctionCall` được hỗ trợ kể từ caver-js phiên bản [v1.6.3](https://www.npmjs.com/package/caver-js/v/1.6.3).

**Tham số**

| Tên          | Loại | Mô tả                           |
| ------------ | ----- | ------------------------------- |
| functionCall | chuỗi | Chuỗi lệnh gọi hàm được mã hóa. |

**Return Value**

| Type   | Description                                                                                                                                   |
| ------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| object | An object which includes plain params. You can use `result[0]` as it is provided to be accessed like an array in the order of the parameters. |

**Examples**

```javascript
// The myContract variable is instantiated with the below abi.
// [
//   {
//     constant: true,
//     inputs: [{ name: 'key', type: 'string' }],
//     name: 'get',
//     outputs: [{ name: '', type: 'string' }],
//     payable: false,
//     stateMutability: 'view',
//     type: 'function',
//   },
//   {
//     constant: false,
//     inputs: [{ name: 'key', type: 'string' }, { name: 'value', type: 'string' }],
//     name: 'set',
//     outputs: [],
//     payable: false,
//     stateMutability: 'nonpayable',
//     type: 'function',
//   },
// ]
> myContract.decodeFunctionCall('0xe942b5160000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000036b65790000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000576616c7565000000000000000000000000000000000000000000000000000000')
Result {
  '0': '2345675643',
  '1': 'Hello!%',
  __length__: 2,
  myNumber: '2345675643',
  mystring: 'Hello!%'
}
```

## myContract.methods <a href="#mycontract-methods" id="mycontract-methods"></a>

```javascript
myContract.methods.methodName([param1 [, param2 [, ...]]])
myContract.methods['methodName']([param1 [, param2 [, ...]]])
```

Creates a transaction object for that method, which then can be called, sent, estimated or ABI encoded.

The methods of this smart contract are available via:

* Method name: `myContract.methods.methodName(123)` or `myContract.methods[methodName](123)`
* Method prototype: `myContract.methods['methodName(uint256)'](123)`
* Method signature: `myContract.methods['0x58cf5f10'](123)`

This allows calling functions with the same name but different parameters from the JavaScript contract object.

## cf) \*function signature (function selector) <a href="#cf-function-signature-function-selector" id="cf-function-signature-function-selector"></a>

The first four bytes of the call data for a function call specifies the function to be called.\
It is the first (left, high-order in big-endian) four bytes of the Keccak-256 (SHA-3) hash of the signature of the function.

The function signature can be given via 2 different methods.\
`1. caver.abi.encodefunctionSignature('funcName(paramType1,paramType2,...)')`\
`2. caver.utils.sha3('funcName(paramType1,paramType2,...)').substr(0, 10)`

ex)

```javascript
caver.abi.encodefunctionSignature('methodName(uint256)')
> 0x58cf5f10

caver.utils.sha3('methodName(uint256)').substr(0, 10)
> 0x58cf5f10
```

**Parameters**

Parameters of any method that belongs to this smart contract, defined in the JSON interface.

**Return Value**

`Promise` returning `object` - An object in which arguments and functions for contract execution are defined.:

| Name                                                                  | Type     | Description                                                                                                                                                                          |
| --------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| arguments                                                             | Array    | The arguments passed to this method.                                                                                                                                                 |
| [call](caver.contract.md#methods-methodname-call)                     | function | The function that will call and execute a constant method in its smart contract on Klaytn Virtual Machine without sending a transaction (cannot alter the smart contract state).     |
| [send](caver.contract.md#methods-methodname-send)                     | function | The function that will send a transaction to the Klaytn and execute its method (can alter the smart contract state).                                                                 |
| [sign](caver.contract.md#methods-methodname-sign)                     | function | The function that will sign a transaction as a sender. The sign function will return signed transaction.                                                                             |
| [signAsFeePayer](caver.contract.md#methods-methodname-signasfeepayer) | function | The function that will sign a transaction as a fee payer. The signAsFeePayer function will return signed transaction.                                                                |
| [estimateGas](caver.contract.md#methods-methodname-estimategas)       | hàm      | Hàm đó sẽ ước tính lượng gas được sử dụng để thực thi.                                                                                                                               |
| [encodeABI](caver.contract.md#methods-methodname-encodeabi)           | hàm      | Hàm mã hóa ABI cho phương pháp này. Nó có thể được gửi bằng cách sử dụng một giao dịch, gọi phương thức hoặc chuyển sang một phương thức hợp đồng thông minh khác làm đối số của nó. |

**LƯU Ý** `ký` và `signAsFeePayer` được hỗ trợ kể từ caver-js phiên bản [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

**Ví dụ**

```javascript
// Gọi một phương thức
> myContract.methods.methodName(123).call({ ... }, function(error, result) { ... })
> myContract.methods.methodName(123).call({ ... }).then((result) => { ... })

// Gửi giao dịch cơ bản và sử dụng promise
> myContract.methods.methodName(123).send({
    from: '0x{address in hex}',
    ...
  }).then(function(receipt) {
    // biên lai cũng có thể là một phiên bản hợp đồng mới khi đến từ "contract.deploy({...}).send()"
  })

// Gửi giao dịch cơ bản và sử dụng eventEmitter
> myContract.methods.methodName(123).send({
    from: '0x{address in hex}',
    ...
  }).on('transactionHash', function(hash) {
      ...
  })
  .on('receipt', function(receipt) {
      ...
  })
  .on('error', console.error)

// Gửi giao dịch ủy thác phí và sử dụng promise
> myContract.methods.methodName(123).send({
    from: '0x{address in hex}',
    feePayer: '0x{fee-payer address}',
    feeDelegation: true,f
    ...
  }).then(function(receipt) {
    // biên lai cũng có thể là một phiên bản hợp đồng mới khi đến từ "contract.deploy({...}).send()"
  })

// Gửi giao dịch ủy thác phí một phần và sử dụng promise
> myContract.methods.methodName(123).send({
    from: '0x{address in hex}',
    feePayer: '0x{fee-payer address}',
    feeDelegation: true,
    feeRatio: 30,
    ...
  }).then(function(receipt) {
    // biên lai cũng có thể là một phiên bản hợp đồng mới khi đến từ "contract.deploy({...}).send()"
  })

// ký giao dịch cơ bản
> myContract.methods.methodName(123).sign({
    from: '0x{address in hex}',
    feeDelegation: true,
    ...
  }).then(function(signedTx) { ... })

// ký giao dịch ủy thác phí
> myContract.methods.methodName(123).sign({
    from: '0x{address in hex}',
    feeDelegation: true,
    ...
  }).then(function(signedTx) { ... })

// ký giao dịch ủy thác phí một phần
> myContract.methods.methodName(123).sign({
    from: '0x{address in hex}',
    feeDelegation: true,
    feeRatio: 30,
    ...
  }).then(function(signedTx) { ... })

// ký giao dịch ủy thác phí với tư cách là người trả phí
> myContract.methods.methodName(123).signAsFeePayer({
    from: '0x{address in hex}',
    feePayer: '0x{fee-payer address}',
    feeDelegation: true,
    ...
  }).then(function(signedTx) { ... })

// ký giao dịch ủy thác phí một phần với tư cách là người trả phí
> myContract.methods.methodName(123).signAsFeePayer({
    from: '0x{address in hex}',
    feePayer: '0x{fee-payer address}',
    feeDelegation: true,
    feeRatio: 30,
    ...
  }).then(function(signedTx) { ... })
```

## methods.methodName.call <a href="#methods-methodname-call" id="methods-methodname-call"></a>

```javascript
myContract.methods.methodName([param1 [, param2 [, ...]]]).call(options [, callback])
myContract.methods['methodName']([param1 [, param2 [, ...]]]).call(options [, callback])
```

Sẽ gọi một phương thức hằng số và thực thi phương thức hợp đồng thông minh của nó trong Máy ảo Klaytn mà không gửi bất kỳ giao dịch nào. Lưu ý rằng việc gọi không thể thay đổi trạng thái hợp đồng thông minh. Bạn nên sử dụng [myContract.call](caver.contract.md#mycontract-call) được cung cấp dưới dạng hàm rút gọn.

**Tham số**

| Tên      | Loại     | Mô tả                                                                                                                                                                 |
| -------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| tùy chọn | đối tượng | (tùy chọn) Các tùy chọn được sử dụng để gọi. Xem bảng dưới đây để biết chi tiết.                                                                                      |
| gọi lại  | hàm       | (tùy chọn) Lệnh gọi lại này sẽ được kích hoạt với kết quả thực thi phương thức hợp đồng thông minh làm đối số thứ hai hoặc với một đối tượng lỗi làm đối số đầu tiên. |

Đối tượng tùy chọn có thể chứa các mục sau:

| Tên     | Loại  | Mô tả                                                                       |
| ------- | ----- | --------------------------------------------------------------------------- |
| từ      | chuỗi | (tùy chọn) Địa chỉ mà các phương thức hợp đồng gọi sẽ được thực hiện từ đó. |
| giá gas | chuỗi | (tùy chọn) Giá gas tính bằng peb để sử dụng cho lệnh gọi này.               |
| gas     | số    | (tùy chọn) Lượng gas tối đa được cung cấp cho lệnh gọi này (giới hạn gas).  |

**Giá trị Trả về**

`Promise` returning `Mixed` - The return value(s) of the smart contract method. If it returns a single value, it is returned as it is. If it has multiple return values, it returns an object with properties and indices.

**Example**

```javascript
// using the promise
> myContract.methods.methodName(123).call({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
  .then(function(result) {
      ...
  })
```

```solidity
// Solidity: MULTIPLE RETURN VALUES
contract MyContract {
    function myFunction() public returns(uint256 myNumber, string memory myString) {
        return (23456, "Hello!%");
    }
}
```

```javascript
> var MyContract = new caver.contract(abi, address)
> MyContract.methods.myfunction().call().then(console.log)
Result {
      mynumber: '23456',
      mystring: 'Hello!%',
      0: '23456',
      1: 'Hello!%'
}
```

```solidity
// Solidity: SINGLE RETURN VALUE
contract MyContract {
    function myfunction() public returns(string memory mystring) {
        return "Hello!%";
    }
}
```

```javascript
> var MyContract = new caver.contract(abi, address)
> MyContract.methods.myfunction().call().then(console.log)
"Hello!%"
```

## methods.methodName.send <a href="#methods-methodname-send" id="methods-methodname-send"></a>

```javascript
myContract.methods.methodName([param1 [, param2 [, ...]]]).send(options [, callback])
myContract.methods['methodName']([param1 [, param2 [, ...]]]).send(options [, callback])
```

Will send a transaction to deploy the smart contract or execute the function of the smart contract. This can alter the smart contract state. It is recommended to use [myContract.send](caver.contract.md#mycontract-send) provided as a short-cut function.

If a smart contract is deployed, 'constructor' can be entered in the methodName, such as `myContract.methods.constructor` or `myContract.methods['constructor']`, but it is recommended to use the [myContract.deploy](caver.contract.md#mycontract-deploy2) function.

The transaction type used for this function depends on the `options` or the value defined in `myContract.options`. If you want to use a fee-delegated transaction through `methods.methodName.send`, `feeDelegation` and `feePayer` should be set properly.

* `feeDelegation` is not defined or defined to `false`: [SmartContractDeploy](caver.transaction/basic.md#smartcontractdeploy) / [SmartContractExecution](caver.transaction/basic.md#smartcontractexecution)
* `feeDelegation` is defined to `true`, but `feePayer` is not defined : Throws an error.
* `feeDelegation` is defined to `true` and `feePayer` is defined, but `feeRatio` is not defined: [FeeDelegatedSmartContractDeploy](caver.transaction/fee-delegation.md#feedelegatedsmartcontractdeploy) / [FeeDelegatedSmartContractExecution](caver.transaction/fee-delegation.md#feedelegatedsmartcontractexecution)
* `feeDelegation` is defined to `true` and `feePayer` and `feeRatio` are defined: [FeeDelegatedSmartContractDeployWithRatio](caver.transaction/partial-fee-delegation.md#feedelegatedsmartcontractdeploywithratio) / [FeeDelegatedSmartContractExecutionWithRatio](caver.transaction/partial-fee-delegation.md#feedelegatedsmartcontractexecutionwithratio)

**NOTE** `caver.wallet` must contains keyring instances corresponding to `from` and `feePayer` in `options` or `myContract.options` to make signatures.

**Parameters**

| Name     | Type     | Description                                                                                                             |
| -------- | -------- | ----------------------------------------------------------------------------------------------------------------------- |
| options  | object   | The options used for sending. See the table below for the details.                                                      |
| callback | function | (optional) This callback will be fired first with the "transactionHash", or with an error object as the first argument. |

The options object can contain the following:

| Name          | Type      | Description                                                                                                                                                                                                                                                                                                                                 |
| ------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| from          | string    | The address from which the transaction should be sent. If omitted, `myContract.options.from` will be used.                                                                                                                                                                                                                                  |
| gas           | number    | The maximum gas provided for this transaction (gas limit).                                                                                                                                                                                                                                                                                  |
| gasPrice      | string    | (optional) The gas price in peb to use for this transaction.                                                                                                                                                                                                                                                                                |
| value         | number \ | string \| BN \| Bignumber | (optional) The value in peb to be transferred to the address of the smart contract by this transaction.                                                                                                                                                                                                       |
| feeDelegation | boolean   | (tùy chọn, mặc định `sai`) Có sử dụng giao dịch ủy thác phí hay không. Nếu bỏ qua, `myContract.options.feeDelegation` sẽ được sử dụng.                                                                                                                                                                                                      |
| feePayer      | chuỗi     | (tùy chọn) Địa chỉ của người trả phí thanh toán phí giao dịch. Khi `feeDelegation` là `đúng`, giá trị sẽ được đặt thành trường `feePayer` trong giao dịch. Nếu bỏ qua, `myContract.options.feePayer` sẽ được sử dụng.                                                                                                                       |
| feeRatio      | chuỗi     | (tùy chọn) Tỷ lệ phí giao dịch mà người trả phí sẽ phải chịu. Nếu `feeDelegation` là `đúng` và `feeRatio` được đặt thành giá trị hợp lệ thì giao dịch ủy thác phí một phần sẽ được sử dụng. Khoảng hợp lệ là từ 1 đến 99. Tỷ lệ không được phép bằng 0 hoặc bằng và cao hơn 100. Nếu bỏ qua, `myContract.options.feeRatio` sẽ được sử dụng. |

**LƯU Ý** `feeDelegation`, `feePayer` và `feeRatio` được hỗ trợ kể từ phiên bản caver-js[v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

**Giá trị Trả về**

`Promise` trả về `PromiEvent`

| Loại      | Mô tả                                                                                                                               |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| PromiEvent | Một bộ phát hiệu ứng kết hợp promise. Nó sẽ được xử lý khi có biên lai giao dịch. Promise sẽ được xử lý với phiên bản hợp đồng mới. |

Đối với PromiEvent, các sự kiện sau đây có sẵn:

* `transactionHash`: Nó được kích hoạt ngay sau khi giao dịch được gửi và có sẵn hàm băm giao dịch. Loại của nó là `chuỗi`.
* `biên lai`: Nó được kích hoạt khi có sẵn biên lai giao dịch. Xem [caver.rpc.klay.getTransactionReceipt](caver.rpc/klay.md#caver-rpc-klay-gettransactionreceipt) để biết thêm chi tiết. Loại của nó là `đối tượng`.
* `lỗi`: Nó được kích hoạt nếu xảy ra lỗi trong khi gửi. Ở lỗi hết gas, thông số thứ hai là biên lai. Loại của nó là `Lỗi`.

**Ví dụ**

```javascript
// sử dụng promise
> myContract.methods.methodName(123).send({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
  .then(function(receipt) {
    // receipt can also be a new contract instance, when coming from a "contract.deploy({...}).send()"
  })


// sử dụng bộ phát hiệu ứng
> myContract.methods.methodName(123).send({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
  .on('transactionHash', function(hash) {
    ...
  })
  .on('receipt', function(receipt) {
    console.log(receipt)
  })
  .on('error', console.error) // If there is an out-of-gas error, the second parameter is the receipt.

// ví dụ biên lai
{
   "transactionHash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",
   "transactionIndex": 0,
   "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
   "blocknumber": 3,
   "contractAddress": "0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe",
   "gasUsed": 30234,
   "events": {
     "eventName": {
       returnValues: {
         myIndexedParam: 20,
         myOtherIndexedParam: '0x123456789...',
         myNonIndexParam: 'My string'
       },
       raw: {
         data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
         topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
       },
       event: 'eventName',
       signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
       logIndex: 0,
       transactionIndex: 0,
       transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
       blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
       blocknumber: 1234,
       address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
    },
    "MyOtherEvent": {
      ...
    },
    "MyMultipleEvent":[{...}, {...}] // Nếu có nhiều sự kiện giống nhau, chúng sẽ nằm trong một mảng.
  }
}

// Triển khai hợp đồng
> myContract.methods.constructor('0x{byte code}', 123).send({ from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe', gas: 1000000 })
> myContract.methods['constructor']('0x{byte code}', 123).send({ from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe', gas: 1000000 })
```

## methods.methodName.sign <a href="#methods-methodname-sign" id="methods-methodname-sign"></a>

```javascript
myContract.methods.methodName([param1 [, param2 [, ...]]]).sign(options)
myContract.methods['methodName']([param1 [, param2 [, ...]]]).sign(options)
```

Ký một giao dịch hợp đồng thông minh với tư cách là người gửi để triển khai hợp đồng thông minh hoặc thực thi hàm của hợp đồng thông minh. Bạn nên sử dụng [myContract.sign](caver.contract.md#mycontract-sign) được cung cấp dưới dạng hàm rút gọn.

Nếu một hợp đồng thông minh được triển khai, 'hàm tạo' có thể được nhập vào tên phương thức chẳng hạn như `myContract.methods.constructor` hoặc `myContract.methods['constructor']`.

Loại giao dịch được sử dụng cho hàm này tùy thuộc vào `tùy chọn` hoặc giá trị được xác định trong `myContract.options`. Nếu bạn muốn sử dụng giao dịch ủy quyền phí thông qua `methods.methodName.sign` thì `feeDelegation` phải được xác định là `đúng`.

* `feeDelegation` không được xác định hoặc được xác định là `sai`: [SmartContractDeploy](caver.transaction/basic.md#smartcontractdeploy) / [SmartContractExecution](caver.transaction/basic.md#smartcontractexecution)
* `feeDelegation` được xác định là `đúng` nhưng `feeRatio` không được xác định: [FeeDelegatedSmartContractDeploy](caver.transaction/fee-delegation.md#feedelegatedsmartcontractdeploy) / [FeeDelegatedSmartContractExecution](caver.transaction/fee-delegation.md#feedelegatedsmartcontractexecution)
* `feeDelegation` is defined to `true` and `feeRatio` is defined: [FeeDelegatedSmartContractDeployWithRatio](caver.transaction/partial-fee-delegation.md#feedelegatedsmartcontractdeploywithratio) / [FeeDelegatedSmartContractExecutionWithRatio](caver.transaction/partial-fee-delegation.md#feedelegatedsmartcontractexecutionwithratio)

**NOTE** `caver.wallet` must contains keyring instances corresponding to `from` in `options` or `myContract.options` to make signatures.

**NOTE** `methods.methodName.sign` is supported since caver-js [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

**Parameters**

| Name    | Type   | Description                                                                                                                                                   |
| ------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| options | object | The options used for creating a transaction. See the parameter table in [methods.methodName.send](caver.contract.md#methods-methodname-send) for the details. |

**Return Value**

`Promise` returning [Transaction](caver.transaction/) - The signed smart contract transaction.

**Example**

```javascript
// Sign a SmartContractDeploy transaction
> myContract.methods.constructor(byteCode, 123).sign({ from: '0x{address in hex}', gas: 1000000 }).then(console.log)
SmartContractDeploy {
  _type: 'TxTypeSmartContractDeploy',
  _from: '0x60498fefbf1705a3db8d7bb5c80d5238956343e5',
  _gas: '0xf4240',
  _signatures: [
    SignatureData {
      _v: '0x07f6',
      _r: '0x26a05...',
      _s: '0x3e3e4...'
    }
  ],
  _to: '0x',
  _value: '0x0',
  _input: '0x60806...',
  _humanReadable: false,
  _codeFormat: '0x0',
  _chainId: '0x3e9',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x2f6'
}
> myContract.methods['constructor'](byteCode, 123).sign({ from: '0x{address in hex}', gas: 1000000 }).then(console.log)

// Sign a FeeDelegatedSmartContractDeploy transaction
> myContract.methods.constructor(byteCode, 123).sign({ from: '0x{address in hex}', feeDelegation: true, gas: 1000000 }).then(console.log)
FeeDelegatedSmartContractDeploy {
  _type: 'TxTypeFeeDelegatedSmartContractDeploy',
  _from: '0x60498fefbf1705a3db8d7bb5c80d5238956343e5',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x07f5', _r: '0xa74f7...', _s: '0x0991e...' } ],
  _feePayer: '0x0000000000000000000000000000000000000000',
  _feePayerSignatures: [ SignatureData { _v: '0x01', _r: '0x', _s: '0x' } ],
  _to: '0x',
  _value: '0x0',
  _input: '0x60806...',
  _humanReadable: false,
  _codeFormat: '0x0',
  _chainId: '0x3e9',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x2f6'
}
> myContract.methods['constructor'](byteCode, 123).sign({ from: '0x{address in hex}', feeDelegation: true, gas: 1000000 }).then(console.log)

// Sign a SmartContractExecution transaction
> myContract.methods.methodName('0x...').sign({ from: '0x{address in hex}', gas: 1000000 }).then(console.log)
SmartContractExecution {
  _type: 'TxTypeSmartContractExecution',
  _from: '0x60498fefbf1705a3db8d7bb5c80d5238956343e5',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x07f5', _r: '0xafbf9...', _s: '0x10ea0...' } ],
  _to: '0xbc6723431a57abcacc4016ae664ee778d313ca6e',
  _value: '0x0',
  _input: '0x983b2d5600000000000000000000000060498fefbf1705a3db8d7bb5c80d5238956343e5',
  _chainId: '0x3e9',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x2f6'
}

> myContract.methods['methodName']('0x...').sign({ from: '0x{address in hex}', gas: 1000000 }).then(console.log)

// Sign a FeeDelegatedSmartContractExecution transaction
> myContract.methods.methodName('0x...').sign({ from: '0x{address in hex}', feeDelegation: true, gas: 1000000 }).then(console.log)
FeeDelegatedSmartContractExecution {
  _type: 'TxTypeFeeDelegatedSmartContractExecution',
  _from: '0x60498fefbf1705a3db8d7bb5c80d5238956343e5',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x07f6', _r: '0xdfc14...', _s: '0x38b9c...' } ],
  _feePayer: '0x0000000000000000000000000000000000000000',
  _feePayerSignatures: [ SignatureData { _v: '0x01', _r: '0x', _s: '0x' } ],
  _to: '0xbc6723431a57abcacc4016ae664ee778d313ca6e',
  _value: '0x0',
  _input: '0x983b2d5600000000000000000000000060498fefbf1705a3db8d7bb5c80d5238956343e5',
  _chainId: '0x3e9',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x2f6'
}
> myContract.methods['methodName']('0x...').sign({ from: '0x{address in hex}', feeDelegation: true, gas: 1000000 }).then(console.log)
```

## methods.methodName.signAsFeePayer <a href="#methods-methodname-signasfeepayer" id="methods-methodname-signasfeepayer"></a>

```javascript
myContract.methods.methodName([param1 [, param2 [, ...]]]).signAsFeePayer(options)
myContract.methods['methodName']([param1 [, param2 [, ...]]]).signAsFeePayer(options)
```

Signs a smart contract transaction as a fee payer to deploy the smart contract or execute the function of the smart contract. It is recommended to use [myContract.signAsFeePayer](caver.contract.md#mycontract-signasfeepayer) provided as a short-cut function.

If a smart contract is deployed, 'constructor' can be entered in the methodName, such as `myContract.methods.constructor` or `myContract.methods['constructor']`.

The transaction type used for this function depends on the `options` or the value defined in `myContract.options`. The `signAsFeePayer` is a function that signs as a transaction fee payer, so `feeDelegation` field must be defined as `true`. Also, the address of the fee payer must be defined in the `feePayer` field.

* `feeDelegation` is not defined : Throws an error.
* `feeDelegation` is defined, but `feePayer` is not defined : Throws an error.
* `feeDelegation` is defined to `true` and `feePayer` is defined, but `feeRatio` is not defined: [FeeDelegatedSmartContractDeploy](caver.transaction/fee-delegation.md#feedelegatedsmartcontractdeploy) / [FeeDelegatedSmartContractExecution](caver.transaction/fee-delegation.md#feedelegatedsmartcontractexecution)
* `feeDelegation` is defined to `true` and `feePayer` and `feeRatio` are defined: [FeeDelegatedSmartContractDeployWithRatio](caver.transaction/partial-fee-delegation.md#feedelegatedsmartcontractdeploywithratio) / [FeeDelegatedSmartContractExecutionWithRatio](caver.transaction/partial-fee-delegation.md#feedelegatedsmartcontractexecutionwithratio)

**NOTE** `caver.wallet` must contains keyring instances corresponding to `feePayer` in `options` or `myContract.options` to make signatures.

**NOTE** `methods.methodName.signAsFeePayer` is supported since caver-js [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

**Parameters**

| Name    | Type   | Description                                                                                                                                                   |
| ------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| options | object | The options used for creating a transaction. See the parameter table in [methods.methodName.send](caver.contract.md#methods-methodname-send) for the details. |

**Return Value**

`Promise` returning [Transaction](caver.transaction/) - The signed smart contract transaction.

**Example**

```javascript
// Sign a FeeDelegatedSmartContractDeploy transaction
> myContract.methods.constructor(byteCode, 123).signAsFeePayer({ from: '0x{address in hex}', feeDelegation: true, feePayer: '0x{address in hex}', gas: 1000000 }).then(console.log)
> FeeDelegatedSmartContractDeploy {
  _type: 'TxTypeFeeDelegatedSmartContractDeploy',
  _from: '0x60498fefbf1705a3db8d7bb5c80d5238956343e5',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x01', _r: '0x', _s: '0x' } ],
  _feePayer: '0x60498fefbf1705a3db8d7bb5c80d5238956343e5',
  _feePayerSignatures: [ SignatureData { _v: '0x07f6', _r: '0x2c385...', _s: '0x7fa79...' } ],
  _to: '0x',
  _value: '0x0',
  _input: '0x60806...',
  _humanReadable: false,
  _codeFormat: '0x0',
  _chainId: '0x3e9',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x2f6'
}
> myContract.methods['constructor'](byteCode, 123).signAsFeePayer({ from: '0x{address in hex}', feeDelegation: true, feePayer: '0x{address in hex}', gas: 1000000 }).then(console.log)

// Sign a FeeDelegatedSmartContractExecution transaction
> myContract.methods.methodName(123).signAsFeePayer({ from: '0x{address in hex}', feeDelegation: true, feePayer: '0x{address in hex}', gas: 1000000 }).then(console.log)
> FeeDelegatedSmartContractExecution {
  _type: 'TxTypeFeeDelegatedSmartContractExecution',
  _from: '0x60498fefbf1705a3db8d7bb5c80d5238956343e5',
  _gas: '0xf4240',
  _signatures: [ SignatureData { _v: '0x01', _r: '0x', _s: '0x' } ],
  _feePayer: '0x60498fefbf1705a3db8d7bb5c80d5238956343e5',
  _feePayerSignatures: [ SignatureData { _v: '0x07f6', _r: '0x793eb...', _s: '0x0f776...' } ],
  _to: '0x294b2618f29714732cfc202d7be53bf5efee90dd',
  _value: '0x0',
  _input: '0x983b2d5600000000000000000000000060498fefbf1705a3db8d7bb5c80d5238956343e5',
  _chainId: '0x3e9',
  _gasPrice: '0x5d21dba00',
  _nonce: '0x2f6'
}
> myContract.methods['methodName'](123).signAsFeePayer({ from: '0x{address in hex}', feeDelegation: true, feePayer: '0x{address in hex}', gas: 1000000 }).then(console.log)
```

## methods.methodName.estimateGas <a href="#methods-methodname-estimategas" id="methods-methodname-estimategas"></a>

```javascript
myContract.methods.methodName([param1 [, param2 [, ...]]]).estimateGas(options [, callback])
```

Will estimate the gas that a method execution will take when executed in the Klaytn Virtual Machine. Ước tính có thể khác với gas thực tế được sử dụng khi gửi giao dịch sau này, vì trạng thái của hợp đồng thông minh có thể khác vào thời điểm đó.

**Tham số**

| Tên      | Loại     | Mô tả                                                                                                                                     |
| -------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| tùy chọn | đối tượng | (tùy chọn) Các tùy chọn được sử dụng để gọi. Xem bảng dưới đây để biết chi tiết.                                                          |
| gọi lại  | hàm       | (tùy chọn) Lệnh gọi lại này sẽ được kích hoạt với kết quả ước tính gas làm đối số thứ hai hoặc với một đối tượng lỗi làm đối số đầu tiên. |

Đối tượng tùy chọn có thể chứa các mục sau:

| Tên     | Loại | Mô tả                                                                                                                                                                        |
| ------- | ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| từ      | chuỗi | (tùy chọn) Địa chỉ mà từ đó việc gọi phương thức hợp đồng sẽ được thực hiện.                                                                                                 |
| gas     | số    | (tùy chọn) Lượng gas tối đa được cung cấp cho lệnh gọi này (giới hạn gas). Đặt một giá trị cụ thể giúp phát hiện lỗi hết gas. Nếu dùng hết gas sẽ về số như cũ.              |
| giá trị | số \ | chuỗi \| BN \| Bignumber | (tùy chọn) Giá trị trong peb sẽ được chuyển đến địa chỉ của hợp đồng thông minh nếu giao dịch để thực thi hàm hợp đồng này được gửi tới Klaytn. |

**Giá trị Trả về**

`Promise` trả về `số`

| Loại | Mô tả                                             |
| ----- | ------------------------------------------------- |
| số    | Gas được sử dụng cho lệnh gọi/giao dịch mô phỏng. |

**Ví dụ**

```javascript
> myContract.methods.methodName(123).estimateGas({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
  .then(function(gasAmount) {
    ...
  })
  .catch(function(error) {
    ...
  })
```

## methods.methodName.encodeABI <a href="#methods-methodname-encodeabi" id="methods-methodname-encodeabi"></a>

```javascript
myContract.methods.methodName([param1 [, param2[, ...]]]).encodeABI()
```

Mã hóa ABI cho phương pháp này. Nó có thể được sử dụng để gửi một giao dịch hoặc gọi một phương thức hoặc chuyển nó vào một phương thức hợp đồng thông minh khác làm đối số.

**Tham số**

Các tham số của bất kỳ phương thức nào thuộc về hợp đồng thông minh này, được xác định trong giao diện JSON.

**Giá trị Trả về**

| Loại | Mô tả                                                       |
| ----- | ----------------------------------------------------------- |
| chuỗi | Mã byte ABI được mã hóa để gửi qua giao dịch hoặc cuộc gọi. |

**Ví dụ**

```javascript
> myContract.methods.methodName(123).encodeABI()
'0x58cf5f1000000000000000000000000000000000000000000000000000000000000007B'
```

## myContract.once <a href="#mycontract-once" id="mycontract-once"></a>

```javascript
myContract.once(event [, options], callback)
```

Subscribes to an event and unsubscribes immediately after the first event or error. Will only fire for a single event.

**Parameters**

| Name     | Type     | Description                                                                                                                                                                                                       |
| -------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| event    | string   | The name of the event in the contract, or `allEvents` to get all events.                                                                                                                                          |
| options  | object   | (optional) The options used for subscription. See the table below for the details.                                                                                                                                |
| callback | function | This callback will be fired for the first event as the second argument, or an error as the first argument. See [myContract.getPastEvents](caver.contract.md#getpastevents) for details about the event structure. |

The options object can contain the following:

| Name   | Type   | Description                                                                                                                                                           |
| ------ | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter | object | (optional) Lets you filter events by indexed parameters, _e.g._, `{filter: {mynumber: [12,13]}}` means all events where "mynumber" is 12 or 13.                       |
| topics | Array  | (optional) This allows you to manually set the topics for the event filter. Given the filter property and event signature, `topic[0]` would not be set automatically. |

**Return Value**

`Promise` returns `object` - An event object. For more detail about event object, please refer to [myContract.getPastEvents](caver.contract.md#getpastevents).

**Example**

```javascript
> myContract.once('eventName', {
    filter: {myIndexedParam: [20,23], myOtherIndexedParam: '0x123456789...'}, // Using an array means OR: e.g. 20 or 23
  }, function(error, event) { console.log(event) })

// event output example
{
    returnValues: {
        myIndexedParam: 20,
        myOtherIndexedParam: '0x123456789...',
        myNonIndexParam: 'My string'
    },
    raw: {
        data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
    },
    event: 'eventName',
    signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    logIndex: 0,
    transactionIndex: 0,
    transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
    blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    blocknumber: 1234,
    address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
}
```

## myContract.subscribe <a href="#mycontract-subscribe" id="mycontract-subscribe"></a>

```javascript
myContract.subscribe(event [, options], callback)
```

Subscribes to an event. This function works same as [myContract.events.eventName](caver.contract.md#mycontract-events).

You can unsubscribe an event by calling the `unsubscribe` function of the subscription object returned by the `subscribe` function.

**NOTE** `myContract.subscribe` is supported since caver-js [v1.9.1-rc.1](https://www.npmjs.com/package/caver-js/v/1.9.1-rc.1).

**Parameters**

| Name    | Type   | Description                                                                                                                                                                                                |
| ------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| event   | string | The name of the event in the contract, or `allEvents` to get all events.                                                                                                                                   |
| options | object | (optional) The options used for subscription. Xem bảng dưới đây để biết chi tiết.                                                                                                                          |
| gọi lại | hàm    | Lệnh gọi lại này sẽ được kích hoạt cho từng sự kiện làm đối số thứ hai hoặc lỗi làm đối số đầu tiên. Xem [myContract.getPastEvents](caver.contract.md#getpastevents) để biết chi tiết về cấu trúc sự kiện. |

Đối tượng tùy chọn có thể chứa các mục sau:

| Tên    | Loại     | Mô tả                                                                                                                                                                              |
| ------ | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bộ lọc | đối tượng | (tùy chọn) Cho phép bạn lọc các sự kiện theo các tham số được lập chỉ mục, _vd:_ `{filter: {mynumber: [12,13]}}` có nghĩa là tất cả các sự kiện trong đó "mynumber" là 12 hoặc 13. |
| chủ đề | Mảng      | (tùy chọn) Điều này cho phép bạn đặt chủ đề cho bộ lọc sự kiện theo cách thủ công. Nếu được cung cấp thuộc tính bộ lọc và chữ ký sự kiện, `chủ đề[0]` sẽ không được đặt tự động.   |

**Giá trị Trả về**

`Promise` trả về `đối tượng` - Đối tượng sự kiện. Để biết thêm chi tiết về đối tượng sự kiện, vui lòng tham khảo [myContract.getPastEvents](caver.contract.md#getpastevents).

**Ví dụ**

```javascript
> const subscription = myContract.subscribe('eventName', {
    filter: {myIndexedParam: [20,23], myOtherIndexedParam: '0x123456789...'}, // Using an array means OR: e.g. 20 or 23
  }, function(error, event) { console.log(event) })
{
    returnValues: {
        myIndexedParam: 20,
        myOtherIndexedParam: '0x123456789...',
        myNonIndexParam: 'My string'
    },
    raw: {
        data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        topics: ['0xfd43a...', '0x7f9fa...']
    },
    event: 'eventName',
    signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    logIndex: 0,
    transactionIndex: 0,
    transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
    blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    blocknumber: 1234,
    address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
}
> subscription.unsubscribe() // unsubscribe the event
```

## myContract.events <a href="#mycontract-events" id="mycontract-events"></a>

```javascript
myContract.events.eventName([options][, callback])
```

Đăng ký một sự kiện.

**Tham số**

| Tên      | Loại     | Mô tả                                                                                                           |
| -------- | --------- | --------------------------------------------------------------------------------------------------------------- |
| tùy chọn | đối tượng | (tùy chọn) Các tùy chọn được sử dụng để đăng ký. Xem bảng dưới đây để biết chi tiết.                            |
| gọi lại  | hàm       | (tùy chọn) Lệnh gọi lại này sẽ được kích hoạt cho từng sự kiện làm đối số thứ hai hoặc lỗi làm đối số thứ nhất. |

Đối tượng tùy chọn có thể chứa các mục sau:

| Tên       | Loại     | Mô tả                                                                                                                                                                              |
| --------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bộ lọc    | đối tượng | (tùy chọn) Cho phép bạn lọc các sự kiện theo các tham số được lập chỉ mục, _vd:_ `{filter: {mynumber: [12,13]}}` có nghĩa là tất cả các sự kiện trong đó "mynumber" là 12 hoặc 13. |
| fromBlock | số        | (tùy chọn) Số khối bắt đầu các sự kiện.                                                                                                                                            |
| chủ đề    | Mảng      | (tùy chọn) Điều này cho phép bạn đặt chủ đề cho bộ lọc sự kiện theo cách thủ công. Nếu được cung cấp thuộc tính bộ lọc và chữ ký sự kiện, `chủ đề[0]` sẽ không được đặt tự động.   |

**Return Value**

`EventEmitter`: The event emitter has the following events:

| Name      | Type   | Description                                                                               |
| --------- | ------ | ----------------------------------------------------------------------------------------- |
| data      | object | Fires on each incoming event with the event object as an argument.                        |
| connected | string | Fires once after the subscription successfully connected. It returns the subscription ID. |
| error     | object | Fires when an error in the subscription occurs.                                           |

**NOTE** `connected` is available with caver-js [v1.5.7](https://www.npmjs.com/package/caver-js/v/1.5.7).

The structure of the returned event `object` looks as follows:

| Name             | Type           | Description                                                                                                                               |
| ---------------- | -------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| event            | string         | The event name.                                                                                                                           |
| signature        | string \      | `null` | The event signature, `null` if it is an anonymous event.                                                                         |
| address          | string         | Address which from this event originated.                                                                                                 |
| returnValues     | object         | The return values coming from the event, _e.g._, `{myVar: 1, myVar2: '0x234...'}`.                                                        |
| logIndex         | number         | Integer of the event index position in the block.                                                                                         |
| transactionIndex | number         | Integer of the transaction's index position where the event was created.                                                                  |
| transactionHash  | 32-byte string | Hash of the transaction this event was created in. `null` when it is still pending.                                                       |
| blockHash        | 32-byte string | Hash of the block this event was created in. `null` when it is still pending.                                                             |
| blocknumber      | number         | The block number this log was created in. `null` when still pending.                                                                      |
| raw.data         | string         | The data containing non-indexed log parameter.                                                                                            |
| raw.topics       | Array          | An array with a maximum of four 32-byte topics, and topic 1-3 contains indexed parameters of the event.                                   |
| id               | string         | A log identifier. It is made through concatenating "log\_" string with `keccak256(blockHash + transactionHash + logIndex).substr(0, 8)` |

**Example**

```javascript
> myContract.events.eventName({
    filter: {myIndexedParam: [20,23], myOtherIndexedParam: '0x123456789...'}, // Using an array means OR: e.g. 20 or 23
    fromBlock: 0
  }, function(error, event) { console.log(event) })
  .on('connected', function(subscriptionId){
      console.log(subscriptionId)
  })
  .on('data', function(event){
      console.log(event) // same results as the optional callback above
  })
  .on('error', console.error)

// event output example
{
    returnValues: {
        myIndexedParam: 20,
        myOtherIndexedParam: '0x123456789...',
        myNonIndexParam: 'My string'
    },
    raw: {
        data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
    },
    event: 'eventName',
    signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    logIndex: 0,
    transactionIndex: 0,
    transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
    blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    blocknumber: 1234,
    address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe',
    id: 'log_41d221bc',
}
```

## events.allEvents <a href="#events-allevents" id="events-allevents"></a>

```javascript
myContract.events.allEvents([options] [, callback])
```

Same as [myContract.events](caver.contract.md#mycontract-events) but receives all events from this smart contract. Optionally, the filter property can filter those events.

## getPastEvents <a href="#getpastevents" id="getpastevents"></a>

```javascript
myContract.getPastEvents(event [, options] [, callback])
```

Gets past events for this contract.

**Parameters**

| Name     | Type     | Description                                                                                                                   |
| -------- | -------- | ----------------------------------------------------------------------------------------------------------------------------- |
| event    | string   | The name of the event in the contract, or `"allEvents"` to get all events.                                                    |
| options  | object   | (optional) The options used for subscription. See the table below for the details.                                            |
| callback | function | (optional) This callback will be fired with an array of event logs as the second argument, or an error as the first argument. |

To options object can contain the following:

| Name      | Type   | Description                                                                                                                                                        |
| --------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| filter    | object | (optional) Lets you filter events by indexed parameters, _e.g._, `{filter: {mynumber: [12,13]}}` means all events where "mynumber" is 12 or 13.                    |
| fromBlock | number | (optional) The block number from which to get events.                                                                                                              |
| toBlock   | number | (optional) The block number to get events up to (defaults to `"latest"`).                                                                                          |
| topics    | Array  | (optional) This allows manually setting the topics for the event filter. Given the filter property and event signature, `topic[0]` would not be set automatically. |

**Return Value**

`Promise` returns `Array` - An array with the past event objects, matching the given event name and filter.

An event object can contain the following:

| Name             | Type      | Description                                                                                                                                                                                                   |
| ---------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| event            | string    | The event name.                                                                                                                                                                                               |
| signature        | string \ | `null` | The event signature, `null` if it’s an anonymous event.                                                                                                                                              |
| address          | string    | Address this event originated from.                                                                                                                                                                           |
| returnValues     | object    | The return values coming from the event, e.g. {myVar: 1, myVar2: '0x234...'}.                                                                                                                                 |
| logIndex         | number    | The event index position in the block.                                                                                                                                                                        |
| transactionIndex | number    | The transaction’s index position the event was created in.                                                                                                                                                    |
| transactionHash  | string    | The hash of the transaction this event was created in.                                                                                                                                                        |
| blockHash        | string    | The hash of the block this event was created in. null when it’s still pending.                                                                                                                                |
| blockNumber      | number    | The block number this log was created in. null when still pending.                                                                                                                                            |
| raw              | object    | An object defines `data` and `topic`. `raw.data` containing non-indexed log parameter. `raw.topic` is an array with a maximum of four 32 Byte topics, and topic 1-3 contains indexed parameters of the event. |

**Example**

```javascript
> myContract.getPastEvents('eventName', {
      filter: {myIndexedParam: [20,23], myOtherIndexedParam: '0x123456789...'}, // Using an array means OR: e.g. 20 or 23
      fromBlock: 0,
      toBlock: 'latest'
  }, function(error, events) { console.log(events) })
  .then(function(events) {
      console.log(events) // same results as the optional callback above
  })

[{
    returnValues: {
        myIndexedParam: 20,
        myOtherIndexedParam: '0x123456789...',
        myNonIndexParam: 'My string'
    },
    raw: {
        data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
    },
    event: 'eventName',
    signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    logIndex: 0,
    transactionIndex: 0,
    transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
    blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    blocknumber: 1234,
    address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
},{
      ...
}]
```
