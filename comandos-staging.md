---
type: CarCharge
_organized: true
---
# Comandos Staging

```ruby
station_ingea_ciudad_costa = Station.find_by(name: "INGEA Ciudad de la Costa")
device_ingea_ciudad_costa = ChargingDevice.find_by(identifier: "3425B4F20142")
charging_point_ingea_ciudad_costa = ChargingPoint.find_by(public_label: "INGEA_CDC_WEMOB_01")


station_ingea_ciudad_costa.reload.status

station_ingea_ciudad_costa.reload.health_status

device_ingea_ciudad_costa.reload.status

charging_point_ingea_ciudad_costa.reload.status

station_ingea_ciudad_costa.mark_unavailable!
device_ingea_ciudad_costa.reload.mark_unavailable!
charging_point_ingea_ciudad_costa.mark_unavailable!
```

```text
```
