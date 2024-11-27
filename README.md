# AzureBicep

*********AppServicePlan.bicep**********

resource azbicepas 'Microsoft.Web/sites@2018-11-01' = {
name: 'azbicep-dev-eus-wapp1'
location: resourceGroup().location
properties: {
serverFarmId: resourceId('Microsoft.Web/serverfarms', 'azbicep-dev-eus-asp1')
	}
    }
resource azbicepappinsights 'Microsoft. Insights/components@2020-02-02-preview' = {
name:azbicep-dev-eus-wapp1-ai'
location: resourceGroup().location
kind: 'web'
properties: {
Application_Type: 'web'
}
}


az deployment group create -g azbicep_dev_eus_rg1 -f 2.AppServicePlan.bicep

*********configure Application settings AppServicePlan.bicep**********

resource azbicepas 'Microsoft.Web/sites@2018-11-01' = {
name: 'azbicep-dev-eus-wapp1'
location: resourceGroup().location
properties: {
serverFarmId: resourceId('Microsoft.Web/serverfarms', 'azbicep-dev-eus-asp1')
	}
    }

resource azbicepwebapplappsetting 'Microsoft.Web/sites/config@2021-01-15' = {
name: 'web'
parent : azbicepas
properties: {
appSettings: [
  {
	name: 'key1'
	value: 'value1'
  }
   {
	name: 'key2'
	value: 'value2'	
  }
]
}
}

resource azbicepappinsights 'Microsoft. Insights/components@2020-02-02-preview' = {
name:azbicep-dev-eus-wapp1-ai'
location: resourceGroup().location
kind: 'web'
properties: {
Application_Type: 'web'
}
}

az deployment group create -g azbicep_dev_eus_rg1 -f 2.AppServicePlan.bicep



*******************Integrate Application insight using instrumentation key with app service****** 



resource azbicepas 'Microsoft.Web/sites@2018-11-01' = {
name: 'azbicep-dev-eus-wapp1'
location: resourceGroup().location
properties: {
serverFarmId: resourceId('Microsoft.Web/serverfarms', 'azbicep-dev-eus-asp1')
	}
    }

resource azbicepwebapplappsetting 'Microsoft.Web/sites/config@2021-01-15' = {
name: 'web'
parent : azbicepas
properties: {
appSettings: [
  {
	name: 'APPINSIGHTS_INSTRUMENTATIONKEY'
	value: 'azbicepinsights.properties.InstrumentationKey'
  }
   {
	name: 'key2'
	value: 'value2'	
  }
]
}
}

resource azbicepappinsights 'Microsoft. Insights/components@2020-02-02-preview' = {
name:azbicep-dev-eus-wapp1-ai'
location: resourceGroup().location
kind: 'web'
properties: {
Application_Type: 'web'
}
}

az deployment group create -g azbicep_dev_eus_rg1 -f 2.AppServicePlan.bicep
 
**************sqldatabase.bicep

resource sqlServer 'Microsoft.Sql/servers@2014-04-01' ={
name: 'azbicep-dev-eus-sqlserver'
location: resourceGroup().location
properties:{
administratorLogin:'sqladmin'
administartorLoginPassword:'Test@1234567'
}
resource sqlServerDatabase 'Microsoft. Sql/servers/databases@2014-04-01' = {
parent: sqlServer
name: 'databasel'
location: resourceGroup().location
properties: {
collation: 'SQL_Latin1_General_CP1_CI_AS'
edition: 'Basic'
maxSizeBytes: '34359738368'
requestedServiceObjectiveName: 'Basic'
}
}

az deployment group create -g azbicep_dev_eus_rg1 -f 3.SQlDatabase.bicep


***************whitelist ip address for azure sql database*********


resource sqlServer 'Microsoft.Sql/servers@2014-04-01' ={
name: 'azbicep-dev-eus-sqlserver'
location: resourceGroup().location
properties:{
administratorLogin:'sqladmin'
administartorLoginPassword:'Test@1234567'
}

resource sqlServerFirewallRules 'Microsoft. Sql/servers/firewallRules@2020-11-01-preview' ={
parent: sqlServer
name: 'Praveen IP Address'
properties: {
	startIpAddress: '1.1.1.1'
	endIpAddress: '1.1.1.1'
I}
}

resource sqlServerDatabase 'Microsoft. Sql/servers/databases@2014-04-01' = {
parent: sqlServer
name: 'databasel'
location: resourceGroup().location
properties: {
collation: 'SQL_Latin1_General_CP1_CI_AS'
edition: 'Basic'
maxSizeBytes: '34359738368'
requestedServiceObjectiveName: 'Basic'
}
}

az deployment group create -g azbicep_dev_eus_rg1 -f 3.SQlDatabase.bicep

