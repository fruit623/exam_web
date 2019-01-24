> 摘自IBM官方文档[IBM官方文档](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040710_.htm)

## API 完成代码

以下是 IBM MQ 返回的完成代码 (MQCC) 列表：

 0. 成功完成 (MQCC_OK)。调用圆满完成；已设置所有输出参数。 *在此情况下，Reason 参数的值始终为 MQRC_NONE。*
 1. 警告（部分完成）(MQCC_WARNING)。部分完成了调用。除 CompCode 和 Reason 输出参数外，可能还设置了一些输出参数。 *Reason 参数提供其他信息。*
 2. 调用失败 (MQCC_FAILED)
未完成调用的处理，并且通常没有更改队列管理器的状态；对异常情况进行了特别记录。仅设置了 CompCode 和 Reason 输出参数；所有其他参数均保持不变。
*原因可能是应用程序中存在故障，或者可能是该程序以外的某些情况导致的结果，例如应用程序的权限可能已被撤销。Reason 参数提供其他信息。*

## API 原因码

原因码参数 (Reason) 是完成代码参数 (CompCode) 的限定条件。

如果没有需要报告的特殊原因，那么将返回 MQRC_NONE。成功调用将返回 MQCC_OK 和 MQRC_NONE。

如果完成代码为 MQCC_WARNING 或 MQCC_FAILED，那么队列管理器将始终报告合理的原因；每个调用描述下都会提供详细信息。

在用户出口例程设置完成代码和原因的位置，应当遵守这些规则。此外，用户出口定义的任何特殊原因值都应当小于零，以确保不与队列管理器定义的值冲突。出口可以在适当的情况下设置队列管理器已定义的原因。

原因码还会出现在：
 
 - MQDLH 结构的 Reason 字段
 - MQMD 结构的 Feedback 字段
  
以下是按数字顺序排列的原因码列表，其中提供了帮助您理解的详细信息，包括：

 - 导致生成该代码的情况说明
 - 关联的完成代码
 - 建议程序员为响应该代码执行的操作

**具体代码：**



 - [0 (0000) (RC0): MQRC_NONE](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040720_.htm)

> **说明**
  调用正常完成。 完成代码 (CompCode) 为 MQCC_OK。
**完成代码**
  MQCC_OK
**程序员响应**
  无。

 - [900 (0384) (RC900): MQRC_APPL_FIRST](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040730_.htm)

> **说明**
	这是数据转换出口返回且由应用程序定义的原因码的最低值。数据转换出口	可以返回范围在 MQRC_APPL_FIRST 到 MQRC_APPL_LAST 内的原因码，指示该出口检测到的特定条件。
**完成代码**
	MQCC_WARNING 或 MQCC_FAILED
**程序员响应**
	由数据转换出口的编写者定义。

- [999 (03E7) (RC999): MQRC_APPL_LAST](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040740_.htm)

> **说明**
这是数据转换出口返回且由应用程序定义的原因码的最高值。数据转换出口可以返回范围在 MQRC_APPL_FIRST 到 MQRC_APPL_LAST 内的原因码，指示该出口检测到的特定条件。
**完成代码**
MQCC_WARNING 或 MQCC_FAILED
**程序员响应**
由数据转换出口的编写者定义。
- [2001 (07D1) (RC2001): MQRC_ALIAS_BASE_Q_TYPE_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040750_.htm)
> **说明**
已发出 MQOPEN 或 MQPUT1 调用，指定别名队列作为目标，但别名队列定义中的 BaseQName 解析为的队列不是本地队列、远程队列的本地定义或集群队列。[V9.0.0.1 May 2017][V9.0.1 Nov 2016] 或者分发列表中的队列包含指向主题对象的别名队列。
**完成代码**
MQCC_FAILED
**程序员响应**
请更正队列定义。
此原因码还用于标识相应的事件消息别名基本队列类型错误。

- [2002 (07D2) (RC2002): MQRC_ALREADY_CONNECTED](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040760_.htm)

