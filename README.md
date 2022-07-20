# CICD_Pipelines

Pipelines code for the following

  1. Deploy-config
  2. Create-congig
  3. Connection-config
  4. Schedule-config




1.   Pipeline for Deploy-config file
-------------------------------------------


{	
	"pipelines": 
		[
			{
				"job": "Deploy Package",
				"processName": "CE_Ticket_PDP",
				"packageVersion": "3.4",
				"notes": "Stage Deployment after email notification update",
				"componentType": "process",
				"componentId": "19532fca-6a1d-4396-a517-89f3af1d592f",
				"env": "GP_AWS_STG_RealTime_Env", 
				"listenerStatus": "RUNNING"
			},
			{
				"job": "Deploy Package",
				"processName": "TICKET_PDP_API",
				"packageVersion": "1.3",
				"componentType": "webservice",
				"componentId": "0ee81735-c783-4958-8ff6-ed0d16040601",
				"env": "GP_AWS_STG_RealTime_Env", 
				"notes": "Stage Deployment after email notification update",
				"listenerStatus": "RUNNING"
			}

		]
}

-------------------------------------------------------------------------------------------

2.  Pipeline File for Create Package Config file


{
	"pipelines": 
		[
			{
				"job": "Create Package",
				"componentId": "19532fca-6a1d-4396-a517-89f3af1d592f",
				"processName": "TicketAccess_PDP_To_Tableau",
				"packageVersion": "5.0",
				"componentType": "process",
				"notes": "Get Details about the Ticket",
				"extractComponentXmlFolder": "TicketAccess_DataLake_To_Tableau",
				"tag": "TRUE",
				"env": "GP_AWS_DEV_Env"
			},
			{
				"job": "Create Package",
				"processName": "Route_PurchaseOrderToSubProcess",
				"packageVersion": "4.0",
				"componentType": "processroute",
				"componentId": "9de14ffa-890d-40e9-8a6e-f49f9856d3a1",
				"notes": "Creating Package for Route_PurchaseOrderToSubProcess",
				"extractComponentXmlFolder": "Route_PurchaseOrderToSubProcess",
				"tag": "TRUE",
				"listenerStatus": "RUNNING",
				"env": "GP_AWS_DEV_Env"
			},
	    		{
				"job": "Create Package",
				"processName": "Route_PurchaseOrderToSubProcess",
				"packageVersion": "4.0",
				"componentType": "processroute",
				"componentId": "9de14ffa-890d-40e9-8a6e-f49f9856d3a1",
				"notes": "Creating Package for Route_PurchaseOrderToSubProcess",
				"extractComponentXmlFolder": "Route_PurchaseOrderToSubProcess",
				"tag": "TRUE",
				"listenerStatus": "RUNNING",
				"env": "GP_AWS_DEV_Env"
			},
			{
			    "job": "Create Package",
			    "componentId": "0ee81735-c783-4958-8ff6-ed0d16040601",
			    "processName": "TICKET_PDP_API",
			    "packageVersion": "2.0",
			    "componentType": "webservice",
			    "notes": "Deploying  TICKET_PDP_API",
			    "extractComponentXmlFolder": "CheckTicketAccess_DataLake_To_Tableau",	
			    "tag": "TRUE",
			    "env": "GP_AWS_DEV_Env"
			}

		]
}

------------------------------------------------------------------------------------------------------

 3. Pipeline File for Connection config file
 
 
 {
	"pipelines": [
		{
			"job": "Update Environment Extensions",
			"env": "GP_AWS_STG_RealTime_Env",
			"key": "arn:aws:secretsmanager:us-east-1:325381443140:secret:Boomi_igr_stg-6BPhRu",
			"extensionJson": {
				"@type": "EnvironmentExtensions",
				"connections": {
					"@type": "Connections",
					"connection": [
						{
							"@type": "Connection",
							"field": [
								{
									"@type": "field",
									"id": "hostname",
									"value": "prodvdsalp.2389.corporate.ge.com",
									"usesEncryption": "false",
									"useDefault": false
								},
								{
									"@type": "field",
									"id": "port",
									"value": "389",
									"usesEncryption": "false",
									"useDefault": false
								},
								{
									"@type": "field",
									"id": "ssl",
									"value": "false",
									"usesEncryption": "false",
									"useDefault": false
								},
								{
									"@type": "field",
									"id": "authType",
									"value": "simple",
									"usesEncryption": "false",
									"useDefault": false
								},
								{
									"@type": "field",
									"id": "principal",
									"value": "uid=chefvdsprod,ou=special users,o=ge.com",
									"usesEncryption": "false",
									"useDefault": false
								},
								{
									"@type": "field",
									"id": "credentials",
									"valueFrom": "hr_ldap_cn_stg_pwd",
									"usesEncryption": "true",
									"useDefault": false
								},
								{
									"@type": "field",
									"id": "referrals",
									"value": "follow",
									"usesEncryption": "false",
									"useDefault": false
								}
							],
							"id": "2253d888-0761-462d-ac97-22235405c519",
							"name": "HR_LDAP_CN"
						}
					]
				},
				"properties": {
					"@type": "",
					"property": []
				},
				"environmentId": "[Update ID]",
				"partial": "true",
				"extensionGroupId": "",
				"id": "[Same as environmentId]"
			}
		}
	]
}

-----------------------------------------------------------------------------------------------------------


 4.  Pipeline File for Schedule-config File
 
 {
    "pipelines": 
	    [
		{
		    "job": "Create Process Schedule",
		    "processName": "CE_Activity_GlobalKronos_To_Smartshop",
		    "atomName": "Batch_STG_Atom",
		    "atomType": "Cloud",
		    "years": "*",
		    "months": "*",
		    "days": "*",
		    "hours": "0-23",
		    "minutes": "0-59\\/5",
		    "daysOfWeek": "*",
		    "daysOfMonth": "*",
		    "env": "GP_AWS_STG_Batch"
		}

	    ]
}
