---
title: 在不同存放區上的 XSLT 轉換
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 369850e9-004a-45d2-b5c3-5060d9135adb
ms.openlocfilehash: 0eb98aad00227688df3e097ad674b44a29e5cb1a
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/02/2020
ms.locfileid: "84281709"
---
# <a name="xslt-transformations-over-different-stores"></a>在不同存放區上的 XSLT 轉換
> [!NOTE]
> 在 .NET Framework 2.0 中，<xref:System.Xml.Xsl.XslTransform> 類別已過時。 您可以使用 <xref:System.Xml.Xsl.XslCompiledTransform> 類別來執行可延伸樣式表語言轉換 (XSLT)。 如需詳細資訊，請參閱[使用 XslCompiledTransform 類別](using-the-xslcompiledtransform-class.md)和[從 XslTransform 類別移轉](migrating-from-the-xsltransform-class.md)。  
  
 .NET Framework 中 ADO.NET 和 XML 類別提供用來存取資料的統一程式設計模型。 資料同時被表示為 XML 資料 (以標記分隔的文字) 和關聯式資料 (由資料列和資料行組成的資料表)。 .NET Framework 中 XML 可將任何資料流的 XML 資料讀取至 XML 文件物件模型 (DOM) 節點樹狀結構；在此處，資料可透過程式設計方式加以存取，而 ADO.NET 則提供存取及操縱 <xref:System.Data.DataSet> 物件內關聯式資料的工具。  
  
 XML DOM 提供在 XML 文件中存取資料，和提供額外的類別，以在 XML 文件中讀取、寫入和巡覽。 <xref:System.Xml> 命名空間支援這些類別，並且將 XML DOM 與 ADO.NET 所提供的資料存取服務統一。 <xref:System.Xml.XmlDataDocument> 可提供資料的關聯式存取。 <xref:System.Xml.XmlDataDocument> 會將 XML 對應至 ADO.NET <xref:System.Data.DataSet> 中的關聯式資料。 任何以 .NET Framework 為基礎的應用程式都可使用 <xref:System.Xml> 命名空間中類別來存取及操縱 <xref:System.Xml.XmlDataDocument> 中 XML 文件與關聯式資料。 這項實作支援收集和分送資料的 N-Tier 架構。 如需詳細資訊，請參閱 [XML 與關聯式資料和 ADO.NET 互相整合](xml-integration-with-relational-data-and-adonet.md)。  
  
## <a name="see-also"></a>另請參閱

- [XslTransform 類別實作 XSLT 處理器](xsltransform-class-implements-the-xslt-processor.md)