> **说明**
发出了 MQCONN 或 MQCONNX 调用，但应用程序已连接到了队列管理器。
>- 在 z/OS® 上，仅针对批处理和 IMS™ 应用程序显示此原因码；针对 CICS® 应用程序不显示此原因码。
>- 在 UNIX、IBM® i、Linux 和 Windows 上，如果应用程序在针对线程已存在非共享句柄时尝试创建非共享句柄，那么将显示此原因码。一个线程可具有多个非共享句柄。
>- 在 UNIX、IBM i、Linux 和 Windows 上，如果在 MQ 通道出口、API 交叉出口或异步使用回调函数中发出 MQCONN 调用，并且将共享 hConn 绑定到了此线程，那么将显示此原因码。
>- 在 UNIX、IBM i、Linux 和 Windows 上，如果在 MQ 通道出口、API 交叉出口或异步使用回调函数中发出未指定 MQCNO_HANDLE_SHARE_* 选项之一的 MQCONNX 调用，并且将共享 hConn 绑定到了此线程，那么将显示此原因码。
>- 在 Windows 上，MTS 对象不接收此原因码，因为允许建立指向队列管理器的其他连接。
>
>**完成代码**
MQCC_WARNING
>**程序员响应**
无。返回的 Hconn 参数的值与针对前面的 MQCONN 或 MQCONNX 调用返回的值相同。
返回此原因码的 MQCONN 或 MQCONNX 调用并不意味着必须发出其他 MQDISC 调用以断开与队列管理器的连接。如果由于在已完成 MQCONN 的情况下调用了该应用程序而导致返回此原因码，那么请勿发出相应的 MQDISC，因为这会导致发出原始 MQCONN 或 MQCONNX 调用的应用程序也断开连接。

- [2003 (07D3) (RC2003): MQRC_BACKED_OUT](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040770_.htm)

> **说明**
当前工作单元遇到不可恢复错误或已被回退。在下列情况下会发出此原因码：
>- 对于 MQCMIT 或 MQDISC 调用，当落实操作失败并且工作单元被回退时。参与该工作单元的所有资源都将返回至它们在该工作单元开始时的状态。在这种情况下，MQCMIT 或 MQDISC 调用完成并发出 MQCC_WARNING。
>>- 在 z/OS® 上，只针对批处理应用程序才会出现此原因码。 
>
>- 对于在工作单元中运行的 MQGET、MQPUT 或 MQPUT1 调用，当工作单元已遇到阻止该工作单元落实的错误（例如，日志空间耗尽时）时。应用程序必须发出相应的调用来回退该工作单元。（对于由队列管理器协调的工作单元，此调用为 MQBACK 调用，但 MQCMIT 调用在这些情况下具有相同的效果。）在此情况下，MQGET、MQPUT 或 MQPUT1 调用会完成并发出 MQCC_FAILED。
>>- 在 z/OS 上，不会发生此情况。 
>
>- 对于异步使用回调（通过 MQCB 调用注册），将回退工作单元，且异步使用程序应调用 MQBACK。
>>- 在 z/OS 上，不会发生此情况。 
>
>-  对于使用 TMF 的 IBM® MQ Client for HP Integrity NonStop Server，在以下情况下可能出现此返回码：
>>- 对于 MQGET、MQPUT 和 MQPUT1 调用，如果存在由 TMF 协调的活动事务，但事务的 IBM MQ 部分由于在该事务上不活动而遭回退。
>>- 如果 TMF/网关检测到 TMF 在应用程序完成当前事务之前回滚当前事务。 
>
>**完成代码**
MQCC_WARNING 或 MQCC_FAILED
>**程序员响应**
检查先前对队列管理器调用的返回。例如，先前的 MQPUT 调用可能失败。

- [2004 (07D4) (RC2004): MQRC_BUFFER_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040780_.htm)

