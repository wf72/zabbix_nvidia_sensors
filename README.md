# NVidia Sensors

## Overview

This template integrates NVidia SMI for multiple graphics card with Zabbix.


The template adds monitoring of:


    * GPU Utilisation
    * GPU Memory Utilisation
    * GPU Power Consumption
    * GPU Memory (Used, Free, Total)
    * GPU Temperature
    * GPU Memory Temperature
    * GPU Fan Speed


The following agent parameters can be used to add the metrics into Zabbix.

```bash
UserParameter=gpu.discovery,t=1;echo -n "[";for i in `nvidia-smi --query-gpu=uuid --format=csv,nounits,noheader`;do if [ $t -eq 0 ]; then echo ",";else t=0;fi; echo -n "{\"{#UUID}\":\"$i\", \"{#INDEX}\":\"`nvidia-smi --query-gpu=index --format=csv,nounits,noheader --id=$i`\", \"{#NAME}\":\"`nvidia-smi --query-gpu=name --format=csv,nounits,noheader --id=$i`\"}";done; echo "]"
UserParameter=gpu.fanspeed[*],nvidia-smi --query-gpu=fan.speed --format=csv,noheader,nounits -i $1 | tr -d "\n"
UserParameter=gpu.power[*],nvidia-smi --query-gpu=power.draw --format=csv,noheader,nounits -i $1 | tr -d "\n"
UserParameter=gpu.temp.gpu[*],nvidia-smi --query-gpu=temperature.gpu --format=csv,noheader,nounits -i $1 | tr -d "\n"
UserParameter=gpu.temp.memory[*],nvidia-smi --query-gpu=temperature.memory --format=csv,noheader,nounits -i $1 | tr -d "\n"
UserParameter=gpu.utilization.gpu[*],nvidia-smi --query-gpu=utilization.gpu --format=csv,noheader,nounits -i $1 | tr -d "\n"
UserParameter=gpu.utilization.memory[*],nvidia-smi --query-gpu=utilization.memory --format=csv,noheader,nounits -i $1 | tr -d "\n"
UserParameter=gpu.memfree[*],nvidia-smi --query-gpu=memory.free --format=csv,noheader,nounits -i $1 | tr -d "\n"
UserParameter=gpu.memused[*],nvidia-smi --query-gpu=memory.used --format=csv,noheader,nounits -i $1 | tr -d "\n"
UserParameter=gpu.memtotal[*],nvidia-smi --query-gpu=memory.total --format=csv,noheader,nounits -i $1 | tr -d "\n"
UserParameter=gpu.persistence_mode[*],nvidia-smi --query-gpu=persistence_mode --format=csv,noheader,nounits -i $1 | tr -d "\n"
UserParameter=gpu.display_active[*],nvidia-smi --query-gpu=display_active --format=csv,noheader,nounits -i $1 | tr -d "\n"
UserParameter=gpu.driver_version[*],nvidia-smi --query-gpu=driver_version --format=csv,noheader,nounits -i $1 | tr -d "\n"
UserParameter=gpu.gpu_bus_id[*],nvidia-smi --query-gpu=gpu_bus_id --format=csv,noheader,nounits -i $1 | tr -d "\n"
```


## Author
Based on https://github.com/zabbix/community-templates/tree/main/Server_Hardware/Other/template_nvidia-smi_integration/5.0 writed by Richard Kavanagh
Updated by Dorofeev Andrei, added gpu low level discovery

## Macros used

{$TEMP_CRIT} - Disaster temperature trigger level
{$TEMP_WARN} - Warn tempreture trigger level

## Template links

There are no template links in this template.

## Discovery rules

There are no discovery rules in this template.

## Items collected

|Name|Description|Type|Key and additional info|
|----|-----------|----|----|
|GPU Temperature|<p>-</p>|`Zabbix agent`|gpu.temp.gpu<p>Update: 1m</p>|
|GPU Memory Temperature|<p>-</p>|`Zabbix agent`|gpu.temp.memory<p>Update: 1m</p>|
|GPU Bus Id|<p>-</p>|`Zabbix agent`|gpu.gpu_bus_id<p>Update: 1h</p>|
|GPU Display Active|<p>-</p>|`Zabbix agent`|gpu.display_active<p>Update: 1m</p>|
|GPU Driver Version|<p>-</p>|`Zabbix agent`|gpu.driver_version<p>Update: 1h</p>|
|GPU Used Memory|<p>-</p>|`Zabbix agent`|gpu.used<p>Update: 1m</p>|
|GPU Power|<p>-</p>|`Zabbix agent`|gpu.power<p>Update: 1m</p>|
|GPU Fan Speed|<p>-</p>|`Zabbix agent`|gpu.fanspeed<p>Update: 1m</p>|
|GPU Free Memory|<p>-</p>|`Zabbix agent`|gpu.memfree<p>Update: 1m</p>|
|GPU Total Memory|<p>-</p>|`Zabbix agent`|gpu.memtotal<p>Update: 1h</p>|
|GPU Utilisation|<p>-</p>|`Zabbix agent`|gpu.utilisation.gpu<p>Update: 1m</p>|
|GPU Memory Utilisation|<p>-</p>|`Zabbix agent`|gpu.utilisation.memory<p>Update: 1m</p>|


## Triggers

{#NAME} {#INDEX} Temperature over {$TEMP_CRIT} {HOSTNAME} - Critical temperature
{#NAME} {#INDEX} Temperature over {$TEMP_WARN} {HOSTNAME} - Warn tempreture

