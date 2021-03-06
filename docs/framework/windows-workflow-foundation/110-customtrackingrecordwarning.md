---
title: 110 - CustomTrackingRecordWarning
ms.date: 03/30/2017
ms.assetid: 3bc093de-be47-4ed0-983f-05b4246446fc
ms.openlocfilehash: 230e889c677ee83b2e71b128413b7107ec11dc2e
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "61924133"
---
# <a name="110---customtrackingrecordwarning"></a>110 - CustomTrackingRecordWarning
## <a name="properties"></a>屬性  
  
|||  
|-|-|  
|ID|110|  
|關鍵字|UserEvents、EndToEndMonitoring、Troubleshooting、HealthMonitoring、WFTracking|  
|層級|警告|  
|通道|Microsoft-Windows-Application Server-Applications/Analytic|  
  
## <a name="description"></a>描述  
 此事件是當工作流程執行個體內的活動發出連同層級警告的 CustomTrackingRecord 事件時，由 ETW 追蹤參與者發出。  
  
## <a name="message"></a>訊息  
 TrackRecord = CustomTrackingRecord, InstanceID = %1, RecordNumber=%2, EventTime=%3, Name=%4, ActivityName=%5, ActivityId=%6, ActivityInstanceId=%7, ActivityTypeName=%8, Data=%9, Annotations=%10, ProfileName = %11  
  
## <a name="details"></a>詳細資料  
  
|資料項目名稱|資料項目型別|描述|  
|--------------------|--------------------|-----------------|  
|InstanceId|xs:GUID|工作流程的執行個體 ID。|  
|RecordNumber|xs:long|發出之記錄的序號。|  
|EventTime|xs:dateTime|發出事件時的 UTC 時間。|  
|名稱|xs:string|CustomTrackingRecord 的名稱。|  
|ActivityName|xs:string|發出 CustomTrackingRecord 的活動名稱。|  
|ActivityId|xs:string|發出 CustomTrackingRecord 的活動 ID。|  
|ActivityInstanceId|xs:string|發出 CustomTrackingRecord 的活動執行個體 ID。|  
|ActivityTypeName|xs:string|發出 CustomTrackingRecord 的活動名稱。|  
|資料|xs:string|使用此事件所追蹤的資料。  值會儲存在 xml 中的項目格式\<項目 >\<項目名稱 ="dataName"t"> dataValue\</項目 > \< /i >。  如果沒有追蹤的資料，則此字串包含\<項目 / >。 ETW 事件大小會受到 ETW 緩衝區大小或 ETW 事件的最大承載所限制。 如果事件大小超過 ETW 限制，則捨棄註釋，並取代資料值來截斷事件\<項目 >... \< /i >。  以下型別會儲存為由 ToString(); string,char,bool,int,short,long,uint,ushort,ulong,System.Single,float,double,System.Guid,System.DateTimeOffset,System.DateTime 傳回給它們的值。  所有其他的型別會使用 System.Runtime.Serialization.NetDataContractSerializer 序列化。|  
|標註|xs:string|加入至此事件中的附註。  值會儲存在 xml 中的項目格式\<項目 >\<項目名稱 ="annotationName"t"> 以\</項目 > \< /i >。  如果沒有註釋指定的字串包含\<項目 / >。 ETW 事件大小會受到 ETW 緩衝區大小或 ETW 事件的最大承載所限制。 如果事件大小超過 ETW 限制，則捨棄註釋，並取代註釋值來截斷事件\<項目 >... \< /i >。|  
|ProfileName|xs:string|造成發送這個事件的名稱或追蹤設定檔。|  
|HostReference|xs:string|若為 Web 主控服務，此欄位會唯一識別 Web 階層架構中的服務。  其格式定義為 ' Web Site Name Application Virtual Path&#124;Service Virtual Path&#124;ServiceName' 範例：'Default Web Site/CalculatorApplication&#124;/CalculatorService.svc&#124;CalculatorService'|  
|AppDomain|xs:string|由 AppDomain.CurrentDomain.FriendlyName 傳回的字串。|