> **说明**
Buffer 参数由于以下原因之一无效：
>- 参数指针无效。（并非始终可以检测出无效的参数指针；如果未检测到，那么会发生不可预测的结果。）
>- 参数指针指向的存储器不足，无法存放 BufferLength 指定的整个长度。
>- 对于 Buffer 为输出参数的调用：该参数指针指向只读存储器。
>
>**完成代码**
MQCC_FAILED
**程序员响应**
请更正该参数。

[2005 (07D5) (RC2005): MQRC_BUFFER_LENGTH_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040790_.htm)

> **说明**
BufferLength 参数无效，或参数指针无效。（并非始终可以检测出无效的参数指针；如果未检测到，那么会发生不可预测的结果。）
>- 如果通道的协商最大消息大小小于任何调用结构的固定部分，那么在进行 MQCONN 或 MQCONNX 调用时，也会向 MQ MQI 客户机程序返回此原因。
>- 如果 AuthorityBuffer 参数过小，无法容纳要返回给 MQZ_ENUMERATE_AUTHORITY_DATA 可安装服务组件调用者的数据，那么此服务组件也应返回此原因。
>- 如果在需要正数长度的位置提供了零长度的多点广播消息，那么也会返回此原因码。
>
>**完成代码**
MQCC_FAILED
**程序员响应**
请指定零或大于零的值。对于 mqAddString 和 mqSetString 调用，特殊值 MQBL_NULL_TERMINATED 同样有效。

- [2006 (07D6) (RC2006): MQRC_CHAR_ATTR_LENGTH_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040800_.htm)

> **说明**
CharAttrLength 为负（针对 MQINQ 或 MQSET 调用），或长度不足以存储所选的所有属性（仅 MQSET 调用）。如果参数指针无效，那么也会出现此原因。（并非始终可以检测出无效的参数指针；如果未检测到，那么会发生不可预测的结果。）
**完成代码**
MQCC_FAILED
**程序员响应**
请指定足够大的值，以保存所有所选属性的合并字符串。

- [2007 (07D7) (RC2007): MQRC_CHAR_ATTRS_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040810_.htm)

> **说明**
CharAttrs 无效。参数指针无效，或针对 MQINQ 调用指向只读存储器，或指向长度不足 CharAttrLength 所指示长度的存储器。（并非始终可以检测出无效的参数指针；如果未检测到，那么会发生不可预测的结果。）
**完成代码**
MQCC_FAILED
**程序员响应**
请更正该参数。

- [2008 (07D8) (RC2008): MQRC_CHAR_ATTRS_TOO_SHORT](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040820_.htm)

> **说明**
对于 MQINQ 调用，CharAttrLength 不够大，无法包含在 Selectors 参数中指定了 MQCA_* 选择器的所有字符属性。
该调用仍会完成，并会在 CharAttrs 参数字符串中填充空间能够容纳的字符属性数。仅返回完整的属性字符串：如果剩余空间不足以容纳整个属性，那么将忽略该属性以及后续的字符属性。字符串末尾未用于保存属性的任何空间都不会更改。
表示一组值的属性（例如，名称列表 Names 属性）将视为单个实体，要么返回其所有值，要么不返回任何值。
**完成代码**
MQCC_WARNING
**程序员响应**
除非只需要值的子集，否则请指定足够大的值。

- [2009 (07D9) (RC2009): MQRC_CONNECTION_BROKEN](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040830_.htm)

> **说明**
与队列管理器的连接已丢失。发生这一情况的原因可能是队列管理器已终止运行。如果调用是具有 MQGMO_WAIT 选项的 MQGET 调用，那么等待已取消。 所有连接及对象句柄现已无效。
>对于 MQ MQI 客户机应用程序，虽然此代码与 MQCC_FAILED 的 CompCode 一起返回，但调用可能已成功完成。
**完成代码**
MQCC_FAILED
**程序员响应**
应用程序可通过发出 MQCONN 或 MQCONNX 调用尝试重新连接到队列管理器。可能需要轮询，直到收到成功响应。
>- 对于 z/OS® for CICS® 应用程序，不需要发布 MQCONN 或 MQCONNX 调用，因为会自动连接 CICS 应用程序。
>
>工作单元中任何未提交的更改应该予以回退。由队列管理器协调的工作单元自动回退。
>- 对于 z/OS IMS™，使用 IMS DIS SUBSYS 命令检查是否启动了子系统，如果需要，使用 IMS STA SUBSYS 命令将其启动。

