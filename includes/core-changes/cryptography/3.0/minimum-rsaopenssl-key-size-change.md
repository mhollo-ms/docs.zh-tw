---
ms.openlocfilehash: 3d94023fc508a56304587121c6cf1444c87b0d52
ms.sourcegitcommit: 0926684d8d34f4c6b5acce58d2193db093cb9cf2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/20/2020
ms.locfileid: "83721015"
---
### <a name="minimum-size-for-rsaopenssl-key-generation-has-increased"></a>RSAOpenSsl 金鑰產生的大小下限已增加

在 Linux 上產生新 RSA 金鑰的最小大小已從 384-位增加至512位。

#### <a name="change-description"></a>變更描述

從 .NET Core 3.0 開始，Linux 上、和的 RSA 實例上的屬性所報告的最低合法金鑰大小， `LegalKeySizes` <xref:System.Security.Cryptography.RSA.Create%2A?displayProperty=nameWithType> <xref:System.Security.Cryptography.RSAOpenSsl.%23ctor%2A> <xref:System.Security.Cryptography.RSACryptoServiceProvider.%23ctor%2A> 從384增加到512。

因此，在 .NET Core 2.2 和更早版本中，方法呼叫（例如）會 `RSA.Create(384)` 成功。 在 .NET Core 3.0 和更新版本中，方法呼叫會擲回例外狀況， `RSA.Create(384)` 指出大小太小。

此變更的原因是，OpenSSL 會在 Linux 上執行密碼編譯作業，並在版本1.0.2 和1.1.0 之間產生最小值。 .NET Core 3.0 傾向于將 OpenSSL 1.1. x 新增至 1.0. x，並產生最小的回報版本，以反映這項較高的相依性限制。

#### <a name="version-introduced"></a>引進的版本

3.0

#### <a name="recommended-action"></a>建議的動作

如果您呼叫任何受影響的 Api，請確定任何產生的金鑰大小不小於提供者的最小值。

> [!NOTE]
> 384位 RSA 已經被視為不安全（如同512位 RSA）。 新式建議（例如[NIST 特殊發行集800-57 第1部分修訂 4](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf)）會建議2048位做為新產生之金鑰的最小大小。

#### <a name="category"></a>類別

密碼編譯

#### <a name="affected-apis"></a>受影響的 API

- <xref:System.Security.Cryptography.AsymmetricAlgorithm.LegalKeySizes?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.RSA.Create%2A?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.RSAOpenSsl.%23ctor%2A>
- <xref:System.Security.Cryptography.RSACryptoServiceProvider.%23ctor%2A>

<!--
#### Affected APIs

- `P:System.Security.Cryptography.AsymmetricAlgorithm.LegalKeySizes`
- `Overload:System.Security.Cryptography.RSA.Create`
- `Overload:System.Security.Cryptography.RSAOpenSsl.#ctor`
- `Overload:System.Security.Cryptography.RSACryptoServiceProvider.#ctor`

-->
