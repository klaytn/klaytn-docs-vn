---
description: >-
  Các API liên quan đến Cơ chế Quản trị Klaytn.
---

# Cơ chế Quản trị Namespace <a id="namespace-governance"></a>

Để quản trị mạng, Klaytn cung cấp các API sau dưới namespace `quản trị`.

Có ba chế độ quản trị khác nhau tại Klaytn.
* `Không có`: Tất cả các nút tham gia mạng đều có quyền thay đổi cấu hình.
* `duy nhất`: Chỉ một nút được chỉ định có quyền thay đổi cấu hình.
* `bỏ phiếu`: Tất cả các nút có quyền biểu quyết đều có thể bỏ phiếu cho một sự thay đổi. Khi tổng số quyền biểu quyết quá bán, một cuộc bỏ phiếu sẽ được thông qua.

Dựa trên chế độ quản trị, người đề xuất có thể bỏ phiếu về các tham số mạng như đơn giá, số tiền đặt cược tối thiểu, v. v. Để trở thành người đề xuất, các nút ứng cử viên cần nạp một lượng KLAY tối thiểu. Tất cả các nút hợp cách có thể đề xuất một khối nhưng cơ hội sẽ phụ thuộc vào số tiền đặt cược.

Khi tính toán tỷ lệ đặt cược để xác định số lượng slot (số lượng cơ hội) để trở thành người đề xuất trong một khoảng thời gian nhất định, Một nút có thể không được phân bổ bất kỳ slot nào do làm tròn số. Tuy nhiên, một nút hợp cách đã nạp một lượng KLAY tối thiểu sẽ luôn được đảm bảo một slot.

Nghĩa là, nếu một nút không hợp cách - nút này không có đủ số lượng KLAY - thì sẽ không có cơ hội đề xuất cũng như xác thực một khối.

**Cảnh báo**
- Một nút quản trị luôn hợp cách ở chế độ `duy nhất` như một ngoại lệ.
- Một cuộc bỏ phiếu sẽ được thực hiện khi một khối được đề xuất. Cuộc bỏ phiếu này được áp dụng sau hai giai đoạn bao gồm cả giai đoạn mà khối được đề xuất. Như một ngoại lệ, chỉ addValidator/removeValidator được áp dụng ngay lập tức.
## governance_vote <a id="governance_vote"></a>

Phương thức `bỏ phiếu` gửi một phiếu bầu mới. Nếu nút có quyền bỏ phiếu dựa trên chế độ quản trị thì có thể đặt phiếu bầu. Nếu không, một thông báo lỗi sẽ được trả về và phiếu bầu sẽ bị bỏ qua.

**Tham số**

- `Khóa` : Tên của cài đặt cấu hình sẽ được thay đổi. Khóa có dạng `domain.field`
- `Giá trị` : Các loại giá trị khác nhau cho mỗi khóa.