- [2010 (07DA) (RC2010): MQRC_DATA_LENGTH_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040840_.htm)

> **说明**
DataLength 参数无效。参数指针无效，或者它指向只读存储器。（并非始终可以检测出无效的参数指针；如果未检测到，那么会发生不可预测的结果。）
如果 BufferLength 参数超过为客户机通道协商的最大消息大小，那么此原因也可以返回到 MQGET、MQPUT 或 MQPUT1 调用上的 MQ MQI 客户机程序。这可能是因为未对通道定义正确设置 MAXMSGL（请参阅最大消息长度 (MAXMSGL)），或者如果使用 MQCONNX 并提供 MQCD，那么需要将该数据结构的 MaxMsgLength 设置为更高的值（请参阅使用 MQCONNX）。
**完成代码**
MQCC_FAILED
**程序员响应**
请更正该参数。
如果 MQ MQI 客户机程序发生错误，另请检查通道的最大消息大小是否足以容纳发送的消息；如果不够大，请增大通道的最大消息大小。

- [2011 (07DB) (RC2011): MQRC_DYNAMIC_Q_NAME_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040850_.htm)

> **说明**
在 MQOPEN 调用上，ObjDesc 参数的 ObjectName 字段中指定了模型队列，但是由于以下原因之一，DynamicQName 字段无效：
>- DynamicQName 完全为空白（或者直至字段中的第一个空字符全都为空白）。
>- 存在对于队列名称无效的字符。
>- 存在超出第 33 个位置（并在任何空字符之前）的星号。
>- 存在星号，后跟不为空且非空白的字符。
>
>当服务器应用程序打开由服务器刚接收到的消息的 MQMD 中的 ReplyToQ 和 ReplyToQMgr 字段指定的应答队列时，有时也可出现此原因码。在此情况下，原因码表明发送原始消息的应用程序将不正确的值放至原始消息的 MQMD 中的 ReplyToQ 和 ReplyToQMgr 字段。
**完成代码**
MQCC_FAILED
**程序员响应**
指定有效的名称。

- [2012 (07DC) (RC2012): MQRC_ENVIRONMENT_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040860_.htm)

