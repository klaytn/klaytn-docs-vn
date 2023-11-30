# Lịch sử nâng cấp căn bản của Klaytn

Trang này trình bày tất cả các lần hard fork cho chuỗi khối Klaytn.

## Shanghai EVM

| ``      | Baobab                        | Cypress                       |
| ------- | ----------------------------- | ----------------------------- |
| Ngày    | Aug 28, 2023 10:30:31 / UTC+9 | Oct 16, 2023 10:50:24 / UTC+9 |
| Số khối | `#131,608,000`                | `#135,456,000`                |

### Tóm tắt

Ethereum's Shanghai hard fork items were introduced with the [v1.11.1 release](https://github.com/klaytn/klaytn/releases/tag/v1.11.1) and [v1.11.0 release](https://github.com/klaytn/klaytn/releases/tag/v1.11.0), which includes changes from [EIP-3651](https://eips.ethereum.org/EIPS/eip-3651), [EIP-3855](https://eips.ethereum.org/EIPS/eip-3855), and [EIP-3860](https://eips.ethereum.org/EIPS/eip-3860)

## KIP-103

| ``      | Baobab                      | Cypress                     |
| ------- | --------------------------- | --------------------------- |
| Ngày    | 06/04/2023 04:25:03 / UTC+9 | 17/04/2023 01:24:48 / UTC+9 |
| Số khối | `#119,145,600`              | `#119,750,400`              |

### Tóm tắt

Lần hard fork KIP-103 được giới thiệu cùng với [phiên bản v1.10.2](https://github.com/klaytn/klaytn/releases/tag/v1.10.2). Nó bao gồm việc triển khai [KIP-103](https://kips.klaytn.foundation/KIPs/kip-103), đây là tiêu chuẩn kỹ thuật của tính năng điều chỉnh lại ngân sách([KGP-6](https://govforum.klaytn.foundation/t/kgp-6-proposal-to-establish-a-sustainable-and-verifiable-klay-token-economy/157)).


### Điều chỉnh lại ngân sách

| ``                                 | Baobab                                     | Cypress                                    |
| ---------------------------------- | ------------------------------------------ | ------------------------------------------ |
| Địa chỉ hợp đồng TreasuryRebalance | 0xD5ad6D61Dd87EdabE2332607C328f5cc96aeCB95 | 0xD5ad6D61Dd87EdabE2332607C328f5cc96aeCB95 |
| Địa chỉ KCV                        | 0xaa8d19a5e17e9e1bA693f13aB0E079d274a7e51E | 0x4f04251064274252D27D4af55BC85b68B3adD992 |
| Địa chỉ KFF                        | 0x8B537f5BC7d176a94D7bF63BeFB81586EB3D1c0E | 0x85D82D811743b4B8F3c48F3e48A1664d1FfC2C10 |
| Địa chỉ KCF                        | 0x47E3DbB8c1602BdB0DAeeE89Ce59452c4746CA1C | 0xdd4C8d805fC110369D3B148a6692F283ffBDCcd3 |


## Kore
| ``      | Baobab                        | Cypress                       |
| ------- | ----------------------------- | ----------------------------- |
| Ngày    | Jan 10, 2023 10:20:50 / UTC+9 | Apr 17, 2023 01:24:48 / UTC+9 |
| Số khối | `#111,736,800`                | `#119,750,400`                |

### Tóm tắt

Lần hard fork Kore được giới thiệu cùng với [phiên bản v1.10.0](https://github.com/klaytn/klaytn/releases/tag/v1.10.0). Nó được triển khai theo phương pháp biểu quyết quản trị trên chuỗi ([KIP-81](https://kips.klaytn.foundation/KIPs/kip-81)), cấu trúc phần thưởng GC mới ([KIP-82](https://kips.klaytn.foundation/KIPs/kip-82)) và các thay đổi EVM.



## Magma
| ``      | Baobab                        | Cypress                       |
| ------- | ----------------------------- | ----------------------------- |
| Ngày    | Aug 08, 2022 11:01:20 / UTC+9 | Aug 29, 2022 11:51:00 / UTC+9 |
| Số khối | `#98,347,376`                 | `#99,841,497`                 |

### Tóm tắt

Lần hard fork Magma được giới thiệu cùng với [phiên bản v1.9.0](https://github.com/klaytn/klaytn/releases/tag/v1.9.0). Nó bao gồm cơ chế định giá phí gas động, [#1493](https://github.com/klaytn/klaytn/pull/1493)) và triển khai [KIP-71](https://kips.klaytn.foundation/KIPs/kip-71).

## EthTxType

| ``      | Baobab                        | Cypress                     |
| ------- | ----------------------------- | --------------------------- |
| Ngày    | Mar 27, 2022 23:56:31 / UTC+9 | 31/03/2022 12:14:39 / UTC+9 |
| Số khối | `#86,513,895`                 | `#86,816,005`               |

### Tóm tắt

Lần thay đổi EthTxType của Ethereum được giới thiệu cùng với [phiên bản v1.8.0](https://github.com/klaytn/klaytn/releases/tag/v1.8.0). Nó bao gồm các loại giao dịch mới để hỗ trợ các loại giao dịch của Ethereum: TxTypeEthereumAccessListand TxTypeEthereumDynamicFee ([#1142](https://github.com/klaytn/klaytn/pull/1142), [#1158](https://github.com/klaytn/klaytn/pull/1158)).

## London EVM

| ``      | Baobab                        | Cypress                       |
| ------- | ----------------------------- | ----------------------------- |
| Ngày    | Jan 14, 2022 11:02:55 / UTC+9 | Mar 31, 2022 12:14:39 / UTC+9 |
| Số khối | `#80,295,291`                 | `#86,816,005`                 |

### Tóm tắt

Các hạng mục hard fork của Ethereum London đã được giới thiệu cùng với [phiên bản v1.7.3](https://github.com/klaytn/klaytn/releases/tag/v1.7.3), bao gồm mã vận hành BaseFee EVM để đảm bảo tính tương thích với EVM Ethereum London ([#1065](https://github.com/klaytn/klaytn/pull/1065), [#1066](https://github.com/klaytn/klaytn/pull/1066), [#1096](https://github.com/klaytn/klaytn/pull/1096)).

## Istanbul EVM

| ``           | Baobab                        | Cypress                       |
| ------------ | ----------------------------- | ----------------------------- |
| Date         | Nov 17, 2021 23:42:13 / UTC+9 | Mar 31, 2022 12:14:39 / UTC+9 |
| Block number | `#75,373,312`                 | `#86,816,005`                 |

### Tóm tắt

Các hạng mục hard fork của Ethereum Istanbul ra mắt cùng với [phiên bản v1.7.0](https://github.com/klaytn/klaytn/releases/tag/v1.7.0), bao gồm các thay đổi từ [EIP-152](https://eips.ethereum.org/EIPS/eip-152), [EIP-1108](https://eips.ethereum.org/EIPS/eip-1108), [EIP-1344](https://eips.ethereum.org/EIPS/eip-1344), [EIP-1844](https://eips.ethereum.org/EIPS/eip-1844) và [EIP-2200](https://eips.ethereum.org/EIPS/eip-2200).