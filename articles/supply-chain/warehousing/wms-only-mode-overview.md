---
title: Warehouse management only mode overview (preview)
description: This article provides information about Warehouse management only mode, which enables the integration of warehouse management (WMS) functionality in Microsoft Dynamics 365 Supply Chain Management with external enterprise resource planning (ERP) and order management systems.
author: perlynne
ms.author: perlynne
ms.reviewer: kamaybac
ms.search.form:
ms.topic: overview
ms.date: 01/29/2024
audience: Application User
ms.search.region: Global
ms.custom: bap-template
---

# Warehouse management only mode overview (preview)

[!include [banner](../includes/banner.md)]
[!INCLUDE [preview-banner](../includes/preview-banner.md)]

<!-- KFM: Preview until further notice -->

*Warehouse management only mode* lets you take advantage of the core warehouse management (WMS) functionality that Microsoft Dynamics 365 Supply Chain Management offers while you also continue to take advantage of your existing investments in third-party enterprise resource planning (ERP) and order management systems. Regardless of the ERP or ordering systems that you have in place, you can now quickly deploy our advanced WMS functionality without having to set up or maintain areas of Supply Chain Management that you don't need. You're then ready to benefit from advanced WMS features such as automation integration, carrier integration, and the Warehouse Management mobile app.

The integration of Supply Chain Management WMS functionality with external ERP and ordering systems is made possible by lightweight source documents that are dedicated to inbound and outbound shipment orders. Because these documents focus exclusively on warehouse management, they can replace multiple types of more general-purpose documents (such as sales orders, purchase orders, and transfer orders) from a pure warehouse management perspective.

[!INCLUDE [preview-note](../includes/preview-note.md)]

## Deployment options

Warehouse management only mode provides several deployment options to support the business needs of running your [warehouse management](warehouse-management-overview.md) processes.