> **说明**
调用对于当前环境无效。
>- 在 z/OS® 上，当以下情况之一适用时：
>>- 已发出 MQCONN 或 MQCONNX 调用，但是应用程序已与该应用程序运行环境中不支持的适配器进行链接。例如，当应用程序与 MQ RRS 适配器进行链接，但是该应用程序在 DB2® 存储过程地址空间中运行时，可能会发生此情况。在此环境中不支持 RRS。希望使用 MQ RRS 适配器的存储过程必须在 Db2 WLM 管理的存储过程地址空间中运行。
>>- 已发出 MQCMIT 或 MQBACK 调用，但是应用程序已与 RRS 批处理适配器 CSQBRSTB 进行链接。此适配器不支持 MQCMIT 和 MQBACK 调用。
>>- 已在 CICS® 或 IMS™ 环境中发出 MQCMIT 或 MQBACK 调用。
>>- RRS 子系统在运行应用程序的 z/OS 系统上不可运行。
>>- 已发出注册事件侦听器的 MQCTL 调用（带有 MQOP_START）或 MQCB 调用，但是不允许应用程序创建 POSIX 线程。
>>- 用于 Java™ 的 IBM® MQ 类 应用程序已在不支持的环境中使用 CLIENT 传输将 MQQueueManager 对象实例化。
z/OS 环境针对 Java 应用程序仅支持 IBM MQ V9.0.4（和更高版本的）类，这些应用程序使用 CLIENT 传输连接到在 z/OS（具有 ADVCAP(ENABLED)）上运行的 IBM MQ V9.0.4（和更高版本的）队列管理器。
请参阅 [DISPLAY QMGR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.ref.adm.doc/q086240_.htm)，以获取有关 ADVCAP 的更多信息。
使用 CLIENT 传输的 用于 Java 的 IBM MQ 类 或 用于 JMS 的 IBM MQ 类 应用程序访问了受 Advanced Message Security for z/OS 策略保护的队列。在使用 CLIENT 传输时，z/OS 环境不支持 Advanced Message Security for z/OS。
>- 在 IBM i、[HPNSS]HP Integrity NonStop Server、 UNIX 和 Windows 上，当以下情况之一适用时：
>>- 应用程序链接到不受支持的库。
>>- 应用程序链接到错误的库（线程化或非线程化）。
>>- 已发出 MQBEGIN、MQCMIT 或 MQBACK 调用，但是外部工作单元管理器在使用中。例如，当 MTS 对象作为 DTC 事务运行时，此原因码出现在 Windows 上。如果队列管理器不支持工作单元，那么也会出现此原因码。
>>- 已在 MQ MQI 客户机环境中发出 MQBEGIN 调用。
>>- 已发出 MQXCLWLN 调用，但是调用未源自集群工作负载出口。
>>- 已发出 MQCONNX 调用，从 MQ 通道出口、API 出口或回调函数内指定选项 MQCNO_HANDLE_SHARE_NONE。仅当共享 hConn 绑定到应用程序线程时，才会出现此原因码。
>>- IBM MQ 对象无法连接快速路径。
>>- 用于 Java 的 IBM MQ 类 应用程序已创建使用 CLIENT 传输的 MQQueueManager 对象，然后调用了 MQQueueManager.begin()。只能在使用 BINDINGS 传输的 MQQueueManager 对象上调用此方法。
>- 在 Windows 上，当使用受管 .NET 客户机时，已尝试使用不受支持的功能之一：
>>- 非受管通道出口
>>- 传输层安全性 (TLS)
>>- XA 事务
>>- 除 TCP/IP 以外的通信
>>- 通道压缩
>
>**完成代码**
MQCC_FAILED
**程序员响应**
执行以下某项操作：
>- 在 z/OS 上：
>>- 将应用程序与正确的适配器进行链接。
>>- 修改应用程序以使用 SRRCMIT 和 SRRBACK 调用来替代 MQCMIT 和 MQBACK 调用。或者，将应用程序与 RRS 批处理适配器 CSQBRRSI 进行链接。除 SRRCMIT 和 SRRBACK 以外，此适配器还支持 MQCMIT 和 MQBACK。
>>- 对于 CICS 或 IMS 应用程序，发出相应的 CICS 或 IMS 调用来落实或回退工作单元。
>>- 在运行应用程序的 z/OS 系统上启动 RRS 子系统。
>>- 如果应用程序使用语言环境 (LE)，请确保它使用 DLL 接口并与 POSIX(ON) 一起运行。
>>- 确保允许应用程序使用 UNIX 系统服务 (USS)。
>>- 确保本地 z/OS 应用程序和 WebSphere® Application Server 应用程序的连接工厂定义使用带有绑定方式连接的传输类型。
>>- 确保已建立到受支持队列管理器的任何客户机方式连接，并且不访问受 IBM MQ Advanced Message Security for z/OS 策略保护的任何队列。
>- 在其他环境中：
>>- 将应用程序与正确的库（线程化或非线程化）进行链接。
>>- 从应用程序中移除不受支持的调用或功能。
>>- 如果要运行快速路经，请将应用程序更改为运行 setuid。

- [2013 (07DD) (RC2013): MQRC_EXPIRY_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040870_.htm)

