# AG_OP
--SELECT		*
--FROM	[AG-ALB].[dbo].[CCUR_PurchaseRequest]A
----JOIN	[AG-ALB].[dbo].[CORE_PurchaseOrderItems]B
----ON		A.[PurchaseOrderId]=B.[PurchaseOrderItemID]

SELECT	* 
FROM [AG-ALB].[dbo].[CORE_PurchaseOrderItems]
WHERE [PurchaseOrderID] between 104618 and 104621

SELECT B.[Vendor], A.VendorName,A. [VendorContact],A. [VendorID], COUNT (A. [VendorID])
FROM	[AG-ALB].[dbo].[CORE_PurchaseOrders]A
JOIN	[AG-ALB].[dbo].[PARTS]B
ON B.[VENDOR]=A.[VendorName]
GROUP BY A.VendorID ,B.[Vendor], A.VendorName,A. [VendorContact]
--DROP PROCEDURE IF EXISTS  TEST_PROC_CCUR_Log_Tables 	

CREATE PROC		TEST_PROC_CCUR_Log_Tables 

		AS
			BEGIN 
			
				(		
				
							SELECT						 A.[PurchaseOrderItemID],A.[PurchaseOrderID],A.[QtyOrdered] , A.[PricePerUnit]
														,A.[LineAmount],A.[AG_EQ], A.[AcctID_Account],A.[MinStockLevel]
														,A.[AcctID_Department],A.[Manufacturer],A.[noun],A.[PartNumber],A.[DateRecd]
														,B.[PONumber],B.[DateofOrder], B.[ETA], B.[Status],B.[Total],B.[OrderedBy],B.[VendorID],B.Subtotal
														,B.[Tax],B.[Freight], B.[HistoricalData], B.[ShipToAddress],B.[DateWanted]

							INTO						#TEST_CCUR_PerchaseOrderI_O

							FROM						[AG-ALB].[dbo].[CORE_PurchaseOrderItems] A
							JOIN						[AG-ALB].[dbo].[CORE_PurchaseOrders] B
							ON							A.PurchaseOrderID=B.PurchaseOrderId

							UNION  
							SELECT						C.[PurchaseOrderItemID],C.[PurchaseOrderID],C.[QtyOrdered] , C.[PricePerUnit]
														,C.[LineAmount],C.[AG_EQ], C.[AcctID_Account],C.[MinStockLevel]
														,C.[AcctID_Department],C.[Manufacturer],C.[noun],C.[PartNumber],C.[DateRecd]
														,D.[PONumber],D.[DateofOrder], D.[ETA], D.[Status],D.[Total],D.[OrderedBy],D.[VendorID],D.Subtotal
														,D.[Tax],D.[Freight],D.[HistoricalData], D.[ShipToAddress],D.[DateWanted] 

							FROM						[AG-DUK].[dbo].[CORE_PurchaseOrderItems] C
							JOIN						[AG-DUK].[dbo].[CORE_PurchaseOrders] D
							ON							C.PurchaseOrderID =D.PurchaseOrderId

							UNION

							SELECT						E.[PurchaseOrderItemID],E.[PurchaseOrderID],E.[QtyOrdered], E.[PricePerUnit]
														,E.[LineAmount],E.[AG_EQ], E.[AcctID_Account],E.[MinStockLevel]
														,E.[AcctID_Department],E.[Manufacturer],E.[noun],E.[PartNumber],E.[DateRecd]
														,F.[PONumber],F.[DateofOrder], F.[ETA], F.[Status],F.[Total],F.[OrderedBy],F.[VendorID],F.Subtotal
														,F.[Tax],F.[Freight], F.[HistoricalData], F.[ShipToAddress],F.[DateWanted]

							FROM						[AG-EAG].[dbo].[CORE_PurchaseOrderItems] E
							JOIN						[AG-EAG].[dbo].[CORE_PurchaseOrders] F
							ON							E.PurchaseOrderID=F.PurchaseOrderId

							UNION 

							SELECT						G.[PurchaseOrderItemID],G.[PurchaseOrderID],G.[QtyOrdered], G.[PricePerUnit]
														,G.[LineAmount],G.[AG_EQ], G.[AcctID_Account],G.[MinStockLevel]
														,G.[AcctID_Department],G.[Manufacturer],G.[noun],G.[PartNumber],G.[DateRecd]
														,H.[PONumber],H.[DateofOrder], H.[ETA], H.[Status],H.[Total],H.[OrderedBy],H.[VendorID],H.Subtotal
														, H.[Tax],H.[Freight], H.[HistoricalData], H.[ShipToAddress],H.[DateWanted]

							FROM						[AG-GEO].[dbo].[CORE_PurchaseOrderItems] G
							JOIN						[AG-GEO].[dbo].[CORE_PurchaseOrders] H
							ON							G.PurchaseOrderID =H.PurchaseOrderId

			)

				SELECT		* 
				FROM		#TEST_CCUR_PerchaseOrderI_O
			
			--DROP TABLE  #TEST_CCUR_Log
	--END 

 --EXEC TEST_PROC_CCUR_Log_Tables

