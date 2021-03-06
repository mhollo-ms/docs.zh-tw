---
title: <source> 項目
ms.date: 09/29/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/sources/source
- http://schemas.microsoft.com/.NetConfiguration/v2.0#source
helpviewer_keywords:
- <source> element
- source element
ms.openlocfilehash: 417722ce2f3865350158413307495e3ab435d386
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2020
ms.locfileid: "79153291"
---
# <a name="source-element"></a>\<source> 項目
指定起始追蹤訊息的追蹤來源。  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.diagnostics>**](system-diagnostics-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<sources>**](sources-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<source>**

## <a name="syntax"></a>語法  
  
```xml  
<source>
  <listeners>...</listeners>  
</source>  
```  
  
## <a name="attributes-and-elements"></a>屬性和項目  
 下列章節說明屬性、子元素和父元素。  
  
### <a name="attributes"></a>屬性  
  
|屬性|描述|  
|---------------|-----------------|  
|`name`|選擇性屬性。<br /><br /> 指定追蹤來源的名稱。|  
|`switchName`|選擇性屬性。<br /><br /> 指定應用程式中追蹤參數實例的名稱。 如果在專案中找不到參數 `<switches>` ，則值會指定參數的層級。|  
|`switchType`|選擇性屬性。<br /><br /> 指定追蹤參數的類型。 如果存在的話，類型必須是有效的類別名稱，而且不能是空字串。|  
|`extraAttribute`|選擇性屬性。<br /><br /> 針對該追蹤來源的方法所識別的追蹤來源特定屬性，指定其值 <xref:System.Diagnostics.TraceSource.GetSupportedAttributes%2A> 。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<listeners>](listeners-element-for-source.md)|包含收集、儲存及路由傳送訊息的接聽程式。|  
  
### <a name="parent-elements"></a>父項目  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|通用語言執行平台和 .NET Framework 應用程式所使用之每個組態檔中的根項目。|  
|`system.diagnostics`|指定用於收集、儲存及路由傳送訊息的追蹤接聽項，以及設定追蹤參數的層級。|  
|`sources`|包含起始追蹤訊息的追蹤來源。|  
  
## <a name="remarks"></a>備註  
 此元素可用於電腦設定檔（Machine.config）和應用程式佈建檔。  
  
## <a name="example"></a>範例  
 下列範例顯示如何使用專案 `<source>` 來新增追蹤來源 `mySource` ，以及設定名為之來源交換器的層級 `sourceSwitch` 。 隨即加入主控台追蹤接聽程式，將追蹤資訊寫入主控台。  
  
```xml  
<configuration>  
  <system.diagnostics>  
    <sources>  
      <source name="mySource" switchName="sourceSwitch" switchType="System.Diagnostics.SourceSwitch"  >  
        <listeners>  
          <add name="console" type="System.Diagnostics.ConsoleTraceListener" >  
            <filter type="System.Diagnostics.EventTypeFilter" initializeData="Error" />  
          </add>  
          <remove name="Default" />  
        </listeners>  
      </source>  
    </sources>  
        <switches>  
           <add name="sourceSwitch" value="Warning" />  
        </switches>
  </system.diagnostics>
</configuration>  
```  
  
## <a name="see-also"></a>另請參閱

- [追蹤和偵錯設定結構描述](index.md)
- [追蹤參數](../../../debug-trace-profile/trace-switches.md)