> **说明**
在 MQPUT 或 MQPUT1 调用上，为消息描述符 MQMD 中的 Expiry 字段指定的值无效。
此原因码也由 JMS 应用程序生成，指定传送延迟值大于以下任一项：
>- 由应用程序指定的消息到期时间，或
>- 由目标队列或主题解析过程中所使用的对象的 CUSTOM(CAPEXPRY) 属性设置的到期时间。
>
>**完成代码**
MQCC_FAILED
**程序员响应**
指定大于零的值或特殊值 MQEI_UNLIMITED。
确保 JMS 应用程序指定的传送延迟小于：
>- 由应用程序指定的消息到期时间，或
>- 由目标队列或主题解析过程中所使用的对象的 CUSTOM(CAPEXPRY) 属性设置的到期时间。

- [2014 (07DE) (RC2014): MQRC_FEEDBACK_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040880_.htm)

> **说明**
在 MQPUT 或 MQPUT1 调用上，为消息描述符 MQMD 中的 Feedback 字段指定的值无效。值不是 MQFB_NONE，并且超出为系统反馈代码定义的范围和为应用程序反馈代码定义的范围。
**完成代码**
MQCC_FAILED
**程序员响应**
指定 MQFB_NONE，或者范围在 MQFB_SYSTEM_FIRST 到 MQFB_SYSTEM_LAST 或 MQFB_APPL_FIRST 到 MQFB_APPL_LAST 之间的值。

- [2016 (07E0) (RC2016): MQRC_GET_INHIBITED](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040890_.htm)

> **说明**
此队列或此队列解析到的队列当前禁止 MQGET 调用。
**完成代码**
MQCC_FAILED
**程序员响应**
如果系统设计允许短期禁止 put 请求，请稍后重试该操作。
此原因码也用于识别对应事件消息遭到禁止。
**系统程序员操作**
使用 ALTER QLOCAL(...) GET(ENABLED) 以允许获取消息。

- [2017 (07E1) (RC2017): MQRC_HANDLE_NOT_AVAILABLE](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040900_.htm)

> **说明**
已发出 MQOPEN、MQPUT1 或 MQSUB 调用，但是已经达到当前任务允许的最大开放句柄数。请注意，当在 MQOPEN 或 MQPUT1 调用上指定了分发列表时，分发列表中的每个队列都使用一个句柄。
>- 在 z/OS® 上，“任务”表示 CICS® 任务、z/OS 任务或 IMS™ 从属区域。
>
>此外，MQSUB 调用还在您不提供输入上的对象句柄时分配两个句柄。
**完成代码**
MQCC_FAILED
**程序员响应**
检查应用程序是否发出的是没有对应 MQCLOSE 调用的 MQOPEN 调用。如果是，请修改应用程序，以在不再需要开放对象时即为每个开放对象发出 MQCLOSE 调用。
此外，检查应用程序是否指定了包含大量正在使用所有可用句柄的队列的分发列表。如果是，请增大任务可以使用的最大句柄数，或者减小分发列表的大小。任务可以使用的最大开发句柄数由 MaxHandles 队列管理器属性给定。

- [2018 (07E2) (RC2018): MQRC_HCONN_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040910_.htm)