SELECT		 [PurchaseOrderItemID],[PurchaseOrderID],[QtyOrdered],[PricePerUnit], AcctID_Account,AcctID_Department,VendorID, PartNumber, AG_EQ ---add VendorId FROM Parts 
			,[LineAmount],[Total],[Subtotal],[Tax],[Freight]
FROM		#TEST_CCUR_Log

-----------------------------////------------------------------------///----------------------------

SELECT			[PurchaseOrderID], [PONumber], [VendorName],[VendorID], [DateofOrder], [ETA], [Status], [Total], [OrderedBy], [DateWanted]
			,	[Terms], [HistoricalData], [ShipToAddress]

			
		INTO		#TEST_CORE_PurchaseOrders

FROM			[AG-ALB].[dbo].[CORE_PurchaseOrders]

UNION
SELECT			[PurchaseOrderID], [PONumber], [VendorName],[VendorID], [DateofOrder], [ETA], [Status], [Total], [OrderedBy], [DateWanted],
				[Terms], [HistoricalData], [ShipToAddress]
FROM			[AG-DUK].[dbo].[CORE_PurchaseOrders]
UNION
SELECT			[PurchaseOrderID], [PONumber], [VendorName],[VendorID], [DateofOrder], [ETA], [Status], [Total], [OrderedBy], [DateWanted]
				,[Terms], [HistoricalData], [ShipToAddress]

FROM			[AG-EAG].[dbo].[CORE_PurchaseOrders]
UNION
SELECT			[PurchaseOrderID], [PONumber], [VendorName],[VendorID], [DateofOrder], [ETA], [Status], [Total], [OrderedBy], [DateWanted]
				,[Terms], [HistoricalData], [ShipToAddress]

FROM			[AG-GEO].[dbo].[CORE_PurchaseOrders]
UNION
SELECT			[PurchaseOrderID], [PONumber], [VendorName],[VendorID], [DateofOrder], [ETA], [Status], [Total], [OrderedBy], [DateWanted]
			  , [Terms], [HistoricalData], [ShipToAddress]
FROM			[AG-ALB].[dbo].[CORE_PurchaseOrders]


		SELECT			*		
		FROM			#TEST_CORE_PurchaseOrders

			
--SELECT* FROM [AG-ALB].[dbo].[CORE_PurchaseOrderItems]
--SELECT* FROM [AG-ALB].[dbo].[CORE_PurchaseOrders]

--SELECT * FROM [AG-ALB].[dbo].[CORE_CostCodes_Block1]
--ORDER BY CostCode_1_ID
--SELECT* FROM [AG-ALB].[dbo].[CORE_CostCodes_Block2]
--ORDER BY CostCode_2_ID
--SELECT * FROM [AG-ALB].[dbo].[Core_CostCodes_Block3]
--SELECT * FROM [AG-ALB].[dbo].[CORE_ShippingAddresses]
--SELECT* FROM [AG-ALB].[dbo].[CORE_SystemSettings]
--SELECT* FROM [AG-ALB].[dbo].[CORE_Terms]
--SELECT* FROM [AG-ALB].[dbo].[CORE_Users]

				SELECT PART_NO, NOUN,VENDOR,MANUF,[LOCATION],APPLICATIO
				FROM [AG-ALB].[dbo].[PARTS] 
				--where [location] is not null and [location] <> ''
				UNION
				SELECT PART_NO, NOUN,VENDOR,MANUF,[LOCATION],APPLICATIO 
				FROM  [AG-DUK].[dbo].[PARTS]
				--where [location] is not null and [location] <> ''
				Union
				SELECT PART_NO, NOUN,VENDOR,MANUF,[LOCATION],APPLICATIO 
				FROM [AG-EAG].[dbo].[PARTS]
				--where [location] is not null and [location] <> ''
				UNION 
				SELECT PART_NO, NOUN,VENDOR,MANUF,[LOCATION],APPLICATIO 
				FROM [AG-GEO].[dbo].[PARTS]
				--where [location] is not null and [location] <> ''

SELECT* FROM [AG-BRN].[dbo].[PARTS]
