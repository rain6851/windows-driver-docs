---
title: OID_TCP_TASK_IPSEC_ADD_UDPESP_SA
author: windows-driver-content
description: This topic describes the OID_TCP_TASK_IPSEC_ADD_UDPESP_SA object identifier (OID).
ms.assetid: 022db6dd-1099-4069-81af-7f919d91464e
keywords:
- OID_TCP_TASK_IPSEC_ADD_UDPESP_SA
ms.date: 11/06/2017
ms.localizationpriority: medium
---

# OID_TCP_TASK_IPSEC_ADD_UDPESP_SA

A transport protocol sets OID_TCP_TASK_IPSEC_ADD_UDPESP_SA to request a miniport driver to add one or more security associations (SAs) for UDP-encapsulated ESP packets to a NIC.

The information for each SA is formatted as an [OFFLOAD_IPSEC_ADD_UDPESP_SA](https://msdn.microsoft.com/library/windows/hardware/ff569057) structure. Note that this structure is almost identical to the [OFFLOAD_IPSEC_ADD_SA](https://msdn.microsoft.com/library/windows/hardware/ff569056) structure used in the [OID_TCP_TASK_IPSEC_ADD_SA](oid-tcp-task-ipsec-add-sa.md) request. The only difference is that the OFFLOAD_IPSEC_ADD_UDPESP_SA structure contains the **EncapTypeEntry** and the **EncapTypeEntryOffldHandle** members.

The first seven members of the OFFLOAD_IPSEC_ADD_UDPESP_SA structure (**SrcAddr**, **SrcMask**, **DestAddr**, **DestMask**, **Protocol**, **SrcPort**, and **DestPort**) constitute a filter that specifies the source and destination, as well as the IP protocols, to which the SAs apply. This filter pertains to a transport-mode connection--that is, an end-to-end connection between two hosts. If the specified connection is made through a tunnel, the source and destination addresses of the tunnel are specified by **SrcTunnelAddr** and **DestTunnelAddr**, respectively.

If a filter parameter is set to zero, that parameter is not used to filter packets for the specified SAs. For example, if **SrcAddr** is set to zero, the specified SAs can apply to a packet that contains any source address. To take this to the extreme, if all the filter parameters are set to zero, the specified SAs apply to any source host sending any type of packet to any destination host.

The TCP/IP transport can specify an IP protocol in the **Protocol** member to indicate that the specified SAs apply only to packets of the specified protocol type. If **Protocol** is set to zero, the specified SAs apply to all packets sent from the specified source to the specified destination.

## OFFLOAD_SECURITY_ASSOCIATION structure

An [OFFLOAD_SECURITY_ASSOCIATION](https://msdn.microsoft.com/library/windows/hardware/ff569061) structure specifies a single security association (SA). The OFFLOAD_SECURITY_ASSOCIATION structure is an element in the **SecAssoc** variable-length array. **SecAssoc** contains one or two OFFLOAD_SECURITY_ASSOCIATION structures.

An SA specified for use in processing authentication headers (AH) will have an operation type of **AUTHENTICATE** and will have an **IntegrityAlgo** (integrity algorithm). The SA will not have an a **ConfAlgo** (confidentiality algorithm). In this case, **ConfAlgo** will contain zeros.

An SA specified for use in processing encapsulating security payloads (ESPs) will have an operation type of **ENCRYPT** and may have an **IntegrityAlgo** (integrity algorithm) and/or a **ConfAlgo** (confidentiality algorithm).

## OFFLOAD_ALGO_INFO structure

The [OFFLOAD_ALGO_INFO](https://msdn.microsoft.com/library/windows/hardware/ff568842) structure, which is a member of an [OFFLOAD_SECURITY_ASSOCIATION](https://msdn.microsoft.com/library/windows/hardware/ff569061) structure, specifies an algorithm used for a security association (SA).

## Requirements

| | |
| --- | --- |
| Version | Windows Vista and later |
| Header | Ntddndis.h (include Ndis.h) |