> **说明**
由于以下原因之一，连接句柄 Hconn 无效：
>- 参数指针无效，或者（对于 MQCONN 或 MQCONNX 调用）指向只读存储器。（并非始终可以检测出无效的参数指针；如果未检测到，那么会发生不可预测的结果。）
>- 所指定的值不是由先前的 MQCONN 或 MQCONNX 调用返回。
>- 所指定的值因先前的 MQDISC 调用而已失效。
>- 句柄是因其他线程发出 MQDISC 调用而已失效的共享句柄。
>- 句柄是正在 MQBEGIN 调用上使用的共享句柄（在 MQBEGIN 上，仅非共享句柄有效）。
>- 句柄是由未创建句柄的线程使用的非共享句柄。
>- 在句柄无效的情况下，已在 MTS 环境中发出调用（例如，在进程或包之间传递句柄；请注意，支持在库包之间传递句柄）。
>- 当通过使用 CICS® TS 3.2 或更高版本运行字符转换出口程序来调用 MQXCNVC 调用时，转换程序未定义为 OPENAPI。当转换过程运行时，TCB 切换到 Quasi Reentrant (QR) TCB，致使连接不正确。
>
>**完成代码**
MQCC_FAILED
**程序员响应**
确保为队列管理器执行成功的 MQCONN 或 MQCONNX 调用，并且尚未为其执行 MQDISC 调用。确保句柄是在其有效作用域内使用（请参阅 MQCONN 中的 MQCONN 描述以获取有关 MQCONN 的更多信息）。
>- 在 z/OS® 上，另请检查应用程序是否已与正确的存根进行链接；此存根对于 CICS 应用程序为 CSQCSTUB，对于批处理程序为 CSQBSTUB，对于 IMS™ 应用程序为 CSQQSTUB。此外，所使用的存根不得属于比应用程序将运行所在的发行版更近的队列管理器发行版。
>
>确保由 CICS TS 3.2 或更高版本应用程序（调用 MQXCNVC 调用）运行的字符转换出口程序定义为 OPENAPI。此定义防止因连接不正确导致 2018 MQRC_HCONN_ERROR 错误，并且允许完成 MQGET。

- [2019 (07E3) (RC2019): MQRC_HOBJ_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040920_.htm)

> **说明**
由于以下原因之一，对象句柄 Hobj 无效：
>- 参数指针无效，或者（对于 MQOPEN 调用）指向只读存储器。（并非始终可以检测出无效的参数指针；如果未检测到，那么会发生不可预测的结果。）
>- 所指定的值不是由先前的 MQOPEN 调用返回。
>- 所指定的值因先前的 MQCLOSE 调用已失效。
>- 句柄是因其他线程发出 MQCLOSE 调用而已失效的共享句柄。
>- 句柄是由未创建句柄的线程使用的非共享句柄。
>- 调用是 MQGET 或 MQPUT，但是句柄所表示的对象不是队列。
>
>**完成代码**
MQCC_FAILED
**程序员响应**
确保为此对象执行成功的 MQOPEN 调用，并且尚未为其执行 MQCLOSE 调用。确保句柄在其有效作用域内使用（请参阅 MQOPEN 中的 MQOPEN 描述以获取更多信息）。

- [2020 (07E4) (RC2020): MQRC_INHIBIT_VALUE_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040930_.htm)

> **说明**
在 MQSET 调用上，为 MQIA_INHIBIT_GET 属性或 MQIA_INHIBIT_PUT 属性指定的值无效。
**完成代码**
MQCC_FAILED
**程序员响应**
请为 InhibitGet 或 InhibitPut 队列属性指定有效值。

- [2021 (07E5) (RC2021): MQRC_INT_ATTR_COUNT_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040940_.htm)

> **说明**
在 MQINQ 或 MQSET 调用上，IntAttrCount 参数为负（MQINQ 或 MQSET），或者小于 Selectors 参数中指定的整数属性选择器 (MQIA_*) 的数量（仅 MQSET）。如果参数指针无效，那么也会出现此原因。（并非始终可以检测出无效的参数指针；如果未检测到，那么会发生不可预测的结果。）
**完成代码**
MQCC_FAILED
**程序员响应**
指定对于所有所选整数属性足够大的值。

- [2022 (07E6) (RC2022): MQRC_INT_ATTR_COUNT_TOO_SMALL](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040950_.htm)

> **说明**
在 MQINQ 调用上，IntAttrCount 参数小于 Selectors 参数中指定的整数属性选择器 (MQIA_*) 的数量。
该调用完成并带有 MQCC_WARNING，其中 IntAttrs 数组使用尽可能多的整数属性进行了填充。
**完成代码**
MQCC_WARNING
**程序员响应**
除非只需要值的子集，否则请指定足够大的值。

- [2023 (07E7) (RC2023): MQRC_INT_ATTRS_ARRAY_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040960_.htm)

