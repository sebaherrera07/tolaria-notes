---
type: Note
---
# TEMP

<http://localhost:3000/vehicle_owner/stations/89>

<http://localhost:3000/vehicle_owner/bookings/new?charging_device_id=110>

BD495C825685D98F

1. Download My Teison from Apple Store or Google Play, select the home version above, register and then login in to app (Turn on Bluetooth and location)
2. Select a charging device, then click the thre dots ... in the upper right corner, then enter the distribution network config.
3. Select a 2.4GHz WiFi and enter the correct WiFi password
4. Check websocket option
5. **Enter** socketUrl: ocpp.ingeacarcharge.biz:443/ocpp
6. Enter socketProt: wss
7. Click Link to router

rails new my_app --skip-active-record --css=tailwind --skip-test --skip-system-test

- [e54161c7-a8f6-42aa-a407-d3f2ff541fa0] ⬆ OUT [3425B4F2E77C]: [GetConfiguration] [2,"a9b51a37-ccf0-4cc5-814b-1c1ac0a204e2","GetConfiguration",{"key":["SupportedFeatureProfiles","AuthorizeRemoteTxRequests","LocalAuthorizeOffline","LocalPreAuthorize","AllowOfflineTxForUnknownId","AuthorizationCacheEnabled","LocalAuthListEnabled","LocalAuthListMaxLength","SendLocalListMaxLength","ReserveConnectorZeroSupported","StopTransactionOnInvalidId"]}]
- 17:56:46[e54161c7-a8f6-42aa-a407-d3f2ff541fa0] ⬆ OUT [3425B4F2E77C]: [ChangeConfiguration] [2,"fb0ced40-e98c-4c8d-a793-4afa0baaf6d5","ChangeConfiguration",{"key":"AuthorizeRemoteTxRequests","value":"true"}]
- 17:56:46[e54161c7-a8f6-42aa-a407-d3f2ff541fa0] ⬆ OUT [3425B4F2E77C]: [ChangeConfiguration] [2,"a428cf6f-7857-4ef0-8b50-e216f221a099","ChangeConfiguration",{"key":"LocalPreAuthorize","value":"false"}]
- 17:56:46[e54161c7-a8f6-42aa-a407-d3f2ff541fa0] ⬆ OUT [3425B4F2E77C]: [ChangeConfiguration] [2,"14e920e3-01f8-4bfd-95af-1d97e0f8581b","ChangeConfiguration",{"key":"AllowOfflineTxForUnknownId","value":"false"}]
- 17:56:46[e54161c7-a8f6-42aa-a407-d3f2ff541fa0] ⬆ OUT [3425B4F2E77C]: [ChangeConfiguration] [2,"1c0aea85-3356-4c8a-af72-680abb7caf52","ChangeConfiguration",{"key":"StopTransactionOnInvalidId","value":"true"}]
- ⬇ IN [3425B4F2E77C]: [3,"a9b51a37-ccf0-4cc5-814b-1c1ac0a204e2",{"configurationKey":[{"key":"SupportedFeatureProfiles","readonly":true,"value":"Core,FirmwareManagement,LocalAuthListManagement,RemoteTrigger"},{"key":"AuthorizeRemoteTxRequests","readonly":false,"value":"false"},{"key":"LocalAuthorizeOffline","readonly":false,"value":"true"},{"key":"LocalPreAuthorize","readonly":false,"value":"true"},{"key":"AllowOfflineTxForUnknownId","readonly":false,"value":"false"},{"key":"AuthorizationCacheEnabled","readonly":false,"value":"true"},{"key":"LocalAuthListEnabled","readonly":false,"value":"false"},{"key":"LocalAuthListMaxLength","readonly":true,"value":"100"},{"key":"SendLocalListMaxLength","readonly":true,"value":"100"},{"key":"StopTransactionOnInvalidId","readonly":false,"value":"true"}],"unknownKey":["ReserveConnectorZeroSupported"]}]
- 17:56:58 ⬇ RESP [a9b51a37-ccf0-4cc5-814b-1c1ac0a204e2]: {"configurationKey" => [{"key" => "SupportedFeatureProfiles", "readonly" => true, "value" => "Core,FirmwareManagement,LocalAuthListManagement,RemoteTrigger"}, {"key" => "AuthorizeRemoteTxRequests", "readonly" => false, "value" => "false"}, {"key" => "LocalAuthorizeOffline", "readonly" => false, "value" => "true"}, {"key" => "LocalPreAuthorize", "readonly" => false, "value" => "true"}, {"key" => "AllowOfflineTxForUnknownId", "readonly" => false, "value" => "false"}, {"key" => "AuthorizationCacheEnabled", "readonly" => false, "value" => "true"}, {"key" => "LocalAuthListEnabled", "readonly" => false, "value" => "false"}, {"key" => "LocalAuthListMaxLength", "readonly" => true, "value" => "100"}, {"key" => "SendLocalListMaxLength", "readonly" => true, "value" => "100"}, {"key" => "StopTransactionOnInvalidId", "readonly" => false, "value" => "true"}], "unknownKey" => ["ReserveConnectorZeroSupported"]}
- 17:57:02 ⬇ IN [3425B4F2E77C]: [3,"fb0ced40-e98c-4c8d-a793-4afa0baaf6d5",{"status":"Accepted"}]
- 17:57:02 ⬇ RESP [fb0ced40-e98c-4c8d-a793-4afa0baaf6d5]: {"status" => "Accepted"}
- 17:57:07 ⬇ IN [3425B4F2E77C]: [3,"a428cf6f-7857-4ef0-8b50-e216f221a099",{"status":"Accepted"}]
- 17:57:07 ⬇ RESP [a428cf6f-7857-4ef0-8b50-e216f221a099]: {"status" => "Accepted"}
- ⬇ IN [3425B4F2E77C]: [3,"14e920e3-01f8-4bfd-95af-1d97e0f8581b",{"status":"Accepted"}]
- 17:57:11 ⬇ RESP [14e920e3-01f8-4bfd-95af-1d97e0f8581b]: {"status" => "Accepted"}
- 17:57:15 ⬇ IN [3425B4F2E77C]: [3,"1c0aea85-3356-4c8a-af72-680abb7caf52",{"status":"Accepted"}]
- 17:57:15 ⬇ RESP [1c0aea85-3356-4c8a-af72-680abb7caf52]: {"status" => "Accepted"}
