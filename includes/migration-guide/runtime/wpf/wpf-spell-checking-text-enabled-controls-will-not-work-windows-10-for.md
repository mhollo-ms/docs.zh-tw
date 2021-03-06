---
ms.openlocfilehash: 1d2e4a058008676c6ea85becebd4bb9220569ef3
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621161"
---
### <a name="wpf-spell-checking-in-text-enabled-controls-will-not-work-in-windows-10-for-languages-not-in-the-oss-input-language-list"></a>若語言不在 OS 的輸入語言清單中，啟用文字之控制項中的　WPF 拼字檢查不適用於 Windows 10

#### <a name="details"></a>詳細資料

因為只有輸入語言清單上的語言可以使用平台拼字檢查功能，所以拼字檢查工具在 Windows 10 上執行時可能不適用於啟用 WPF 文字的控制項。在 Windows 10 中，將語言新增至可用鍵盤的清單時，Windows 會自動下載並安裝對應的 Feature on Demand (FOD) 套件，以提供拼字檢查功能。 只要將語言加入輸入語言清單中，就可支援拼字檢查工具。

#### <a name="suggestion"></a>建議

請注意，要進行拼字檢查的語言或文字必須新增為輸入語言，拼字檢查才能在 Windows 10 中正常運作。

| 名稱    | 值       |
|:--------|:------------|
| 影響範圍   |Edge|
|版本|4.6|
|類型|執行階段|