> **说明**
在 MQINQ 或 MQSET 调用上，IntAttrs 参数无效。参数指针无效（MQINQ 和 MQSET），或者指向只读存储器或长度与 IntAttrCount 参数所指示长度不符的存储器（仅 MQINQ）。（并非始终可以检测出无效的参数指针；如果未检测到，那么会发生不可预测的结果。）
**完成代码**
MQCC_FAILED
**程序员响应**
请更正该参数。

- [2024 (07E8) (RC2024): MQRC_SYNCPOINT_LIMIT_REACHED](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040970_.htm)

> **说明**
MQGET、MQPUT 或 MQPUT1 调用失败，因为它会导致当前工作单元中的未落实消息数超过为队列管理器定义的限制（请参阅 MaxUncommittedMsgs 队列管理器属性）。未落实消息数是自当前工作单元启动以来的以下消息数的总和：
>- 应用程序使用 MQPMO_SYNCPOINT 选项放置的消息
>- 应用程序使用 MQGMO_SYNCPOINT 选项检索的消息
>- 由队列管理器针对使用 MQPMO_SYNCPOINT 选项放置的消息生成的触发器消息和 COA 报告消息
>- 由队列管理器针对使用 MQGMO_SYNCPOINT 选项检索的消息生成的 COD 报告消息
>- 在 HP Integrity NonStop Server 上，当超过单个 TM/MP 事务中的最大 I/O 操作数时，将会出现此原因码。
>
>将同步点外的消息发布到主题时，可能会接收此原因码；请参阅同步点下的发布以获取更多信息。
**完成代码**
MQCC_FAILED
**程序员响应**
检查应用程序是否正在循环。如果未在循环，请考虑降低应用程序的复杂性。或者，增大工作单元中未落实消息的最大数量的队列管理器限制。
>- 在 z/OS® 上，可以使用 ALTER QMGR 命令来更改未落实消息的最大数量的限制。
>- 在 IBM® i 上，可以使用 CHGMQM 命令来更改未落实消息的最大数量的限制。
>- 在 HP Integrity NonStop Server 上，应用程序应取消事务并使用工作单元中更小的操作数进行重试。请参阅《MQSeries® for Tandem NonStop Kernel 系统管理指南》以获取更多详细信息。

- [2025 (07E9) (RC2025): MQRC_MAX_CONNS_LIMIT_REACHED](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040980_.htm)

> **说明**
已拒绝 MQCONN 或 MQCONNX 调用，因为已超过最大并发连接数。
>- 在 z/OS® 上，连接限制对于 TSO 和批处理均为 32767。
>- 在 IBM® i、[HPNSS]HP Integrity NonStop Server、UNIX 和 Windows 上，此原因码也可出现在 MQOPEN 调用上。
>- 使用 Java™ 应用程序时，连接管理器可能会定义并发连接数的限制。
注： `使用 IBM MQ 的应用程序可能已将连接的管理委派给框架或连接池，例如，Java EE 应用程序服务器、应用程序框架（如 Spring）、IBM 容器（针对 IBM Cloud（原称为 Bluemix™））或这些对象的组合。有关更多信息，请参阅使用 JMS 连接池。`
>
>**完成代码**
MQCC_FAILED
**程序员响应**
请增大相应参数值的大小，或者减少并发连接数。

- [2026 (07EA) (RC2026): MQRC_MD_ERROR](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_9.0.0/com.ibm.mq.tro.doc/q040990_.htm)

> **说明**
由于以下原因之一，MQMD 结构无效：
>- StrucId 字段不是 MQMD_STRUC_ID。
>- Version 字段指定的值无效或不受支持。
>- 参数指针无效。（并非始终可以检测出无效的参数指针；如果未检测到，那么会发生不可预测的结果。）
>- 即使调用成功，队列管理器也无法将更改的结构复制到应用程序存储器。例如，如果指针指向只读存储器，那么可能发生此情况。
>
> **完成代码**
MQCC_FAILED
**程序员响应**
确保正确设置 MQMD 结构中的输入字段。
























































































































































