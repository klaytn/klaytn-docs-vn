## klay_getFilterChanges <a id="klay_getfilterchanges"></a>

Phương thức truy vấn thay đổi (poll) đối với bộ lọc, trả về một mảng các bản ghi phát sinh kể từ lần truy vấn thay đổi trước đó.

**Tham số**

| Tên      | Loại  | Mô tả                              |
| -------- | ----- | ---------------------------------- |
| SỐ LƯỢNG | chuỗi | Id bộ lọc (*ví dụ*: "0x16" // 22). |

**Giá trị trả về**

`Mảng` - Mảng các đối tượng bản ghi, hoặc mảng trống nếu không có thay đổi kể từ lần truy vấn thay đổi trước đó.
- Đối với các bộ lọc được tạo bằng [klay_newBlockFilter](#klay_newblockfilter), kết quả trả về là các hàm băm khối (DỮ LIỆU 32 byte), *ví dụ:*, `["0x3454645634534..."]`.
- Đối với các bộ lọc được tạo bằng [klay_newPendingTransactionFilter](#klay_newpendingtransactionfilter), giá trị trả về là các mã băm của giao dịch (DỮ LIỆU 32 byte), *ví dụ*: `["0x6345343454645..."]`.
- Đối với các bộ lọc được tạo bằng [klay_newFilter](#klay_newfilter), các bản ghi là các đối tượng có tham số như sau:

| Tên              | Loại           | Mô tả                                                                                                                                                                                                                                              |
| ---------------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| đã xoá           | THẺ             | `true` nếu bản ghi đã bị xóa do tổ chức lại chuỗi. `false` nếu đó là bản ghi hợp lệ.                                                                                                                                                               |
| logIndex         | SỐ LƯỢNG        | Số nguyên vị trí chỉ mục bản ghi trong khối. `null` khi đó là bản ghi đang chờ xử lý.                                                                                                                                                              |
| transactionIndex | SỐ LƯỢNG        | Số nguyên bản ghi vị trí chỉ mục giao dịch đã được tạo từ đó. `null` nếu đang chờ xử lý.                                                                                                                                                           |
| transactionHash  | DỮ LIỆU 32 byte | Mã băm của giao dịch mà bản ghi này được tạo từ đó. `null` nếu đang chờ xử lý.                                                                                                                                                                     |
| blockHash        | DỮ LIỆU 32 byte | Mã băm của khối chứa bản ghi này. `null` nếu đang chờ xử lý.                                                                                                                                                                                       |
| blockNumber      | SỐ LƯỢNG        | Số khối chứa bản ghi này. `null` nếu đang chờ xử lý.                                                                                                                                                                                               |
| địa chỉ          | DỮ LIỆU 20 byte | Địa chỉ khởi tạo bản ghi này.                                                                                                                                                                                                                      |
| dữ liệu          | DỮ LIỆU         | Chứa các đối số không được lập chỉ mục của bản ghi.                                                                                                                                                                                                |
| chủ đề           | Mảng DỮ LIỆU    | Mảng gồm 0 đến 4 DỮ LIỆU 32 byte của các đối số được lập chỉ mục của bản ghi. (Trong Solidity: Chủ đề đầu tiên là mã băm chữ ký của sự kiện (*ví dụ*: `Deposit(address,bytes32,uint256)`), trừ khi bạn khai báo sự kiện với từ khóa `anonymous`.). |

**Ví dụ**

```shell
// Yêu cầu
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_getFilterChanges","params":["0x16"],"id":73}' https://public-en-baobab.klaytn.net

// Kết quả
{
    "id":1,
    "jsonrpc":"2.0",
    "result": [{
    "logIndex": "0x1", // 1
    "blockNumber":"0x1b4", // 436
    "blockHash": "0x8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "transactionHash":  "0xdf829c5a142f1fccd7d8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcf",
    "transactionIndex": "0x0", // 0
    "address": "0x16c5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "data":"0x0000000000000000000000000000000000000000000000000000000000000000",
    "topics": ["0x59ebeb90bc63057b6515673c3ecf9438e5058bca0f92585014eced636878c9a5"]
    },{
        ...
    }]
}
```


## klay_getFilterLogs <a id="klay_getfilterlogs"></a>

Trả về một mảng gồm tất cả các bản ghi khớp với bộ lọc bằng id đã cho, có được bằng cách sử dụng [klay_newFilter](#klay_newfilter).  Lưu ý rằng các id bộ lọc được trả về bằng các hàm tạo bộ lọc khác, chẳng hạn như [klay_newBlockFilter](#klay_newblockfilter) hoặc [klay_newPendingTransactionFilter](#klay_newpendingtransactionfilter), không thể sử dụng được với hàm này.

Việc thực thi API này có thể bị giới hạn bởi hai cấu hình nút để quản lý một cách an toàn tài nguyên của nút Klaytn.
- Số lượng kết quả trả về tối đa trong một truy vấn (Mặc định: 10.000).
- Giới hạn thời gian thực thi của một truy vấn (Mặc định: 10 giây).

**Tham số**

| Tên      | Loại  | Mô tả     |
| -------- | ----- | --------- |
| SỐ LƯỢNG | chuỗi | Id bộ lọc |

**Return Value**

See [klay_getFilterChanges](#klay_getfilterchanges)

**Example**

```shell
// Request
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_getFilterLogs","params":["0xd32fd16b6906e67f6e2b65dcf48fc272"],"id":1}' https://public-en-baobab.klaytn.net

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":[{
      "address":"0x87ac99835e67168d4f9a40580f8f5c33550ba88b",
      "topics":["0xd596fdad182d29130ce218f4c1590c4b5ede105bee36690727baa6592bd2bfc8"],
      "data":"0x0000000000000000000000000000000000000000000000000000000000000064000000000000000000000000000000000000000000000000000000000000007b",
      "blockNumber":"0x54",
      "transactionHash":"0xcd4703cd62bd930d4652999bce8dcb75b7ade49d922fa42dc11e568c52a5fa6f",
      "transactionIndex":"0x0",
      "blockHash":"0x9a49f30f1d1876ff3913bd0aa58f328822e7a369cb13e0640b82234f26e781bb",
      "logIndex":"0x0",
      "removed":false
  }]
}
```


## klay_getLogs <a id="klay_getlogs"></a>

Returns an array of all logs matching a given filter object.

The execution of this API can be limited by two node configurations to manage resources of Klaytn node safely.
- The number of maximum returned results in a single query (Default: 10,000).
- The execution duration limit of a single query (Default: 10 seconds).

**Parameters**

`Object` - The filter options:

| Name      | Type                      | Description                                                                                                                                                                                                                                                                                                      |
| --------- | ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| fromBlock | QUANTITY &#124; TAG       | (optional, default: `"latest"`) Integer or hexadecimal block number, or the string `"earliest"`, `"latest"` or `"pending"` as in the [default block parameter](block.md#the-default-block-parameter).                                                                                                            |
| toBlock   | QUANTITY &#124; TAG       | (optional, default: `"latest"`) Integer or hexadecimal block number, or the string `"earliest"`, `"latest"` or `"pending"` as in the [default block parameter](block.md#the-default-block-parameter).                                                                                                            |
| address   | 20-byte DATA &#124; Array | (optional) Contract address or a list of addresses from which logs should originate.                                                                                                                                                                                                                             |
| topics    | Array of DATA             | (optional) Array of 32-byte DATA topics. Topics are order-dependent. Each topic can also be an array of DATA with “or” options.                                                                                                                                                                                  |
| blockHash | 32-byte DATA              | (optional) A filter option that restricts the logs returned to the single block with the 32-byte hash blockHash. Using blockHash is equivalent to fromBlock = toBlock = the block number with hash blockHash. If blockHash is present in in the filter criteria, then neither fromBlock nor toBlock are allowed. |

{% hint style="success" %}
NOTE: In versions earlier than Klaytn v1.7.0, only integer block number, the string `"earliest"` and `"latest"` are available.
{% endhint %}

**Return Value**

See [klay_getFilterChanges](#klay_getfilterchanges)

**Examples**

```shell
// Request
$ curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_getLogs","params":[{"fromBlock":"0x1","toBlock":"latest","address":"0x87ac99835e67168d4f9a40580f8f5c33550ba88b"}],"id":1}' https://public-en-baobab.klaytn.net

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":[
    {
      "address":"0x87ac99835e67168d4f9a40580f8f5c33550ba88b",
      "topics":["0xfa9b2165fc71c1d6ffa03291c7f5d223ea363ec063d747eec9ce2d30d24855ef"],
      "data":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000060000000000000000000000000d3564e57bb5c6f4d983a493a946534f8e1e8b481000000000000000000000000000000000000000000000000000000000000001341646472657373426f6f6b436f6e747261637400000000000000000000000000",
      "blockNumber":"0xd3b5",
      "transactionHash":"0x57ca8ff0a0d454d4c5418694c21bc4ef3de26cf7cd18dd404d6a7189a826bfe0",
      "transactionIndex":"0x0",
      "blockHash":"0x279251a907c6ab1fb723595511ff401432e7c2437d54189298f53a7d33ce3a60",
      "logIndex":"0x0",
      "removed":false
    },
    {
      "address":"0x87ac99835e67168d4f9a40580f8f5c33550ba88b",
      "topics":["0xfa3e1e272694072320aad73a3fadd8876c4bf8f40899c6c7ce2fda9f4e652cfa"],
      "data":"0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000003000000000000000000000000000000000000000000000000000000000000000300000000000000000000000041383b6ee0ea5108d6b139165a9c85351aacd39800000000000000000000000057f7439898e652fa9b5654022297588532e5e0370000000000000000000000005b5b7a718a4124eb746ae00b1ce6edcaa5ab55bc",
      "blockNumber":"0xd3b5",
      "transactionHash":"0x57ca8ff0a0d454d4c5418694c21bc4ef3de26cf7cd18dd404d6a7189a826bfe0",
      "transactionIndex":"0x0",
      "blockHash":"0x279251a907c6ab1fb723595511ff401432e7c2437d54189298f53a7d33ce3a60",
      "logIndex":"0x1",
      "removed":false
    },
    {
      "address":"0x87ac99835e67168d4f9a40580f8f5c33550ba88b",
      "topics":["0xc7b359b1e189b7d721be7f0765a8d745be718566b8e67cbd2728dae5d6fd64b6"],
      "data":"0x000000000000000000000000d3564e57bb5c6f4d983a493a946534f8e1e8b481000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000003000000000000000000000000286d09b578d6126e09296dfe6c775ea7d0cf06e9000000000000000000000000860350f6d774efd16046335c388b832b910d3f8c00000000000000000000000061a7cbdd597848494fa85cbb76f9c63ad9c06cad",
      "blockNumber":"0x14d96",
      "transactionHash":"0x73282602d2f908180f47e3c8673f41c0899cbbb2d606976c2f77188ffa57d6e7",
      "transactionIndex":"0x0",
      "blockHash":"0xa5268a093cd5df7eccde18217a7019a35ab761088312027af16682aafa704ee3",
      "logIndex":"0x1",
      "removed":false
    },
    {
      "address":"0x87ac99835e67168d4f9a40580f8f5c33550ba88b",
      "topics":["0xc7b359b1e189b7d721be7f0765a8d745be718566b8e67cbd2728dae5d6fd64b6"],
      "data":"0x000000000000000000000000d3564e57bb5c6f4d983a493a946534f8e1e8b4810000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000030000000000000000000000002f91d1b79dd06da1b622122d61e05e64562de61e0000000000000000000000006e76e0ce76dfba55060400144318d4821a58510600000000000000000000000031b93ca83b5ad17582e886c400667c6f698b8ccd",
      "blockNumber":"0x14e4e",
      "transactionHash":"0xf9d86ed451d67abc68c517f7fa0e0a7a8e3dedec23f56febda2b7f52d35185b6",
      "transactionIndex":"0x0",
      "blockHash":"0x7ddf4a0a203d40afc1706aa24b787da601e1bce326319349d0eeef6c41656fa5",
      "logIndex":"0x1",
      "removed":false
    }
  ]
}
```

```shell
// Request
$ curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_getLogs","params":[{"fromBlock":"earliest","toBlock":"latest","topics":["0xc7b359b1e189b7d721be7f0765a8d745be718566b8e67cbd2728dae5d6fd64b6"]}],"id":2}' https://public-en-baobab.klaytn.net

// Result
{
  "jsonrpc":"2.0",
  "id":2,
  "result":[
    {
      "address":"0x87ac99835e67168d4f9a40580f8f5c33550ba88b",
      "topics":["0xc7b359b1e189b7d721be7f0765a8d745be718566b8e67cbd2728dae5d6fd64b6"],
      "data":"0x000000000000000000000000d3564e57bb5c6f4d983a493a946534f8e1e8b481000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000003000000000000000000000000286d09b578d6126e09296dfe6c775ea7d0cf06e9000000000000000000000000860350f6d774efd16046335c388b832b910d3f8c00000000000000000000000061a7cbdd597848494fa85cbb76f9c63ad9c06cad",
      "blockNumber":"0x14d96",
      "transactionHash":"0x73282602d2f908180f47e3c8673f41c0899cbbb2d606976c2f77188ffa57d6e7",
      "transactionIndex":"0x0",
      "blockHash":"0xa5268a093cd5df7eccde18217a7019a35ab761088312027af16682aafa704ee3",
      "logIndex":"0x1",
      "removed":false
    },
    {
      "address":"0x87ac99835e67168d4f9a40580f8f5c33550ba88b",
      "topics":["0xc7b359b1e189b7d721be7f0765a8d745be718566b8e67cbd2728dae5d6fd64b6"],
      "data":"0x000000000000000000000000d3564e57bb5c6f4d983a493a946534f8e1e8b4810000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000030000000000000000000000002f91d1b79dd06da1b622122d61e05e64562de61e0000000000000000000000006e76e0ce76dfba55060400144318d4821a58510600000000000000000000000031b93ca83b5ad17582e886c400667c6f698b8ccd",
      "blockNumber":"0x14e4e",
      "transactionHash":"0xf9d86ed451d67abc68c517f7fa0e0a7a8e3dedec23f56febda2b7f52d35185b6",
      "transactionIndex":"0x0",
      "blockHash":"0x7ddf4a0a203d40afc1706aa24b787da601e1bce326319349d0eeef6c41656fa5",
      "logIndex":"0x1",
      "removed":false
    },
    {
      "address":"0x87ac99835e67168d4f9a40580f8f5c33550ba88b",
      "topics":["0xc7b359b1e189b7d721be7f0765a8d745be718566b8e67cbd2728dae5d6fd64b6"],
      "data":"0x000000000000000000000000d3564e57bb5c6f4d983a493a946534f8e1e8b481000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000003000000000000000000000000a2b1264624c92257dd8e7f0cac42d451061d1510000000000000000000000000b381ee81e319e5ec48f42d0b47b5e4361c9a6f740000000000000000000000003855407fa65c4c5104648b3a9e495072df62b585",
      "blockNumber":"0x14f38",
      "transactionHash":"0xc8f8c637ea9fcbe71e23fe0779b59fb10173e8c4fd7e49bce3cce76ff67d353d",
      "transactionIndex":"0x0",
      "blockHash":"0xb1717038e443f517bd7a8c37b66fb731fed573f5fa5486ebbbb5e4c9060be50b",
      "logIndex":"0x1",
      "removed":false
    },
    {
      "address":"0x87ac99835e67168d4f9a40580f8f5c33550ba88b",
      "topics":["0xc7b359b1e189b7d721be7f0765a8d745be718566b8e67cbd2728dae5d6fd64b6"],
      "data":"0x000000000000000000000000d3564e57bb5c6f4d983a493a946534f8e1e8b4810000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000030000000000000000000000009dd579f23912665b956b0cd50387b29a62052732000000000000000000000000c98a86af2eca2989c0cb2a2b8d4bb841f11e94ab000000000000000000000000f65e07b6626ab43ecea744803fa46bd4a89bfdb6",
      "blockNumber":"0x14fe7",
      "transactionHash":"0x14da1883bb2aae487ce1cb93cd39bc9bb802adbba083f337051877358150ab3f",
      "transactionIndex":"0x0",
      "blockHash":"0xcd820189f00e9a6faaea7313437b92114e69bd32e18b4a28e7763117716c6fa9",
      "logIndex":"0x1",
      "removed":false
    }
  ]
}
```


## klay_newBlockFilter <a id="klay_newblockfilter"></a>

Creates a filter in the node, to notify when a new block arrives. To check if the state has changed, call [klay_getFilterChanges](#klay_getfilterchanges).

**Parameters**

None

**Return Value**

| Type     | Description |
| -------- | ----------- |
| QUANTITY | Id bộ lọc.  |

**Ví dụ**

```shell
// Yêu cầu
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_newBlockFilter","params":[],"id":73}' https://public-en-baobab.klaytn.net

// Kết quả
{
  "jsonrpc":"2.0",
  "id":73,
  "result":"0xc2f2e8168a7e38b5d979d0f7084130ee"
}
```


## klay_newFilter <a id="klay_newfilter"></a>

Tạo một đối tượng bộ lọc dựa trên các tùy chọn bộ lọc để thông báo khi trạng thái thay đổi (bản ghi).
- Để kiểm tra thay đổi trạng thái, hãy gọi [klay_getFilterChanges](#klay_getfilterchanges).
- Để có được tất cả các bản ghi khớp với bộ lọc được tạo bởi `klay_newFilter`, hãy gọi [klay_getFilterLogs](#klay_getfilterlogs).

**Lưu ý về việc xác định bộ lọc chủ đề:** Các chủ đề phụ thuộc vào thứ tự. Một giao dịch với bản ghi có các chủ đề `[A, B]` sẽ được khớp bởi các bộ lọc chủ đề như sau:
* `[]` "bất cứ thứ gì"
* `[A]` "A ở vị trí đầu tiên (và bất cứ thứ gì sau đó)"
* `[null, B]` "bất cứ thứ gì ở vị trí đầu tiên VÀ B ở vị trí thứ hai (và bất cứ thứ gì sau đó)"
* `[A, B]` "A ở vị trí đầu tiên VÀ B ở vị trí thứ hai (và bất cứ thứ gì sau đó)"
* `[[A, B], [A, B]]` "(A HOẶC B) ở vị trí đầu tiên VÀ (A HOẶC B) ở vị trí thứ hai (và bất cứ thứ gì sau đó)"

**Tham số**

`Object` - Các tùy chọn bộ lọc:

| Tên       | Loại                       | Mô tả                                                                                                                                                                                               |
| --------- | --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| fromBlock | SỐ LƯỢNG &#124; THẺ         | (tùy chọn, mặc định: `"latest"`) Số khối số nguyên hoặc thập lục phân hoặc chuỗi `"earliest"`, `"latest"` hoặc `"pending"` như trong [tham số khối mặc định](block.md#the-default-block-parameter). |
| toBlock   | SỐ LƯỢNG &#124; THẺ         | (tùy chọn, mặc định: `"latest"`) Số khối số nguyên hoặc thập lục phân hoặc chuỗi `"earliest"`, `"latest"` hoặc `"pending"` như trong [tham số khối mặc định](block.md#the-default-block-parameter). |
| địa chỉ   | DỮ LIỆU 20 byte &#124; Mảng | (tùy chọn) Địa chỉ hợp đồng hoặc danh sách các địa chỉ khởi tạo bản ghi.                                                                                                                            |
| chủ đề    | Mảng DỮ LIỆU                | (tùy chọn) Mảng các chủ đề DỮ LIỆU 32 byte. Các chủ đề phụ thuộc vào thứ tự. Mỗi chủ đề cũng có thể là một mảng DỮ LIỆU với các tùy chọn "hoặc".                                                    |

{% hint style="success" %}
LƯU Ý: Trong các phiên bản trước phiên bản Klaytn v1.7.0, chỉ có số khối số nguyên, chuỗi `"earliest"` và `"latest"` khả dụng.
{% endhint %}

**Giá trị trả về**

| Loại     | Mô tả     |
| -------- | --------- |
| SỐ LƯỢNG | Id bộ lọc |

**Ví dụ**

```shell
// Yêu cầu
$ curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_newFilter","params":[{"fromBlock":"earliest","toBlock":"latest","address":"0x87ac99835e67168d4f9a40580f8f5c33550ba88b","topics":["0xd596fdad182d29130ce218f4c1590c4b5ede105bee36690727baa6592bd2bfc8"]}],"id":1}' https://public-en-baobab.klaytn.net

// Kết quả
{"jsonrpc":"2.0","id":1,"result":"0xd32fd16b6906e67f6e2b65dcf48fc272"}
```


## klay_newPendingTransactionFilter <a id="klay_newpendingtransactionfilter"></a>

Tạo một bộ lọc trong nút để thông báo khi có giao dịch mới đang chờ xử lý. Để kiểm tra thay đổi trạng thái, hãy gọi [klay_getFilterChanges](#klay_getfilterchanges).

**Tham số**

Không có

**Giá trị trả về**

| Loại    | Mô tả      |
| -------- | ---------- |
| SỐ LƯỢNG | Id bộ lọc. |

**Ví dụ**

```shell
// Yêu cầu
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_newPendingTransactionFilter","params":[],"id":73}' https://public-en-baobab.klaytn.net

// Kết quả
{
  "jsonrpc":"2.0",
  "id":73,
  "result":"0x90cec22a723fcc725fb2462733c2880f"
}
```

## klay_subscribe <a id="klay_subscribe"></a>

Tạo đăng ký mới cho các sự kiện cụ thể bằng cách sử dụng RPC Pub/Sub thông qua WebSocket hoặc bộ lọc thông qua HTTP. Tính năng này cho phép khách hàng đợi các sự kiện thay vì phải truy vấn thay đổi.

Nút sẽ trả về id đăng ký cho mỗi đăng ký được tạo. Đối với mỗi sự kiện khớp với đăng ký, thông báo chứa dữ liệu liên quan sẽ được gửi cùng với id đăng ký. Nếu một kết nối bị đóng lại, tất cả các đăng ký được tạo qua kết nối đó sẽ bị xóa.

**Tham số**

`Object` - Loại thông báo: `"newHeads"` hoặc `"logs"`.


`"newHeads"` thông báo cho bạn khi mỗi khối được thêm vào chuỗi khối. `"logs"` thông báo cho bạn khi các bản ghi được đưa vào các khối mới. Loại thông báo này yêu cầu phải có tham số thứ hai chỉ định tùy chọn bộ lọc. Để biết thêm thông tin, vui lòng truy cập [klay_newFilter > tham số](https://docs.klaytn.com/dapp/json-rpc/api-references/klay/filter#klay_newfilter).

**Giá trị trả về**

| Loại    | Mô tả                                                                                                                |
| -------- | -------------------------------------------------------------------------------------------------------------------- |
| SỐ LƯỢNG | Id đăng ký khi tạo đăng ký. Đối với mỗi sự kiện khớp với đăng ký, thông báo chứa dữ liệu liên quan cũng sẽ được gửi. |


**Ví dụ**

API này phù hợp để sử dụng cùng với công cụ Websocket, [`wscat`](https://www.npmjs.com/package/wscat).

```shell
// Yêu cầu
wscat -c http://localhost:8552
> {"jsonrpc":"2.0", "id": 1, "method": "klay_subscribe", "params": ["newHeads"]}

// Kết quả
< {"jsonrpc":"2.0","id":1,"result":"0x48bb6cb35d6ccab6eb2b4799f794c312"}
< {"jsonrpc":"2.0","method":"klay_subscription","params":{"subscription":"0x48bb6cb35d6ccab6eb2b4799f794c312","result":{"parentHash":"0xc39755b6ac01d1e8c58b1088e416204f7af5b6b66bfb4f474523292acbaa7d57","reward":"0x2b2a7a1d29a203f60e0a964fc64231265a49cd97","stateRoot":"0x12aa1d3ab0440d844c28fbc6f89d26082f39a8435b512fa487ff55c2056aceb3","number":"0x303bea4”, ... ... }}}
```

```shell
// Yêu cầu
wscat -c http://localhost:8552
> {"jsonrpc":"2.0", "id": 1, "method": "klay_subscribe", "params": ["logs", {"fromBlock":"earliest","toBlock":"latest","address":"0x87ac99835e67168d4f9a40580f8f5c33550ba88b","topics":["0xd596fdad182d29130ce218f4c1590c4b5ede105bee36690727baa6592bd2bfc8"]}]}

// Kết quả
< {"jsonrpc":"2.0","id":1,"result":"0xbdab16c8e4ae1b9e6930c78359de3e0e"}
< {"jsonrpc":"2.0","method":"klay_subscription","params":{"subscription":"0xbdab16c8e4ae1b9e6930c78359de3e0e","result":{"address":"0x2e4bb340e26caffb4073d7f1151f37d17524cdbc","topics":["0xb1a7310b1a46c788fcf30784cad70442d5232acaef480b0c094c76bee8d9c77d"],"data":"0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000d2588fe96a34c56a5d0a484cb603bc16fc5cdbbc","blockNumber":"0x3041201","transactionHash":"0xdacdebc77006fc566f65448524a0bc770056d8c7a05244bc7bfb2123b1bd398c","transactionIndex":"0x0","blockHash":"0x899b2dbfe96a34ce5d965dbcfcf39d072b4ce1097d479923e6b6355f3e2609ec","logIndex":"0x0","removed":false}}}
```


## klay_uninstallFilter <a id="klay_uninstallfilter"></a>

Gỡ cài đặt bộ lọc với id đã cho. Nên luôn được gọi khi không còn cần theo dõi. Ngoài ra, bộ lọc hết thời gian chờ nếu không được yêu cầu [klay_getFilterChanges](#klay_getfilterchanges) trong một khoảng thời gian.

**Tham số**

| Tên    | Loại     | Mô tả      |
| ------ | -------- | ---------- |
| bộ lọc | SỐ LƯỢNG | Id bộ lọc. |

**Giá trị trả về**

| Loại   | Mô tả                                                             |
| ------- | ----------------------------------------------------------------- |
| Boolean | `true` nếu gỡ cài đặt bộ lọc thành công, nếu không sẽ là `false`. |

**Ví dụ**

```shell
// Yêu cầu
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_uninstallFilter","params":["0xb"],"id":73}' https://public-en-baobab.klaytn.net

// Kết quả
{
  "jsonrpc": "2.0",
  "id":1,
  "result": true
}
```


## klay_unsubscribe <a id="klay_unsubscribe"></a>

Hủy đăng ký với id đăng ký cụ thể bằng cách sử dụng RPC Pub/Sub thông qua WebSocket hoặc bộ lọc thông qua HTTP. Chỉ có kết nối đã tạo đăng ký mới có thể hủy đăng ký.

**Tham số**

| Loại    | Mô tả       |
| -------- | ----------- |
| SỐ LƯỢNG | Id đăng ký. |

**Giá trị trả về**

| Loại    | Mô tả                                                       |
| ------- | ----------------------------------------------------------- |
| Boolean | `true` nếu hủy đăng ký thành công, nếu không sẽ là `false`. |


**Ví dụ**

API này phù hợp để sử dụng cùng với công cụ Websocket, [`wscat`](https://www.npmjs.com/package/wscat).

```shell
// Yêu cầu
> {"jsonrpc":"2.0", "id": 1, "method": "klay_unsubscribe", "params": ["0xab8ac7a4045025d0c2807d63060eea6d"]}

// Kết quả
< {"jsonrpc":"2.0","id":1,"result":true}
```