| Khóa                                | Mô tả                                                                                                                                                                                                                                                                                                                           |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `"governance.governancemode"`       | `CHUỖI`. Một trong ba chế độ quản trị. `"không có"`, `"duy nhấy"`, `"bỏ phiếu"`                                                                                                                                                                                                                                                 |
| `"governance.governingnode"`        | `ĐỊA CHỈ`. Địa chỉ của nút quản trị được chỉ định. Nó chỉ hoạt động khi chế độ quản trị là `"duy nhất"` ví dụ:`"0xe733cb4d279da696f30d470f8c04decb54fcb0d2"`                                                                                                                                                                    |
| `"governance.unitprice"`            | `SỐ`. Giá đơn vị gas. e.g., `25000000000`                                                                                                                                                                                                                                                                                       |
| `"governance.addvalidator"`         | `ĐỊA CHỈ`. Địa chỉ của một ứng cử viên nút xác thực mới. vd: `0xe733cb4d279da696f30d470f8c04decb54fcb0d2`                                                                                                                                                                                                                       |
| `"governance.removevalidator"`      | `ĐỊA CHỈ`. Địa chỉ của nút xác thực hiện tại cần được xóa. vd: `0xe733cb4d279da696f30d470f8c04decb54fcb0d2`                                                                                                                                                                                                                     |
| `"governance.deriveshaimpl"`        | `SỐ`. Chính sách tạo mã băm giao dịch và mã băm biên lai trong tiêu đề khối. Xem [tại đây](https://github.com/klaytn/klaytn/blob/v1.10.0/blockchain/types/derive_sha.go#L34) để biết các tùy chọn khả dụng. vd: `2` (DeriveShaConcat)                                                                                           |
| `"governance.govparamcontract"`     | `ĐỊA CHỈ`. Địa chỉ của hợp đồng GovParam. vd: `0xe733cb4d279da696f30d470f8c04decb54fcb0d2`                                                                                                                                                                                                                                      |
| `"istanbul.epoch"`                  | `SỐ`. Khoảng thời gian trong đó các phiếu bầu được thu thập theo khối. Khi khoảng thời gian kết thúc, tất cả các phiếu bầu chưa được thông qua sẽ bị xóa. vd: `86400`                                                                                                                                                           |
| `"istanbul.committeesize"`          | `SỐ`. Số lượng nút xác thực trong một ủy ban.(`sub` trong cấu hình chuỗi), ví dụ: `7`                                                                                                                                                                                                                                           |
| `"reward.mintingamount"`            | `CHUỖI`. Số lượng Peb được đúc khi một khối được tạo. Giá trị phải ở trong dấu ngoặc kép. vd: `"9600000000000000000"`                                                                                                                                                                                                           |
| `"reward.ratio"`                    | `CHUỖI`. Tỷ lệ phân phối cho CN/KGF/KIR được phân tách bằng `"/"`. Tổng của tất cả các giá trị phải bằng `100`. vd: `"50/40/10"` nghĩa là CN 50%, KGF 40%, KIR 10%                                                                                                                                                              |
| `"reward.kip82ratio"`               | `CHUỖI`. Tỷ lệ phân phối của người đề xuất khối cho người đặt cược được phân tách bằng `"/"`. Tổng của tất cả các giá trị phải bằng `"100"`. Xem [KIP-82](https://github.com/klaytn/kips/blob/master/KIPs/kip-82.md) để biết thêm chi tiết. vd: `"20/80"` có nghĩa là người đề xuất nhận 20% trong khi người đặt cược nhận 80%. |
| `"reward.useginicoeff"`             | `BOOL`. Sử dụng hệ số Gini hoặc không. `đúng`, `sai`                                                                                                                                                                                                                                                                            |
| `"reward.deferredtxfee"`            | `BOOL`. Cách đưa ra phí giao dịch cho người đề xuất. Nếu đúng, điều đó có nghĩa là phí tx sẽ được tổng hợp bằng phần thưởng khối và được phân phối cho người đề xuất, KIR và KGF. Nếu sai, tất cả phí tx sẽ được trao cho người đề xuất. `đúng`, `sai`                                                                          |
| `"reward.minimumstake"`             | `CHUỖI`. Lượng Klay cần thiết để trở thành CN (Nút đồng thuận). Giá trị phải ở trong dấu ngoặc kép. e.g., `"5000000"`                                                                                                                                                                                                           |
| `"kip71.lowerboundbasefee"`         | `SỐ`. Phí cơ sở thấp nhất cho phép. Xem [KIP-71](https://github.com/klaytn/kips/blob/main/KIPs/kip-71.md) để biết thêm chi tiết. vd: `25000000000`                                                                                                                                                                              |
| `"kip71.upperboundbasefee"`         | `SỐ`. Phí cơ sở cao nhất cho phép. vd: `750000000000`                                                                                                                                                                                                                                                                           |
| `"kip71.gastarget"`                 | `SỐ`. Gas khối mà phí cơ sở muốn đạt được. Phí cơ sở tăng khi khối cha chứa nhiều hơn mục tiêu gas và giảm khi khối cha chứa ít hơn mục tiêu gas. vd: `30000000`                                                                                                                                                                |
| `"kip71.basefeedenominator"`        | `SỐ`. Kiểm soát tốc độ thay đổi phí cơ sở. vd: `20`                                                                                                                                                                                                                                                                             |
| `"kip71.maxblockgasusedforbasefee"` | `SỐ`. Gas khối tối đa nắm được trong tính toán phí cơ sở. vd: `60000000`                                                                                                                                                                                                                                                        |


**Giá trị Trả về**

| Loại  | Mô tả                 |
| ----- | --------------------- |
| Chuỗi | Kết quả gửi phiếu bầu |

**Ví dụ**

```javascript
> governance.vote ("governance.governancemode", "ballot")
"Phiếu bầu của bạn đã được đặt thành công."

> governance.vote ("governance.governingnode", "0x12345678990123456789901234567899012345678990")
"Phiếu bầu của bạn đã được đặt thành công."

> governance.vote("istanbul.epoch", 604800)
"Phiếu bầu của bạn đã được đặt thành công."

> governance.vote("governance.unitprice", 25000000000)
"Phiếu bầu của bạn đã được đặt thành công."

> governance.vote("istanbul.committeesize", 7)
"Phiếu bầu của bạn đã được đặt thành công."

> governance.vote("reward.mintingamount", "9600000000000000000")
"Phiếu bầu của bạn đã được đặt thành công."

> governance.vote("reward.ratio", "40/30/30")
"Phiếu bầu của bạn đã được đặt thành công."

> governance.vote("reward.useginicoeff", false)
"Phiếu bầu của bạn đã được đặt thành công."

// Nếu nhập sai dữ liệu
> governance.vote("reward.ratio", 100)
"Không thể đặt phiếu bầu của bạn. Vui lòng kiểm tra khóa và giá trị phiếu bầu của bạn"

> governance.vote("governance.governingnode", 1234)
"Không thể đặt phiếu bầu của bạn. Vui lòng kiểm tra khóa và giá trị phiếu bầu của bạn"

// when `governancemode` is "single" and the node is not `governingnode`
> governance.vote("governance.governancemode", "ballot")
"Bạn không có quyền bỏ phiếu"
```


## governance_showTally <a id="governance_showtally"></a>

Thuộc tính `showTally` cung cấp số phiếu bầu quản trị hiện tại. Nó hiển thị tỷ lệ phê duyệt tổng hợp theo tỷ lệ phần trăm. Khi vượt quá 50%, một cuộc bỏ phiếu sẽ được thông qua.

**Tham số**

Không có

**Giá trị Trả về**

| Loại | Mô tả                                                             |
| ----- | ----------------------------------------------------------------- |
| Tally | Giá trị của mỗi phiếu bầu và tỷ lệ tán thành theo tỷ lệ phần trăm |

**Ví dụ**

```javascript
> governance.showTally
[{
    ApprovalPercentage: 36.2,
    Key: "unitprice",
    Value: 25000000000
}, {
    ApprovalPercentage: 72.5,
    Key: "mintingamount",
    Value: "9600000000000000000"
}]
```


## governance_totalVotingPower <a id="governance_totalvotingpower"></a>

Thuộc tính `totalVotingPower` cung cấp tổng của tất cả quyền biểu quyết mà CN có. Mỗi CN có 1.0 ~ 2.0 quyền biểu quyết. In `"none"`, `"single"` governance mode, `totalVotingPower` don't provide any information.

**Parameters**

None

**Return Value**

| Type  | Description                         |
| ----- | ----------------------------------- |
| Float | Total Voting Power or error message |

**Example**

```javascript
// In "ballot" governance mode
> governance.totalVotingPower
32.452

// In "none", "single" governance mode
> governance.totalVotingPower
"In current governance mode, voting power is not available"
```


## governance_myVotingPower <a id="governance_myvotingpower"></a>

The `myVotingPower` property provides the voting power of the node. The voting power can be 1.0 ~ 2.0. In `"none"`, `"single"` governance mode, `totalVotingPower` don't provide any information.

**Parameters**

None

**Return Value**

| Type  | Description                          |
| ----- | ------------------------------------ |
| Float | Node's Voting Power or error message |

**Example**

```javascript
// In "ballot" governance mode
> governance.myVotingPower
1.323

// In "none", "single" governance mode
> governance.myVotingPower
"In current governance mode, voting power is not available"
```


## governance_myVotes <a id="governance_myvotes"></a>

The `myVotes` property provides my vote information in the epoch. Each vote is stored in a block when the user's node generates a new block. After current epoch ends, this information is cleared.

**Parameters**

None

**Return Value**

| Type      | Description                                                                                                                                                                                                      |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Vote List | Node's Voting status in the epoch<br>- `BlockNum`: The block number that this vote is stored<br>- `Casted`: If this vote is stored in a block or not<br>- `Key/Value`: The content of the vote |

**Example**

```javascript
> governance.vote("governance.governancemode", "ballot")
"Your vote was successfully placed."

> governance.myVotes
[{
    BlockNum: 403,
    Casted: true,
    Key: "governance.governancemode",
    Value: "ballot"
}]

```

## governance_getChainConfig <a id="governance_getchainconfig"></a>

The `getChainConfig` returns the chain configuration at a specific block. If the parameter is not set, it returns the chain configuration at the latest block.

**Tham số**

| Loại                | Mô tả                                                                                                                                                                        |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SỐ LƯỢNG &#124; THẺ | Số nguyên hoặc khối thập lục phân hoặc chuỗi `"cũ nhất"`, `"mới nhất"` hoặc `"đang chờ xử lý"` như trong [tham số khối mặc định](klay/block.md#the-default-block-parameter). |

{% hint style="success" %}
LƯU Ý: Số khối có thể lớn hơn số khối mới nhất, trong trường hợp đó API sẽ trả về giá trị dự kiến ​​dựa trên trạng thái chuỗi hiện tại. Các tham số quản trị trong tương lai có thể thay đổi thông qua các phiếu bầu quản trị bổ sung hoặc các thay đổi trạng thái hợp đồng GovParam.
{% endhint %}

**Return Value**

| Type | Description                                   |
| ---- | --------------------------------------------- |
| JSON | Chain configuration at the given block number |

**Example**

```javascript
> governance.getChainConfig()
{
  chainId: 1001,
  deriveShaImpl: 0,
  ethTxTypeCompatibleBlock: 86513895,
  governance: {
    govParamContract: "0x84214cec245d752a9f2faf355b59ddf7f58a6edb",
    governanceMode: "single",
    governingNode: "0x99fb17d324fa0e07f23b49d09028ac0919414db6",
    kip71: {
      basefeedenominator: 20,
      gastarget: 30000000,
      lowerboundbasefee: 25000000000,
      maxblockgasusedforbasefee: 60000000,
      upperboundbasefee: 750000000000
    },
    reward: {
      deferredTxFee: true,
      kip82ratio: "20/80",
      minimumStake: 5000000,
      mintingAmount: 6400000000000000000,
      proposerUpdateInterval: 3600,
      ratio: "50/40/10",
      stakingUpdateInterval: 86400,
      useGiniCoeff: true
    }
  },
  istanbul: {
    epoch: 604800,
    policy: 2,
    sub: 22
  },
  istanbulCompatibleBlock: 75373312,
  koreCompatibleBlock: 111736800,
  londonCompatibleBlock: 80295291,
  magmaCompatibleBlock: 98347376,
  unitPrice: 250000000000
}
```

## governance_chainConfig <a id="governance_chainconfig"></a>

The `chainConfig` property provides the latest chain configuration. This is equivalent to `chainConfigAt()` with an empty parameter.

{% hint style="warning" %}
`governance_chainConfig` Không được dùng API kể từ Klaytn v1.11 (Xem [klaytn#1783](https://github.com/klaytn/klaytn/pull/1783)). Thay vào đó, hãy sử dụng <a href="#governance_getchainconfig">`governance_getChainConfig`</a>.

LƯU Ý: Không được dùng API RPC kể từ v1.11. Tuy nhiên, thuộc tính `governance.chainConfig` trong bảng điều khiển Klaytn JavaScript đã bị xóa kể từ Klaytn v1.10.2.
{% endhint %}

{% hint style="success" %}
LƯU Ý: Trong các phiên bản trước Klaytn v1.10.0, API này trả về cấu hình chuỗi ban đầu. Tuy nhiên, do tên dễ gây nhầm lẫn nên nó được cập nhật kể từ phiên bản Klaytn v1.10.0. Để truy vấn cấu hình chuỗi ban đầu, hãy sử dụng `chainConfigAt(0)` thay thế.
{% endhint %}

**Tham số**

Không có

**Giá trị Trả về**

| Loại | Mô tả                   |
| ----- | ----------------------- |
| JSON  | Cấu hình chuỗi hiện tại |

**Ví dụ**

```javascript
> governance.chainConfig
{
  chainId: 1001,
  deriveShaImpl: 2,
  governance: {
    govParamContract: "0x0000000000000000000000000000000000000000",
    governanceMode: "ballot",
    governingNode: "0xe733cb4d279da696f30d470f8c04decb54fcb0d2",
    kip71: {
      basefeedenominator: 20,
      gastarget: 30000000,
      lowerboundbasefee: 25000000000,
      maxblockgasusedforbasefee: 60000000,
      upperboundbasefee: 750000000000
    },
    reward: {
      deferredTxFee: true,
      kip82ratio: "20/80",
      minimumStake: 5000000,
      mintingAmount: 6400000000000000000,
      proposerUpdateInterval: 3600,
      ratio: "50/40/10",
      stakingUpdateInterval: 20,
      useGiniCoeff: false
    }
  },
  istanbul: {
    epoch: 20,
    policy: 2,
    sub: 1
  },
  istanbulCompatibleBlock: 0,
  koreCompatibleBlock: 0,
  londonCompatibleBlock: 0,
  magmaCompatibleBlock: 0,
  unitPrice: 25000000000
}
```

## governance_chainConfigAt <a id="governance_chainconfigat"></a>

`chainConfigAt` trả về cấu hình chuỗi tại một khối cụ thể. Nếu tham số không được đặt, nó sẽ trả về cấu hình chuỗi tại khối mới nhất.

{% hint style="warning" %}
`governance_chainConfigAt` Không được dùng API kể từ Klaytn v1.11 (xem [klaytn#1783](https://github.com/klaytn/klaytn/pull/1783)). Thay vào đó, hãy sử dụng <a href="#governance_getchainconfig">`governance_getChainConfig`</a>.
{% endhint %}

**Tham số**

| Loại               | Mô tả                                                                                                                                                                        |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SỐ LƯỢNG &#124; THẺ | Số nguyên hoặc khối thập lục phân hoặc chuỗi `"cũ nhất"`, `"mới nhất"` hoặc `"đang chờ xử lý"` như trong [tham số khối mặc định](klay/block.md#the-default-block-parameter). |

{% hint style="success" %}
LƯU Ý: Số khối có thể lớn hơn số khối mới nhất, trong trường hợp đó API sẽ trả về giá trị dự kiến ​​dựa trên trạng thái chuỗi hiện tại. Các tham số quản trị trong tương lai có thể thay đổi thông qua các phiếu bầu quản trị bổ sung hoặc các thay đổi trạng thái hợp đồng GovParam.
{% endhint %}

**Giá trị Trả về**

| Loại | Mô tả                             |
| ----- | --------------------------------- |
| JSON  | Cấu hình chuỗi tại số khối đã cho |

**Ví dụ**

```javascript
> governance.chainConfigAt()
{
  chainId: 1001,
  deriveShaImpl: 2,
  governance: {
    govParamContract: "0x0000000000000000000000000000000000000000",
    governanceMode: "ballot",
    governingNode: "0xe733cb4d279da696f30d470f8c04decb54fcb0d2",
    kip71: {
      basefeedenominator: 20,
      gastarget: 30000000,
      lowerboundbasefee: 25000000000,
      maxblockgasusedforbasefee: 60000000,
      upperboundbasefee: 750000000000
    },
    reward: {
      deferredTxFee: true,
      kip82ratio: "20/80",
      minimumStake: 5000000,
      mintingAmount: 6400000000000000000,
      proposerUpdateInterval: 3600,
      ratio: "50/40/10",
      stakingUpdateInterval: 20,
      useGiniCoeff: false
    }
  },
  istanbul: {
    epoch: 20,
    policy: 2,
    sub: 1
  },
  istanbulCompatibleBlock: 0,
  koreCompatibleBlock: 0,
  londonCompatibleBlock: 0,
  magmaCompatibleBlock: 0,
  unitPrice: 25000000000
}
```

## governance_nodeAddress <a id="governance_nodeaddress"></a>

Thuộc tính `nodeAddress` cung cấp địa chỉ của nút mà người dùng đang sử dụng. Nó được lấy từ nodekey và được sử dụng để ký các tin nhắn đồng thuận. Và giá trị của `"governingnode"` phải là một trong những địa chỉ nút của nút xác thực.

**Tham số**

Không có

**Giá trị Trả về**

| Loại   | Mô tả                       |
| ------- | --------------------------- |
| ĐỊA CHỈ | 20 địa chỉ BYTE của một nút |

**Ví dụ**

```javascript
> governance.nodeAddress
"0xe733cb4d279da696f30d470f8c04decb54fcb0d2"
```

## governance_getParams <a id="governance_getparams"></a>

`getParams` trả về các tham số quản trị tại một khối cụ thể.

**Tham số**

| Loại                | Mô tả                                                                                                                                                                        |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SỐ LƯỢNG &#124; THẺ | Số nguyên hoặc khối thập lục phân hoặc chuỗi `"cũ nhất"`, `"mới nhất"` hoặc `"đang chờ xử lý"` như trong [tham số khối mặc định](klay/block.md#the-default-block-parameter). |

{% hint style="success" %}
LƯU Ý: Số khối có thể lớn hơn số khối mới nhất, trong trường hợp đó API sẽ trả về giá trị dự kiến ​​dựa trên trạng thái chuỗi hiện tại. Các tham số quản trị trong tương lai có thể thay đổi thông qua các phiếu bầu quản trị bổ sung hoặc các thay đổi trạng thái hợp đồng GovParam.
{% endhint %}

**Giá trị Trả về**

| Loại | Mô tả            |
| ----- | ---------------- |
| JSON  | tham số quản trị |

**Ví dụ**

```javascript
> governance.getParams(89)
{
  governance.deriveshaimpl: 2,
  governance.governancemode: "single",
  governance.governingnode: "0x99fb17d324fa0e07f23b49d09028ac0919414db6",
  governance.govparamcontract: "0x0000000000000000000000000000000000000000",
  governance.unitprice: 25000000000,
  istanbul.committeesize: 22,
  istanbul.epoch: 604800,
  istanbul.policy: 2,
  kip71.basefeedenominator: 20,
  kip71.gastarget: 30000000,
  kip71.lowerboundbasefee: 25000000000,
  kip71.maxblockgasusedforbasefee: 60000000,
  kip71.upperboundbasefee: 750000000000,
  reward.deferredtxfee: true,
  reward.kip82ratio: "20/80",
  reward.minimumstake: "5000000",
  reward.mintingamount: "9600000000000000000",
  reward.proposerupdateinterval: 3600,
  reward.ratio: "34/54/12",
  reward.stakingupdateinterval: 86400,
  reward.useginicoeff: true
}
```

## governance_itemsAt <a id="governance_itemsat"></a>

`itemsAt` trả về các tham số quản trị tại một khối cụ thể.

{% hint style="warning" %}
`governance_itemsAt` Không được dùng API kể từ Klaytn v1.11 (xem [klaytn#1783](https://github.com/klaytn/klaytn/pull/1783)). Thay vào đó, hãy sử dụng <a href="#governance_getparams">`governance_getParams`</a>.
{% endhint %}

**Tham số**

| Loại               | Mô tả                                                                                                                                                                        |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SỐ LƯỢNG &#124; THẺ | Số nguyên hoặc khối thập lục phân hoặc chuỗi `"cũ nhất"`, `"mới nhất"` hoặc `"đang chờ xử lý"` như trong [tham số khối mặc định](klay/block.md#the-default-block-parameter). |

{% hint style="success" %}
LƯU Ý: Trong các phiên bản trước phiên bản Klaytn v1.7.0, chỉ có số khối số nguyên, chuỗi `"cũ nhất"` và `"mới nhất"` khả dụng.
{% endhint %}

{% hint style="success" %}
LƯU Ý: Số khối có thể lớn hơn số khối mới nhất, trong trường hợp đó API sẽ trả về giá trị dự kiến ​​dựa trên trạng thái chuỗi hiện tại. Các tham số quản trị trong tương lai có thể thay đổi thông qua các phiếu bầu quản trị bổ sung hoặc các thay đổi trạng thái hợp đồng GovParam.
{% endhint %}

**Giá trị Trả về**

| Loại | Mô tả        |
| ----- | ------------ |
| JSON  | mục quản trị |

**Ví dụ**

```javascript
> governance.itemsAt(89)
{
  governance.deriveshaimpl: 2,
  governance.governancemode: "single",
  governance.governingnode: "0x7bf29f69b3a120dae17bca6cf344cf23f2daf208",
  governance.govparamcontract: "0x0000000000000000000000000000000000000000",
  governance.unitprice: 25000000000,
  istanbul.committeesize: 13,
  istanbul.epoch: 30,
  istanbul.policy: 2,
  kip71.basefeedenominator: 20,
  kip71.gastarget: 30000000,
  kip71.lowerboundbasefee: 25000000000,
  kip71.maxblockgasusedforbasefee: 60000000,
  kip71.upperboundbasefee: 750000000000,
  reward.deferredtxfee: true,
  reward.kip82ratio: "20/80",
  reward.minimumstake: "5000000",
  reward.mintingamount: "9600000000000000000",
  reward.proposerupdateinterval: 30,
  reward.ratio: "34/54/12",
  reward.stakingupdateinterval: 60,
  reward.useginicoeff: true
}
```

## governance_pendingChanges <a id="governance_pendingchanges"></a>

`pendingChanges` trả về danh sách các mục đã nhận đủ số phiếu nhưng chưa hoàn tất. Vào cuối giai đoạn hiện tại, những thay đổi này sẽ được hoàn tất và kết quả sẽ có hiệu lực từ giai đoạn này đến giai đoạn tiếp theo.

**Tham số**

Không có

**Giá trị Trả về**

| Loại              | Mô tả                                                        |
| ------------------ | ------------------------------------------------------------ |
| Danh sách Bỏ phiếu | Các thay đổi hiện đang chờ xử lý bao gồm các khóa và giá trị |

**Ví dụ**
```javascript
> governance.pendingChanges
{
  reward.minimumstake: "5000000",
  reward.useginicoeff: false
}
```

## governance_votes <a id="governance_votes"></a>

`phiếu bầu` trả về phiếu bầu từ tất cả các nút trong một giai đoạn. Những phiếu bầu này được thu thập từ tiêu đề của mỗi khối.

**Tham số**

Không có

**Giá trị Trả về**

| Loại              | Mô tả                                                       |
| ------------------ | ----------------------------------------------------------- |
| Danh sách Bỏ phiếu | Phiếu bầu hiện tại bao gồm các khóa, giá trị và địa chỉ nút |

**Ví dụ**
```javascript
> governance.votes
[{
    key: "reward.minimumstake",
    validator: "0xe733cb4d279da696f30d470f8c04decb54fcb0d2",
    value: "5000000"
}, {
    key: "reward.useginicoeff",
    validator: "0xa5bccb4d279419abe2d470f8c04dec0789ac2d54",
    value: false
}]
```

## governance_idxCache <a id="governance_idxcache"></a>
Thuộc tính `idxCache` trả về một mảng idxCache hiện tại trong bộ nhớ đệm. idxCache chứa số khối nơi thay đổi quản trị diễn ra. Theo mặc định, bộ đệm có thể có tối đa 1000 số khối trong bộ nhớ.

**Tham số**

Không có

**Giá trị Trả về**

| Loại       | Mô tả                                 |
| ----------- | ------------------------------------- |
| mảng uint64 | Số khối nơi thay đổi quản trị diễn ra |

**Ví dụ**
```javascript
> governance.idxCache
[0, 30]
```

## governance_idxCacheFromDb <a id="governance_idxcachefromdb"></a>
`idxCacheFromDb` trả về một mảng chứa tất cả các số khối đã từng có thay đổi quản trị. Kết quả của `idxCacheFromDb` giống hoặc dài hơn kết quả của `idxCache`

**Tham số**

Không có

**Giá trị Trả về**

| Loại       | Mô tả                                        |
| ----------- | -------------------------------------------- |
| mảng uint64 | Tất cả số khối nơi thay đổi quản trị diễn ra |

**Ví dụ**
```javascript
> governance.idxCacheFromDb
[0, 30]
```

## governance_itemCacheFromDb <a id="governance_itemcachefromdb"></a>
`itemCacheFromDb` trả về thông tin quản trị được lưu trữ trong khối đã cho. Nếu không có thay đổi nào được lưu trữ trong khối đã cho, hàm sẽ trả về `null`.

**Tham số**

| Loại  | Description                                                      |
| ------ | ---------------------------------------------------------------- |
| uint64 | A block number to query the governance change made in the block. |

**Return Value**

| Type | Description                                    |
| ---- | ---------------------------------------------- |
| JSON | Stored governance information at a given block |

**Example**
```javascript
> governance.itemCacheFromDb(0)
{
  governance.governancemode: "single",
  governance.governingnode: "0xe733cb4d279da696f30d470f8c04decb54fcb0d2",
  governance.unitprice: 25000000000,
  istanbul.committeesize: 1,
  istanbul.epoch: 30,
  istanbul.policy: 2,
  reward.deferredtxfee: true,
  reward.minimumstake: "5000000",
  reward.mintingamount: "6400000000000000000",
  reward.proposerupdateinterval: 3600,
  reward.ratio: "50/40/10",
  reward.stakingupdateinterval: 20,
  reward.useginicoeff: false
}
```
## governance_getStakingInfo <a id="governance_getstakinginfo"></a>

The `getStakingInfo` returns staking information at a specific block. The result includes the following information.
- `BlockNum`: The block number at which the staking information is given.
- `CouncilNodeAddrs`: The addresses of the consensus node.
- `CouncilRewardAddrs`: The addresses to which the block reward of the associated nodes is sent.
- `CouncilStakingAddrs`: The contract addresses in which the associated nodes deploy for staking.
- `CouncilStakingAmounts`: The amount of KLAY which the associated nodes stake.
- `Gini`: Gini coefficient.
- `KIRAddr`: The contract address of KIR.
- `PoCAddr`: The contract address of KGF. PoC is the previous name of KGF.
- `UseGini`: The boolean value whether or not the Gini coefficient is used.

Note that the order of all addresses and the staking amounts are matched.

**Parameters**

| Type                | Description                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| QUANTITY &#124; TAG | Integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](./klay/block.md#the-default-block-parameter). |

**Return Value**

| Type | Description         |
| ---- | ------------------- |
| JSON | Staking information |

**Example**

```javascript
> governance.getStakingInfo("latest")
{
  BlockNum: 57801600,
  CouncilNodeAddrs: ["0x99fb17d324fa0e07f23b49d09028ac0919414db6", "0x571e53df607be97431a5bbefca1dffe5aef56f4d", "0xb74ff9dea397fe9e231df545eb53fe2adf776cb2", "0x5cb1a7dccbd0dc446e3640898ede8820368554c8", "0x776817c0ef3d06d794cf01ae9afa33d7397b9b40", "0xc180ca565b34b5b63877674f5fe647e7da079022", "0x03497f51c31fe8b402df0bde90fd5a85f87aa943"],
  CouncilRewardAddrs: ["0xb2bd3178affccd9f9f5189457f1cad7d17a01c9d", "0x6559a7b6248b342bc11fbcdf9343212bbc347edc", "0x82829a60c6eac4e3e9d6ed00891c69e88537fd4d", "0xa86fd667c6a340c53cc5d796ba84dbe1f29cb2f7", "0x6e22cbe2b8bbd1df9f1d3c8ebae6d7ff5414a734", "0x24e593fb29731e54905025c230727dc28d229f77", "0x2b2a7a1d29a203f60e0a964fc64231265a49cd97"],
  CouncilStakingAddrs: ["0x12fa1ab4c3e17c1c08c1b5a945c864c8e8bf707e", "0xfd56604f1a20268ff7a0eab2ab48e25ee1e0f653", "0x1e0f6aaa9baa6081dc4910a854eebf8854c262ab", "0x5e6988415ebe0f6b088f5a676003ba60f572875a", "0xbb44998c2af35b8faee694cffe216558056d747e", "0x68cba498b7175cde9de08fc2e85ad3e9c8caefa8", "0x98efb31eeccafe35d53a6926e2a54c0858d9eebc"],
  CouncilStakingAmounts: [5000000, 5000000, 5000000, 5000000, 5000000, 5000000, 5000000],
  Gini: 0,
  KIRAddr: "0x716f89d9bc333286c79db4ebb05516897c8d208a",
  PoCAddr: "0x2bcf9d3e4a846015e7e3152a614c684de16f37c6",
  UseGini: true
}
```
