---
title: <serviceDiscovery>
ms.date: 03/30/2017
ms.assetid: a3c68a4a-fc95-43c5-aacb-785936c0cf39
ms.openlocfilehash: 7ac067e84f2a4d2724e3d8f2d0af9b220fd15538
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399625"
---
# \<serviceDiscovery>
指定服務端點的探索能力。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceDiscovery>**  
  
## <a name="syntax"></a>語法  
  
```xml  
<behaviors>
  <serviceBehaviors>
    <behavior name="String">
      <serviceDiscovery>
        <announcementEndpoints>
          <endpoint name="String"
                    kind="Type" />
        </announcementEndpoints>
        <discoveryEndpoints>
          <endpoint name="String"
                    kind="Type" />
        </discoveryEndpoints>
      </serviceDiscovery>
    </behavior>
  </serviceBehaviors>
</behaviors>
```  
  
## <a name="attributes-and-elements"></a>屬性和項目  
 下列章節說明屬性、子元素和父元素。  
  
### <a name="attributes"></a>屬性  
 無。  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<announcementEndpoint>](announcementendpoint.md)|公告端點的集合。 使用此區段指定用於傳送公告訊息的端點。|  
|[\<discoveryEndpoint>](discoveryendpoint.md)|探索端點的集合。 使用此區段指定用於接聽探索訊息的端點。|  
  
### <a name="parent-elements"></a>父項目  
  
|元素|描述|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|指定行為項目。|  
  
## <a name="remarks"></a>備註  
 加入至服務的行為組態時，這個組態項目會將該服務的所有端點標示為可探索。 您可以使用 [\<discoveryEndpoint>](discoveryendpoint.md) 或子項目，進一步設定這類端點的探索功能 [\<announcementEndpoint>](announcementendpoint.md) 。 使用 [\<announcementEndpoint>](announcementendpoint.md) 區段來設定通知，方法是指定要用來傳送服務宣告的端點設定（online/Hello 和 offline/再見）。 使用 [\<discoveryEndpoint>](discoveryendpoint.md) 區段來手動指定要接聽探索訊息的端點。  
  
## <a name="example"></a>範例  
 下列組態範例將 CalculatorService 指定為可探索，並且選擇性地指定要使用的公告端點。  
  
```xml  
<services>
  <service name="CalculatorService"
           behaviorConfiguration="CalculatorServiceBehavior">
    ...
  </service>
</services>
<behaviors>
  <serviceBehaviors>
    <behavior name="CalculatorServiceBehavior">
      <serviceDiscovery>
        <announcementEndpoints>
          <endpoint name="udpEndpoint"
                    kind="udpAnnouncementEndpoint" />
        </announcementEndpoints>
      </serviceDiscovery>
    </behavior>
  </serviceBehaviors>
</behaviors>
```  
  
## <a name="see-also"></a>另請參閱

- <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior>