You can [start a free trial of Dynamics 365 Supply Chain Management](https://go.microsoft.com/fwlink/?linkid=2252982) via the [unified admin experience for finance and operations apps](/power-platform/admin/unified-experience/finance-operations-apps-overview). You can then try out an implementation as shown in the following high-level diagram of the elements and processes of an integrated system. For more information, see [this example that shows how to use inbound and outbound shipment orders](wms-only-mode-example.md).

:::image type="content" source="media/wms-only-high-level-integrations.svg" alt-text="High-level integration diagram." lightbox="media/wms-only-high-level-integrations.svg":::

The solution is very flexible. Therefore, you can choose the configuration of features and systems that best fits your business needs. Here are a few examples of ways that you can integrate Supply Chain Management warehouse management mode with other systems:

- Use Supply Chain Management to handle only warehouse operations, and use an external system to handle all orders and financial processing. For this type of implementation, you must configure the warehouse management processes as part of Supply Chain Management.
- Use Supply Chain Management to handle warehousing plus a wider range of processes (such as sales, purchase, and production orders), and also use it to handle warehouse operations for other ERP and order processing systems. For this type of implementation, the same warehouse instance can handle all the logistic warehouse processes for both internal and external integrations.
- Use a combination of the previous two approaches. For example, you can set up a dedicated legal entity in the Supply Chain Management deployment that handles only the warehouse management processes for external systems.

## Inbound process

The following illustration highlights the elements of the inbound process.

:::image type="content" source="media/wms-only-inbound-wom-process.svg" alt-text="Inbound process for Warehouse management only mode." lightbox="media/wms-only-inbound-wom-process.svg":::

Here's a high-level description of the inbound process:

1. An external system submits an *inbound shipment order* message to Supply Chain Management.
1. Supply Chain Management processes the message in Warehouse management only mode and creates orders.
1. Inbound loads are created manually, automatically, or through import (depending on your configuration).
1. A warehouse worker uses the Warehouse Management mobile app to *register* the inbound shipment transactions.
1. Supply Chain Management runs [receiving completed](wms-only-mode-using.md#receiving-completed) processes that are related to each relevant load. These processes update the load status to *Received*, generate [shipment receipts](wms-only-mode-using.md#shipment-receipts), and trigger *business events* for the external systems.
1. The external systems read and use the [shipment receipt](wms-only-mode-using.md#shipment-receipts) data for further processing. For example, if purchase orders are associated with the inbound shipment orders in the external system, this processing involves purchase order invoicing.
1. Supply Chain Management finalizes the inbound shipment orders by running the *Post shipment receipts* [batch job](../../fin-ops-core/dev-itpro/sysadmin/process-automation.md).

For a more detailed description of this process and the related processes, see [Work with warehouse management only mode in Supply Chain Management](wms-only-mode-using.md).

## Outbound process

The following illustration highlights the elements of the outbound process.

:::image type="content" source="media/wms-only-outbound-wom-process.svg" alt-text="Outbound process for Warehouse management only mode." lightbox="media/wms-only-outbound-wom-process.svg":::

Here's a high-level description of the outbound process:

1. An external system submits an *outbound shipment order* message.
1. Supply Chain Management processes the message in Warehouse management only mode and creates orders.
1. Inventory reservations are created either manually or automatically (depending on your configuration).
1. The orders are released for further warehouse processing, either manually or automatically. If you're using outbound load planning processes, you can create loads by using the outbound load planning workbench before you release the orders.
1. Depending on the setup of your [wave template](wave-templates.md) definitions, warehouse work might be created and released immediately.
1. The outbound warehouse work is processed, and the status of the related outbound shipment order line transactions is updated to *Picked*.
1. The loads are outbound ship confirmed. As a result, *business events* and a [*shipment packing slip*](wms-only-mode-using.md#shipment-packing-slips) are created for the external system.
1. The external system reads the shipment packing slip and uses its data for further processing (such as sales order invoicing for sales orders that are associated with outbound shipment orders).
1. Supply Chain Management finalizes the outbound shipment order by running the *Post shipment packing slips* [batch job](../../fin-ops-core/dev-itpro/sysadmin/process-automation.md).

For a more detailed description of this process and the related processes, see [Work with warehouse management only mode in Supply Chain Management](wms-only-mode-using.md).

## Unsupported processes

The following high-level processes aren't supported out of the box when Supply Chain Management is integrated with external systems. The list is most relevant to existing customers who already run warehouse management processes and are considering adopting the *Warehouse management only mode* functionality.  

| Process | Description |
|---|---|
| Return order processing with disposition codes | When you use the inbound shipment orders that are related to an inbound return process, you can't use the process that's used by [sales return orders](sales-returns.md) that supports return reason codes and disposition codes as part of the flow for the *Return order receiving (and put away)* mobile device menu item. When processing return orders with [blind return receiving](sales-returns-unannounced.md#blind-return-process), the automated disposition code related to scrapping isn't supported for inbound shipment orders; likewise, processing return orders with [return details](sales-returns-unannounced.md#return-details-process) isn't supported for outbound or inbound shipment orders. |
| Inbound Warehouse Management mobile app flows | <p>The Warehouse Management mobile app flows for inbound shipment orders don't support the following functionality:</p><ul><li>[Goods in transit](../landed-cost/in-transit-processing.md#warehouse-management), where the receiving process is handled against a container</li><li>[Deferred receiving](mixed-license-plate-receiving.md#deferred-receiving-processing), where the actual inventory on-hand registration is postponed and run via background processing</li></ul> |
| Production flows | Inbound and outbound shipment orders don't support production order, batch order, or kanban processing, including material consumption and reporting as finished via the Warehouse Management mobile app. In addition, you can't use [cross-docking from production orders to outbound docks](../production-control/cross-docking-opportunities.md) in combination with inbound and outbound shipment orders. |
| Transportation management processes | The transportation management engines that are currently supported for purchase order loads aren't supported for the inbound shipment order processes. Note that charges can't be assigned, and direct invoicing can't be processed, for either inbound shipment orders or outbound shipment orders. Therefore, the apportionment weight engine can't be used to generate freight bills. |
| Creation of orders from the warehouse app | The process of creating outbound shipment orders from the Warehouse Management mobile app isn't supported. (That process resembles the *Create transfer order from license plates* process for mobile devices.) |
| Internal order processing information that's provided to external systems | When you're using the supported orders in Supply Chain Management (such as transfer, sales, purchase, and production orders), all the related business process data is automatically maintained in Supply Chain Management. However, no business events or related inbound and outbound on-hand information is provided to external systems for these types of processes. For example, if you create a transfer order, ship inventory out of one warehouse, and receive it in another warehouse in Supply Chain Management, you can't use the method that's described for inbound and outbound shipment orders to inform the external systems about the operations. You must use a different method. |
| [Order-committed reservations](flexible-warehouse-level-dimension-reservation.md) as part of the *allow reservation on demand order* capability | *Outbound shipment order line* transaction reservations don't support reservations on inventory dimensions below the location in the reservation hierarchy. (However, these reservations are supported for *Sales order line* transactions.) |
| [Owner dimension](../inventory/consignment.md#inventory-owners) values that differ from the operating legal entity | There isn't yet support for importing and processing shipment order lines where the *Owner* tracking dimension value differs from the legal entity (company) that's used. |
| Items enabled for [catch weight processing](catch-weight-processing.md) | Items that are enabled for catch weight processing aren't supported for inbound or outbound shipping orders. |
| Policies and processes around vendor or customer accounts | Representations of vendors and customers aren't used for inbound or outbound shipping orders. Therefore, you can't use related order processing policies with this type of setup. For example, you can't use customer-specific or vendor-specific [product filters](filters-and-filter-codes.md) or [nonconformance management](../inventory/quality-management-processes.md#nonconformance). |
| Order line registration and pick update processing | Inbound and outbound shipment order lines don't support the manual registration and un-registration processes that are supported by other types of order lines (such as purchase, sales, and transfer order lines). |
