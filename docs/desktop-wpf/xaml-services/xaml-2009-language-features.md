---
title: XAML 2009 語言功能
ms.date: 03/30/2017
helpviewer_keywords:
- XAML 2009 [XAML Services]
- XAML [XAML Services], XAML 2009
ms.assetid: f6bb18d8-c86a-4549-8862-323e6b32a8dd
ms.openlocfilehash: e58e6757b88958bf8a3547c8a272c2e6298dcecb
ms.sourcegitcommit: c2d9718996402993cf31541f11e95531bc68bad0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2020
ms.locfileid: "82071595"
---
# <a name="xaml-2009-language-features"></a>XAML 2009 語言功能
XAML 2009 是新 XAML 語言功能的縮寫詞彙，可擴充現有的 XAML 語言規格。 XAML 2009 引進了數個新的指示詞和建構。 其中包括[x:參數指令](xarguments-directive.md);[x:工廠方法指令](xfactorymethod-directive.md);[x:參考標記擴展](xreference-markup-extension.md);[x:類型參數指令](xtypearguments-directive.md);和通用語言基元(例如`x:Char`)的內置類型。

## <a name="xaml-2009-support-in-wpf-and-visual-studio"></a>WPF 和 Visual Studio 中的 XAML 2009 支援

在 WPF 中，您可以使用 XAML 2009 功能，但只能針對未編譯 WPF 標記的 XAML。 編譯標記的 XAML 和 BAML 形式的 XAML 目前不支援 XAML 2009 語言關鍵字和功能。

請注意，對於 CLR 類型和類型系統，在 WPF 中載入鬆散 XAML 的現有技術，可能也會有比編譯標記的 XAML 更嚴格的安全性和存取限制。 如需詳細資訊，請參閱 [安全性 (WPF)](../../framework/wpf/security-wpf.md) 或 [WPF 安全性策略 - 平台安全性](../../framework/wpf/wpf-security-strategy-platform-security.md)。

XAML 2009 也引進了額外的功能，可修改先前的 XAML 2006 建構或修改基本標記形式。

### <a name="xkey-as-an-object-element"></a>x:Key 做為物件項目

XAML 2009 可支援 `x:Key` 做為物件 (具有物件項目值的屬性 (Property) 項目)，但 XAML 2006 僅支援 `x:Key` 做為屬性 (Attribute)。 請參閱 [x:Key Directive](xkey-directive.md)的＜XAML 2009＞一節。

### <a name="xmlns-on-property-elements"></a>屬性項目上的 xmlns

XAML 2009 可支援屬性項目的 XAML 命名空間 (xmlns) 定義，但 XAML 2006 僅支援物件項目上的 xmlns 定義。

### <a name="event-attributes"></a>事件屬性

針對事件所支援的屬性，XAML 2006 會假定標記編譯與將事件送出到標記編譯有關。 XAML 2009 支援類似標記延伸的標記形式，這會將事件連接延遲到 XAML 的執行階段剖析和載入。 不過，WPF 應用程式和 WPF UI 的 XAML 情節通常不會使用這項功能。 WPF 及其 XAML 2006 實作會使用事件處理常式連接組合進行在 <xref:System.Windows.UIElement> 層級定義的路由事件，並使用其標記編譯器步驟進行大部分的事件屬性處理。 標記編譯器也會前置處理在 XAML 中找到的任何事件屬性，建置動作會在此 XAML 中宣告使用標記編譯器。

## <a name="see-also"></a>另請參閱

- [XAML 概觀 (WPF)](../fundamentals/xaml.md)