# bacnet-scanner

A Jupyter Notebook using the BAC0 library for BACnet scanning tests.

## Pre-requisites installation

```
python3 -m pip install -r requirements.txt
```

## Example use and output

```
python3 -m jupyterlab
```

```python
import BAC0
from pprint import pprint
```


```python
bacnet = BAC0.connect()
```

    2022-03-27 19:22:50,580 - INFO    | Starting BAC0 version 21.12.03 (Complete)
    2022-03-27 19:22:50,584 - INFO    | Use BAC0.log_level to adjust verbosity of the app.
    2022-03-27 19:22:50,585 - INFO    | Ex. BAC0.log_level('silence') or BAC0.log_level('error')
    2022-03-27 19:22:50,586 - INFO    | Starting TaskManager
    2022-03-27 19:22:50,696 - INFO    | Using ip : 192.168.1.152
    2022-03-27 19:22:50,819 - INFO    | Starting app...
    2022-03-27 19:22:50,820 - INFO    | BAC0 started
    2022-03-27 19:22:50,821 - INFO    | Registered as Simple BACnet/IP App
    2022-03-27 19:22:50,847 - INFO    | Update Local COV Task started
    2022-03-27 19:22:52,071 - INFO    | Server started : http://192.168.1.152:8111
     * Serving Flask app 'BAC0.web.FlaskServer' (lazy loading)
     * Environment: production
    [31m   WARNING: This is a development server. Do not use it in a production deployment.[0m
    [2m   Use a production WSGI server instead.[0m
     * Debug mode: off
    

     * Running on all addresses.
       WARNING: This is a development server. Do not use it in a production deployment.
     * Running on http://192.168.1.152:8111/ (Press CTRL+C to quit)
    

    2022-03-27 19:22:53,070 - INFO    | 192.168.1.173 network number is 39999
    2022-03-27 19:22:53,086 - INFO    | 192.168.1.155 network number is 39999
    2022-03-27 19:22:56,118 - INFO    | Found those networks : {39999}
    2022-03-27 19:22:56,119 - INFO    | Discovering network 39999
    


```python
bacnet.discover()
```

    2022-03-27 19:22:59,375 - INFO    | 192.168.1.173 network number is 39999
    2022-03-27 19:22:59,391 - INFO    | 192.168.1.155 network number is 39999
    2022-03-27 19:23:03,220 - INFO    | Found those networks : {39999}
    2022-03-27 19:23:03,221 - INFO    | Discovering network 39999
    




    [('192.168.1.173', 4114459),
     ('192.168.1.128', 10),
     ('192.168.1.155', 1),
     ('192.168.1.141', 1141)]




```python
pprint(bacnet.devices)
```

                                  Manufacturer        Address   Device ID
    Name                                                                 
    sauter-as                           SAUTER  192.168.1.128          10
    ECY-S1000-EA0633    Distech Controls, Inc.  192.168.1.141        1141
    FC-102-1                Danfoss Drives A/S  192.168.1.155           1
    O3-DIN-CPU 4114459     Delta Controls Inc.  192.168.1.173     4114459
    


```python
# df = bacnet.devices
# df.columns = [c.replace(' ', '_') for c in df.columns]
# pprint(df)
devices = []
for index, row in bacnet.devices.iterrows():
    d_manufacturer = row['Manufacturer']
    d_address = row['Address']
    d_device_id = row[' Device ID'] # yes, there is a whitespace before Device ID
    print('Connecting device',d_manufacturer,index,d_address,d_device_id)
    d = BAC0.device(address=d_address,device_id=d_device_id,network=bacnet)
    devices.append(d)
print(devices)
```

    Connecting device SAUTER sauter-as 192.168.1.128 10
    2022-03-27 19:23:11,001 - INFO    | Changing device state to DeviceDisconnected'>
    2022-03-27 19:23:11,120 - INFO    | Changing device state to RPMDeviceConnected'>
    2022-03-27 19:23:11,304 - INFO    | Device 10:[sauter-as] found... building points list
    2022-03-27 19:23:11,368 - INFO    | Ready!
    2022-03-27 19:23:11,369 - INFO    | Device defined for normal polling with a delay of 10sec
    2022-03-27 19:23:11,369 - INFO    | Polling started, values read every 10 seconds
    Connecting device Distech Controls, Inc. ECY-S1000-EA0633 192.168.1.141 1141
    2022-03-27 19:23:11,372 - INFO    | Changing device state to DeviceDisconnected'>
    2022-03-27 19:23:11,490 - INFO    | Changing device state to RPMDeviceConnected'>
    2022-03-27 19:23:11,680 - INFO    | Device 1141:[ECY-S1000-EA0633] found... building points list
    2022-03-27 19:23:25,793 - INFO    | Ready!
    2022-03-27 19:23:25,794 - INFO    | Device defined for normal polling with a delay of 10sec
    2022-03-27 19:23:25,795 - INFO    | Polling started, values read every 10 seconds
    Connecting device Danfoss Drives A/S FC-102-1 192.168.1.155 1
    2022-03-27 19:23:25,797 - INFO    | Changing device state to DeviceDisconnected'>
    2022-03-27 19:23:26,170 - INFO    | Changing device state to RPMDeviceConnected'>
    2022-03-27 19:23:26,353 - INFO    | Device 1:[FC-102-1] found... building points list
    2022-03-27 19:23:29,763 - INFO    | Ready!
    2022-03-27 19:23:29,765 - INFO    | Device defined for normal polling with a delay of 10sec
    2022-03-27 19:23:29,767 - INFO    | Polling started, values read every 10 seconds
    Connecting device Delta Controls Inc. O3-DIN-CPU 4114459 192.168.1.173 4114459
    2022-03-27 19:23:29,769 - INFO    | Changing device state to DeviceDisconnected'>
    2022-03-27 19:23:29,885 - INFO    | Changing device state to RPMDeviceConnected'>
    2022-03-27 19:23:30,131 - INFO    | Device 4114459:[O3-DIN-CPU 4114459] found... building points list
    2022-03-27 19:23:31,227 - INFO    | Ready!
    2022-03-27 19:23:31,228 - INFO    | Device defined for normal polling with a delay of 10sec
    2022-03-27 19:23:31,229 - INFO    | Polling started, values read every 10 seconds
    [sauter-as / Connected, ECY-S1000-EA0633 / Connected, FC-102-1 / Connected, O3-DIN-CPU 4114459 / Connected]
    


```python
for device in devices:
    try:
        print(device.bacnet_properties['objectName'])
    except:
        pass
    try:        
        print(device.points)
    except:
        pass
    try:
        print(device.bacnet_properties)
    except:
        pass
    print(10*'-')

```

    sauter-as
    []
    {'apduSegmentTimeout': 2000, 'apduTimeout': 3000, 'applicationSoftwareVersion': '2.22', 'daylightSavingsStatus': True, 'description': '', 'deviceAddressBinding': [], 'firmwareRevision': 'V4.0.2b1740', 'localDate': (122, 3, 27, 7), 'localTime': (20, 23, 2, 70), 'location': '', 'maxApduLengthAccepted': 1476, 'modelName': 'EY-RC504F202', 'numberOfApduRetries': 5, 'objectIdentifier': ('device', 10), 'objectList': [('device', 10), ('file', 301), ('file', 1000), ('file', 1001), ('file', 1002), ('file', 1003), (384, 1000), (384, 1001), (384, 1008), (384, 1009), (384, 1016), (384, 1017), (384, 1024), (384, 1025), (384, 1500), (384, 1501), (384, 1502), (384, 1503), (384, 1498), (384, 1499), (384, 2000), (384, 2001), (384, 2008), (384, 2009), (384, 2016), (384, 2017), (384, 2024), (384, 2025), (384, 2500), (384, 2501), (384, 2502), (384, 2503), (384, 2498), (384, 2499), ('program', 1), ('positiveIntegerValue', 1), ('positiveIntegerValue', 2)], 'objectName': 'sauter-as', 'objectType': 'device', 'protocolObjectTypesSupported': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0], 'protocolServicesSupported': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 1, 1, 1, 1, 1, 0, 1, 1, 0], 'protocolVersion': 1, 'segmentationSupported': 'segmentedBoth', 'systemStatus': 'operational', 'utcOffset': -60, 'vendorIdentifier': 80, 'vendorName': 'SAUTER', 'protocolRevision': 16, 'activeCovSubscriptions': [], 'backupFailureTimeout': 300, 'configurationFiles': [], 'databaseRevision': 6, 'lastRestoreTime': <bacpypes.basetypes.TimeStamp object at 0x000002AD55487640>, 'maxSegmentsAccepted': 16, 'profileName': '', 'lastRestartReason': 'detectedPoweredOff', 'restartNotificationRecipients': [<bacpypes.basetypes.Recipient object at 0x000002AD55487A00>], 'timeOfDeviceRestart': <bacpypes.basetypes.TimeStamp object at 0x000002AD554875B0>, 'structuredObjectList': [], 'backupAndRestoreState': 'idle', 'backupPreparationTime': 9, 'restoreCompletionTime': 120, 'restorePreparationTime': 1, 'serialNumber': '411001066022', '@prop_8207': '5c:c2:13:04:9c:bf', '@prop_8208': 'A', '@prop_8252': <bacpypes.constructeddata.Any object at 0x000002AD55479E80>, '@prop_8255': '2.22', '@prop_8277': '411001066022', '@prop_8278': '2116', '@prop_8300': <bacpypes.constructeddata.Any object at 0x000002AD55479400>, '@prop_8301': <bacpypes.constructeddata.Any object at 0x000002AD55475220>, '@prop_8302': <bacpypes.constructeddata.Any object at 0x000002AD55475490>, '@prop_8313': 'N/A', '@prop_8347': 9.0, '@prop_8348': 50.0, '@prop_8351': 0, '@prop_8352': <bacpypes.constructeddata.Any object at 0x000002AD55475D90>, '@prop_8353': <bacpypes.constructeddata.Any object at 0x000002AD55475160>, '@prop_8354': 60, '@prop_8355': 60, '@prop_8356': '', '@prop_8357': 'EY-RC504F202', '@prop_8358': True, '@prop_8380': True}
    ----------
    ECY-S1000-EA0633
    {'protocolRevision': 14, 'databaseRevision': 1618, 'lastRestoreTime': <bacpypes.basetypes.TimeStamp object at 0x000002AD5541E730>, 'activeCovSubscriptions': [], 'configurationFiles': [('file', 1)], 'backupFailureTimeout': 300, 'maxSegmentsAccepted': 255, '@prop_1028': False, 'localDate': (122, 3, 27, 7), 'numberOfApduRetries': 3, 'localTime': (19, 23, 44, 0), 'alignIntervals': True, 'location': '', 'lastRestartReason': 'detectedPowerLost', 'objectIdentifier': ('device', 1141), 'intervalOffset': 0, 'objectList': [('device', 1141), ('file', 1), ('program', 1), ('analogInput', 101), ('analogInput', 102), ('analogInput', 103), ('analogInput', 104), ('analogInput', 105), ('analogInput', 106), ('analogInput', 107), ('analogInput', 108), ('analogInput', 204), ('analogInput', 205), ('analogOutput', 101), ('analogOutput', 102), ('analogOutput', 103), ('analogOutput', 104), ('analogOutput', 105), ('analogOutput', 202), ('analogOutput', 204), ('analogValue', 1), ('analogValue', 2), ('analogValue', 3), ('analogValue', 4), ('analogValue', 5), ('analogValue', 6), ('analogValue', 7), ('analogValue', 8), ('analogValue', 9), ('analogValue', 10), ('analogValue', 11), ('analogValue', 12), ('analogValue', 13), ('analogValue', 14), ('analogValue', 15), ('analogValue', 16), ('analogValue', 17), ('analogValue', 18), ('analogValue', 19), ('analogValue', 20), ('analogValue', 21), ('analogValue', 22), ('analogValue', 23), ('analogValue', 24), ('analogValue', 25), ('analogValue', 26), ('analogValue', 27), ('analogValue', 28), ('analogValue', 29), ('analogValue', 30), ('analogValue', 31), ('analogValue', 32), ('analogValue', 33), ('analogValue', 34), ('analogValue', 35), ('analogValue', 36), ('analogValue', 37), ('analogValue', 38), ('analogValue', 39), ('analogValue', 40), ('analogValue', 41), ('analogValue', 42), ('analogValue', 43), ('analogValue', 44), ('analogValue', 45), ('analogValue', 46), ('analogValue', 47), ('analogValue', 48), ('analogValue', 49), ('analogValue', 50), ('analogValue', 51), ('analogValue', 52), ('analogValue', 53), ('analogValue', 54), ('analogValue', 55), ('analogValue', 56), ('analogValue', 57), ('analogValue', 58), ('analogValue', 59), ('analogValue', 60), ('analogValue', 61), ('analogValue', 62), ('analogValue', 63), ('analogValue', 64), ('analogValue', 65), ('analogValue', 66), ('analogValue', 67), ('analogValue', 68), ('analogValue', 69), ('analogValue', 70), ('analogValue', 71), ('analogValue', 72), ('analogValue', 73), ('analogValue', 74), ('analogValue', 75), ('analogValue', 76), ('analogValue', 77), ('analogValue', 78), ('analogValue', 79), ('analogValue', 80), ('analogValue', 81), ('analogValue', 82), ('analogValue', 83), ('analogValue', 84), ('analogValue', 85), ('analogValue', 86), ('analogValue', 87), ('analogValue', 88), ('analogValue', 89), ('analogValue', 90), ('analogValue', 100), ('analogValue', 101), ('analogValue', 102), ('analogValue', 103), ('analogValue', 104), ('analogValue', 105), ('analogValue', 106), ('analogValue', 107), ('analogValue', 108), ('analogValue', 200), ('analogValue', 201), ('analogValue', 202), ('analogValue', 203), ('analogValue', 204), ('analogValue', 205), ('analogValue', 206), ('analogValue', 207), ('analogValue', 208), ('analogValue', 209), ('analogValue', 210), ('analogValue', 211), ('analogValue', 212), ('analogValue', 213), ('analogValue', 214), ('analogValue', 215), ('analogValue', 216), ('analogValue', 217), ('analogValue', 218), ('analogValue', 219), ('analogValue', 220), ('analogValue', 221), ('analogValue', 222), ('analogValue', 223), ('analogValue', 224), ('analogValue', 225), ('analogValue', 226), ('analogValue', 227), ('analogValue', 228), ('analogValue', 229), ('analogValue', 230), ('analogValue', 231), ('analogValue', 232), ('analogValue', 233), ('analogValue', 234), ('analogValue', 235), ('analogValue', 236), ('analogValue', 237), ('analogValue', 238), ('analogValue', 239), ('analogValue', 240), ('analogValue', 241), ('analogValue', 242), ('analogValue', 243), ('analogValue', 244), ('analogValue', 245), ('analogValue', 246), ('analogValue', 247), ('analogValue', 248), ('analogValue', 249), ('analogValue', 250), ('analogValue', 251), ('analogValue', 252), ('analogValue', 253), ('analogValue', 254), ('analogValue', 255), ('analogValue', 256), ('analogValue', 257), ('analogValue', 258), ('analogValue', 259), ('analogValue', 260), ('analogValue', 261), ('analogValue', 262), ('analogValue', 263), ('analogValue', 264), ('analogValue', 265), ('analogValue', 266), ('analogValue', 267), ('analogValue', 268), ('analogValue', 269), ('analogValue', 270), ('analogValue', 271), ('analogValue', 10001), ('analogValue', 10002), ('analogValue', 10003), ('analogValue', 10011), ('analogValue', 10012), ('analogValue', 10013), ('analogValue', 10021), ('analogValue', 10022), ('analogValue', 10023), ('analogValue', 10031), ('analogValue', 10032), ('analogValue', 10033), ('analogValue', 10041), ('analogValue', 10042), ('analogValue', 10043), ('analogValue', 10051), ('analogValue', 10052), ('analogValue', 10053), ('analogValue', 10061), ('analogValue', 10062), ('analogValue', 10063), ('analogValue', 10071), ('analogValue', 10072), ('analogValue', 10073), ('analogValue', 10081), ('analogValue', 10082), ('analogValue', 10083), ('analogValue', 10091), ('analogValue', 10092), ('analogValue', 10093), ('analogValue', 10101), ('analogValue', 10102), ('analogValue', 10103), ('analogValue', 10111), ('analogValue', 10112), ('analogValue', 10113), ('analogValue', 10121), ('analogValue', 10122), ('analogValue', 10123), ('analogValue', 10131), ('analogValue', 10132), ('analogValue', 10133), ('analogValue', 10141), ('analogValue', 10142), ('analogValue', 10143), ('analogValue', 10151), ('analogValue', 10152), ('analogValue', 10153), ('analogValue', 10161), ('analogValue', 10162), ('analogValue', 10163), ('analogValue', 10171), ('analogValue', 10172), ('analogValue', 10173), ('analogValue', 10181), ('analogValue', 10182), ('analogValue', 10183), ('analogValue', 10191), ('analogValue', 10192), ('analogValue', 10193), ('analogValue', 10201), ('analogValue', 10202), ('analogValue', 10203), ('analogValue', 10211), ('analogValue', 10212), ('analogValue', 10213), ('analogValue', 10221), ('analogValue', 10222), ('analogValue', 10223), ('analogValue', 10231), ('analogValue', 10232), ('analogValue', 10233), ('binaryInput', 201), ('binaryInput', 202), ('binaryInput', 203), ('binaryInput', 206), ('binaryInput', 207), ('binaryInput', 208), ('binaryInput', 301), ('binaryInput', 302), ('binaryInput', 303), ('binaryInput', 304), ('binaryInput', 305), ('binaryInput', 306), ('binaryInput', 307), ('binaryOutput', 201), ('binaryOutput', 203), ('binaryValue', 1), ('binaryValue', 2), ('binaryValue', 3), ('binaryValue', 4), ('binaryValue', 5), ('binaryValue', 6), ('binaryValue', 7), ('binaryValue', 8), ('binaryValue', 9), ('binaryValue', 10), ('binaryValue', 11), ('binaryValue', 12), ('binaryValue', 13), ('binaryValue', 14), ('binaryValue', 15), ('binaryValue', 16), ('binaryValue', 17), ('binaryValue', 18), ('binaryValue', 19), ('binaryValue', 20), ('binaryValue', 21), ('binaryValue', 22), ('binaryValue', 23), ('binaryValue', 24), ('binaryValue', 25), ('binaryValue', 26), ('binaryValue', 100), ('binaryValue', 101), ('binaryValue', 102), ('loop', 1), ('loop', 2), ('loop', 3), ('loop', 4), ('loop', 5), ('loop', 6), ('loop', 7), ('loop', 8), ('loop', 9), ('loop', 10), ('loop', 11), ('loop', 12), ('loop', 13), ('loop', 14), ('loop', 15), ('loop', 16), ('loop', 17), ('loop', 18), ('loop', 19), ('loop', 20), ('loop', 21), ('loop', 22), ('loop', 23), ('loop', 24), ('notificationClass', 1), ('notificationClass', 2), ('schedule', 1), ('schedule', 2), ('multiStateValue', 1), ('multiStateValue', 2), ('multiStateValue', 3), ('multiStateValue', 9), ('trendLog', 100101), ('trendLog', 100102), ('trendLog', 100103), ('trendLog', 100104), ('trendLog', 100105), ('trendLog', 100106), ('trendLog', 100107), ('trendLog', 100108), ('trendLog', 100201), ('trendLog', 100202), ('trendLog', 100203), ('trendLog', 100204), ('trendLog', 100205), ('trendLog', 100206), ('trendLog', 100207), ('trendLog', 100301), ('trendLog', 100302), ('trendLog', 100303), ('trendLog', 100304), ('trendLog', 100305), ('trendLog', 100306), ('trendLog', 100307), ('trendLog', 200101), ('trendLog', 200102), ('trendLog', 200103), ('trendLog', 200104), ('trendLog', 200105), ('trendLog', 200201), ('trendLog', 200202), ('trendLog', 200203), ('trendLog', 200204), ('trendLog', 300018), ('trendLog', 300019), ('trendLog', 300020), ('trendLog', 300021), ('trendLog', 300024), ('trendLog', 300025), ('trendLog', 300026), ('trendLog', 300027), ('trendLog', 300028), ('trendLog', 300029), ('trendLog', 300030), ('trendLog', 300031), ('trendLog', 300032), ('trendLog', 300038), ('trendLog', 300039), ('trendLog', 300040), ('trendLog', 300041), ('trendLog', 300042), ('trendLog', 300043), ('trendLog', 300044), ('trendLog', 300050), ('trendLog', 300051), ('trendLog', 300052), ('trendLog', 300053), ('trendLog', 300054), ('trendLog', 300055), ('trendLog', 300056), ('trendLog', 300062), ('trendLog', 300063), ('trendLog', 300064), ('trendLog', 300065), ('trendLog', 300066), ('trendLog', 300067), ('trendLog', 300068), ('trendLog', 300074), ('trendLog', 300075), ('trendLog', 300076), ('trendLog', 300077), ('trendLog', 300078), ('trendLog', 300079), ('trendLog', 300080), ('trendLog', 300086), ('trendLog', 300087), ('trendLog', 300088), ('trendLog', 300089), ('trendLog', 300090), ('trendLog', 300200), ('trendLog', 300201), ('trendLog', 300206), ('trendLog', 300207), ('trendLog', 300209), ('trendLog', 300210), ('trendLog', 300211), ('trendLog', 300212), ('trendLog', 300218), ('trendLog', 300219), ('trendLog', 300220), ('trendLog', 300221), ('trendLog', 300222), ('trendLog', 300223), ('trendLog', 300229), ('trendLog', 300230), ('trendLog', 300231), ('trendLog', 300232), ('trendLog', 300233), ('trendLog', 300234), ('trendLog', 300240), ('trendLog', 300241), ('trendLog', 300242), ('trendLog', 300243), ('trendLog', 300244), ('trendLog', 300245), ('trendLog', 300251), ('trendLog', 300252), ('trendLog', 300253), ('trendLog', 300254), ('trendLog', 300255), ('trendLog', 300256), ('trendLog', 300262), ('trendLog', 300263), ('trendLog', 300264), ('trendLog', 300265), ('trendLog', 300266), ('trendLog', 300267), ('trendLog', 300268), ('trendLog', 300269), ('trendLog', 300270), ('trendLog', 300271), ('trendLog', 500002), ('trendLog', 500003), ('trendLog', 500009)], 'objectName': 'ECY-S1000-EA0633', 'maxApduLengthAccepted': 1476, '@prop_1019': 5, 'backupAndRestoreState': 'idle', 'maxInfoFrames': 20, 'maxMaster': 127, 'restartNotificationRecipients': [<bacpypes.basetypes.Recipient object at 0x000002AD5541ECA0>], 'timeSynchronizationInterval': 15, 'timeOfDeviceRestart': <bacpypes.basetypes.TimeStamp object at 0x000002AD5541E640>, 'restorePreparationTime': 60, '@prop_1017': 'Europe/London', 'utcTimeSynchronizationRecipients': [], 'restoreCompletionTime': 300, 'backupPreparationTime': 60, 'modelName': 'ECY-S1000 Rev 1.0A', 'firmwareRevision': '1.14.17191.1', 'objectType': 'device', 'daylightSavingsStatus': True, 'deviceAddressBinding': [<bacpypes.basetypes.AddressBinding object at 0x000002AD5541EF40>], 'segmentationSupported': 'segmentedBoth', 'description': '', 'protocolServicesSupported': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 0], 'protocolVersion': 1, 'protocolObjectTypesSupported': [1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 0, 1, 1, 1, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 'apduSegmentTimeout': 5000, 'vendorName': 'Distech Controls, Inc.', 'vendorIdentifier': 364, 'utcOffset': 0, 'applicationSoftwareVersion': '1.9.17338.368', 'apduTimeout': 6000, 'systemStatus': 'operational', 'timeSynchronizationRecipients': []}
    ----------
    FC-102-1
    [FC-102-1/Analog Input 53 : 0.00 percent, FC-102-1/Analog Input 54 : 0.00 percent, FC-102-1/Terminal 42 Output Bus Control : 0.00 percent, FC-102-1/Pulse out #27 Bus Control : 0.00 percent, FC-102-1/Pulse out #29 Bus Control : 0.00 percent, FC-102-1/Speed Act Value : 0.00 percent, FC-102-1/Input Reference 1 : 0.00 percent, FC-102-1/Input Reference 2 : 0.00 percent, FC-102-1/Output Speed : 0.00 percent, FC-102-1/PID Feedback : 0.00 percent, FC-102-1/Motor Current : 0.00 amperes, FC-102-1/Power : 0.00 kilowatts, FC-102-1/Motor Thermal : 0.00 percent, FC-102-1/Operating Hours : 4643.00 hours, FC-102-1/Running Hours : 0.00 hours, FC-102-1/kWh Counter : 0.00 kilowattHours, FC-102-1/Motor Voltage : 0.00 volts, FC-102-1/Frequency : 0.00 hertz, FC-102-1/Torque : 0.00 percent, FC-102-1/DC Link Voltage : 43.00 volts, FC-102-1/Heatsink Temp : 0.00 degreesCelsius, FC-102-1/Inverter Thermal : 0.00 percent, FC-102-1/Setpoint 1 : 0.00 percent, FC-102-1/Bus Feedback 1 : 50.00 percent, FC-102-1/Setpoint 2 : 0.00 percent, FC-102-1/Bus Feedback 2 : 50.00 percent, FC-102-1/Setpoint 3 : 0.00 percent, FC-102-1/Bus Feedback 3 : 50.00 percent, FC-102-1/Alarm Log: Error Code : 80.00 noUnits, FC-102-1/Fault Code : 6.00 noUnits, FC-102-1/PID Start Speed : 0.00 hertz, FC-102-1/On Reference Bandwidth : 2.50 percent, FC-102-1/PID Proportional Gain : 0.50 noUnits, FC-102-1/PID Integral Time : 20.00 seconds, FC-102-1/PID Differentiation Time : 0.00 seconds, FC-102-1/PID Diff Gain Limit : 5.00 noUnits, FC-102-1/Sensorless Readout : 0.00 noUnits, FC-102-1/PID Output : 0.00 percent, FC-102-1/Alarm Word : 0.00 noUnits, FC-102-1/Alarm Word 2 : 0.00 noUnits, FC-102-1/External Reference : 0.00 noUnits, FC-102-1/Warning Word : 4199424.00 noUnits, FC-102-1/Warning Word 2 : 0.00 noUnits, FC-102-1/Warning Word 3 : 0.00 noUnits, FC-102-1/Feedback[Unit] : 0.00 percent, FC-102-1/Air Pressure to Flow Air Flow 2 : 0.00 cubicMetersPerHour, FC-102-1/Ext. Status Word : 6291456.00 noUnits, FC-102-1/Ext. Status Word 2 : 131.00 noUnits, FC-102-1/Digital input Term 33 : False, FC-102-1/Digital input Term 32 : False, FC-102-1/Digital input Term 29 : False, FC-102-1/Digital input Term 27 : False, FC-102-1/Digital input Term 19 : False, FC-102-1/Digital input Term 18 : False, FC-102-1/Digital input Term 37 : True, FC-102-1/Digital Output Term 27 : False, FC-102-1/Digital Output Term 29 : False, FC-102-1/Relay 1 : False, FC-102-1/Relay 2 : False, FC-102-1/RUN/STOP command : False, FC-102-1/REF 1/REF 2 Select : False, FC-102-1/Fault Reset Command : False, FC-102-1/RUN/STOP Monitor : False, FC-102-1/OK/FAULT Monitor : True, FC-102-1/HAND/AUTO Reference : True, FC-102-1/Warning : True, FC-102-1/Trip : False, FC-102-1/Triplock : False, FC-102-1/Coasting : False, FC-102-1/CW/CWW : False, FC-102-1/Jog : False, FC-102-1/Reset : False, FC-102-1/Reset KWh Counter : False, FC-102-1/Reset Running Hours Counter : False, FC-102-1/ReVStatus : False, FC-102-1/Speed = Reference : False, FC-102-1/Bus Control : False, FC-102-1/Running : False, FC-102-1/Ramp 1/Ramp 2 : False, FC-102-1/Smart Logic Controller State : STATE1, FC-102-1/Active Setup : Active Setup 1, FC-102-1/Configuration Mode : Open Loop, FC-102-1/Cascade Status : Disabled, FC-102-1/Pump Status : 1:O 2:O ]
    {'objectIdentifier': ('device', 1), 'objectName': 'FC-102-1', 'objectType': 'device', 'systemStatus': 'operational', 'vendorName': 'Danfoss Drives A/S', 'vendorIdentifier': 190, 'modelName': 'MCA 125', 'firmwareRevision': '2.12', 'applicationSoftwareVersion': '2.12', 'location': '', 'description': '', 'protocolVersion': 1, 'protocolRevision': 14, 'protocolServicesSupported': [1, 0, 0, 1, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 1, 0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 1, 1, 1, 1, 1, 0, 0, 1, 0], 'protocolObjectTypesSupported': [1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 'objectList': [('device', 1), ('trendLog', 0), ('trendLog', 1), ('trendLog', 2), ('trendLog', 3), ('trendLog', 4), ('trendLog', 5), ('trendLog', 6), ('eventLog', 0), ('binaryInput', 0), ('binaryInput', 1), ('binaryInput', 2), ('binaryInput', 3), ('binaryInput', 4), ('binaryInput', 5), ('binaryInput', 6), ('binaryOutput', 0), ('binaryOutput', 1), ('binaryOutput', 4), ('binaryOutput', 5), ('binaryValue', 1), ('binaryValue', 2), ('binaryValue', 3), ('binaryValue', 4), ('binaryValue', 5), ('binaryValue', 6), ('binaryValue', 21), ('binaryValue', 22), ('binaryValue', 23), ('binaryValue', 24), ('binaryValue', 25), ('binaryValue', 26), ('binaryValue', 27), ('binaryValue', 28), ('binaryValue', 29), ('binaryValue', 30), ('binaryValue', 31), ('binaryValue', 32), ('binaryValue', 33), ('binaryValue', 34), ('analogInput', 0), ('analogInput', 1), ('analogOutput', 0), ('analogOutput', 1), ('analogOutput', 2), ('analogValue', 0), ('analogValue', 1), ('analogValue', 2), ('analogValue', 3), ('analogValue', 4), ('analogValue', 5), ('analogValue', 6), ('analogValue', 15), ('analogValue', 21), ('analogValue', 22), ('analogValue', 23), ('analogValue', 24), ('analogValue', 25), ('analogValue', 26), ('analogValue', 27), ('analogValue', 28), ('analogValue', 29), ('analogValue', 30), ('analogValue', 31), ('analogValue', 35), ('analogValue', 36), ('analogValue', 40), ('analogValue', 41), ('analogValue', 50), ('analogValue', 51), ('analogValue', 52), ('analogValue', 53), ('analogValue', 54), ('analogValue', 55), ('analogValue', 56), ('analogValue', 57), ('analogValue', 58), ('analogValue', 59), ('analogValue', 61), ('analogValue', 62), ('analogValue', 65), ('analogValue', 66), ('analogValue', 67), ('analogValue', 68), ('analogValue', 70), ('analogValue', 71), ('analogValue', 74), ('analogValue', 75), ('schedule', 0), ('schedule', 1), ('schedule', 2), ('schedule', 3), ('schedule', 4), ('schedule', 5), ('calendar', 0), ('calendar', 1), ('calendar', 2), ('calendar', 3), ('calendar', 4), ('calendar', 5), ('multiStateValue', 0), ('multiStateValue', 1), ('multiStateValue', 3), ('notificationClass', 0), ('notificationClass', 2), ('notificationClass', 3), ('notificationClass', 100), ('notificationClass', 200), ('notificationClass', 300), ('notificationClass', 400), ('loop', 0), ('eventEnrollment', 0), ('eventEnrollment', 1), ('eventEnrollment', 2), ('eventEnrollment', 3), ('eventEnrollment', 4), ('eventEnrollment', 5), ('eventEnrollment', 6), ('eventEnrollment', 7), ('eventEnrollment', 8), ('eventEnrollment', 9), ('eventEnrollment', 10), ('eventEnrollment', 11), ('eventEnrollment', 12), ('eventEnrollment', 13), ('characterstringValue', 0), ('characterstringValue', 1)], 'maxApduLengthAccepted': 1476, 'segmentationSupported': 'segmentedBoth', 'maxSegmentsAccepted': 2, 'apduSegmentTimeout': 5000, 'apduTimeout': 6000, 'numberOfApduRetries': 3, 'localTime': (18, 23, 42, 65), 'localDate': (122, 3, 27, 7), 'utcOffset': 0, 'daylightSavingsStatus': False, 'deviceAddressBinding': [], 'databaseRevision': 10, 'activeCovSubscriptions': [], 'lastRestartReason': 'detectedPoweredOff', 'timeOfDeviceRestart': <bacpypes.basetypes.TimeStamp object at 0x000002AD553F3A60>, 'restartNotificationRecipients': [], 'serialNumber': '011126G149'}
    ----------
    O3-DIN-CPU 4114459
    [O3-DIN-CPU 4114459/Google Cloud IoT Gateway Configuration : {
    "hostName": "mqtt.googleapis.com",
    "tcpPort": 8883,
    "location": "eu-west1",
    "project": "iot-workshop-2020",
    "registry": "udmi-testing",
    "device": "CGW-4",
    }]
    {'activeVtSessions': [], 'apduSegmentTimeout': 5000, 'apduTimeout': 7000, 'applicationSoftwareVersion': '4.9.0.5003 (490-001)', 'daylightSavingsStatus': False, 'description': '', 'deviceAddressBinding': [], 'firmwareRevision': '6.4.0.O3-DIN.1.246', 'localDate': (100, 2, 25, 5), 'localTime': (5, 29, 1, 0), 'location': '', 'maxApduLengthAccepted': 1476, 'maxInfoFrames': 8, 'maxMaster': 127, 'modelName': 'O3-DIN-CPU', 'numberOfApduRetries': 2, 'objectIdentifier': ('device', 4114459), 'objectList': [('device', 4114459), ('file', 1), ('file', 2), ('file', 3), ('file', 1001), ('file', 1011), ('file', 1012), ('file', 1201), ('file', 1202), ('file', 1203), ('file', 1204), ('file', 1205), ('file', 1206), ('file', 1207), ('file', 1208), ('file', 1209), ('file', 1210), ('file', 1211), ('file', 1212), ('file', 1213), ('file', 1214), ('file', 1215), ('file', 1216), ('file', 1217), ('file', 1218), ('file', 1219), ('file', 1220), ('file', 1221), ('file', 1222), ('file', 1223), ('file', 1224), ('file', 1225), ('file', 1226), ('file', 1227), ('notificationClass', 1), ('notificationClass', 2), ('notificationClass', 3), ('notificationClass', 4), ('notificationClass', 5), ('notificationClass', 6), ('notificationClass', 7), ('notificationClass', 8), ('notificationClass', 9), ('notificationClass', 10), ('notificationClass', 11), ('characterstringValue', 1), ('networkPort', 1), ('networkPort', 2), ('networkPort', 3), ('networkPort', 4), ('networkPort', 5), ('networkPort', 6), ('networkPort', 7), ('networkPort', 8), ('networkPort', 9), (142, 1), (152, 1), (177, 1), (177, 2), (177, 3), (177, 4), (178, 1), (178, 2), (178, 3), (178, 4), (178, 5), (178, 6), (178, 7), (178, 8), (178, 9), (178, 10), (178, 11), (178, 12), (178, 13), (178, 14), (178, 15), (178, 16), (178, 17), (178, 18), (178, 19), (178, 20), (178, 21), (178, 22), (178, 23), (178, 24), (178, 25), (178, 26), (181, 4114459), (266, 1), (323, 1), (329, 1), (330, 1)], 'objectName': 'O3-DIN-CPU 4114459', 'objectType': 'device', 'protocolObjectTypesSupported': [1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1], 'protocolServicesSupported': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 1], 'protocolVersion': 1, 'segmentationSupported': 'segmentedBoth', 'systemStatus': 'operational', 'timeSynchronizationRecipients': [], 'utcOffset': 0, 'vendorIdentifier': 8, 'vendorName': 'Delta Controls Inc.', 'vtClassesSupported': [], 'protocolRevision': 18, 'activeCovSubscriptions': [], 'backupFailureTimeout': 60, 'configurationFiles': [], 'databaseRevision': 596, 'lastRestoreTime': <bacpypes.basetypes.TimeStamp object at 0x000002AD554D8070>, 'maxSegmentsAccepted': 166, 'profileName': '008-ebcon-v490-001-6.4.0.246-device', 'autoSlaveDiscovery': [], 'manualSlaveAddressBinding': [], 'slaveAddressBinding': [], 'slaveProxyEnable': [], 'alignIntervals': True, 'intervalOffset': 0, 'lastRestartReason': 'softwareWatchdog', 'restartNotificationRecipients': [<bacpypes.basetypes.Recipient object at 0x000002AD554BE6D0>], 'timeOfDeviceRestart': <bacpypes.basetypes.TimeStamp object at 0x000002AD554D80A0>, 'timeSynchronizationInterval': 0, 'utcTimeSynchronizationRecipients': [], 'structuredObjectList': None, 'backupAndRestoreState': 'idle', 'backupPreparationTime': 0, 'restoreCompletionTime': 0, 'restorePreparationTime': 0, 'serialNumber': '113006/0423', '@prop_1216': [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], '@prop_2000': 312, '@prop_2001': [0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0], '@prop_2003': False, '@prop_2009': False, '@prop_2050': 0, '@prop_2054': False, '@prop_13001': 4114459, '@prop_13002': 0, '@prop_13011': <bacpypes.constructeddata.Any object at 0x000002AD554D19A0>, '@prop_13012': <bacpypes.constructeddata.Any object at 0x000002AD554D1B20>, '@prop_13016': <bacpypes.constructeddata.Any object at 0x000002AD554D3F70>, '@prop_13017': '1.8a', '@prop_13018': 50, '@prop_13020': <bacpypes.constructeddata.Any object at 0x000002AD554D5670>, '@prop_13021': True, '@prop_13022': 20, '@prop_13023': 5, '@prop_13024': '(none)'}
    ----------
    


```python
device_number = 2
try:
    prop = ('objectName')
    print('device name', devices[device_number].read_property(prop))
except:
    pass
try:
    prop = ('firmwareRevision')
    print('firmware revision', devices[device_number].read_property(prop))
except:
    pass
prop = ('objectList')
object_list = devices[device_number].read_property(prop)
print(object_list)
print(devices[device_number].points)
```

    device name FC-102-1
    firmware revision 2.12
    [('device', 1), ('trendLog', 0), ('trendLog', 1), ('trendLog', 2), ('trendLog', 3), ('trendLog', 4), ('trendLog', 5), ('trendLog', 6), ('eventLog', 0), ('binaryInput', 0), ('binaryInput', 1), ('binaryInput', 2), ('binaryInput', 3), ('binaryInput', 4), ('binaryInput', 5), ('binaryInput', 6), ('binaryOutput', 0), ('binaryOutput', 1), ('binaryOutput', 4), ('binaryOutput', 5), ('binaryValue', 1), ('binaryValue', 2), ('binaryValue', 3), ('binaryValue', 4), ('binaryValue', 5), ('binaryValue', 6), ('binaryValue', 21), ('binaryValue', 22), ('binaryValue', 23), ('binaryValue', 24), ('binaryValue', 25), ('binaryValue', 26), ('binaryValue', 27), ('binaryValue', 28), ('binaryValue', 29), ('binaryValue', 30), ('binaryValue', 31), ('binaryValue', 32), ('binaryValue', 33), ('binaryValue', 34), ('analogInput', 0), ('analogInput', 1), ('analogOutput', 0), ('analogOutput', 1), ('analogOutput', 2), ('analogValue', 0), ('analogValue', 1), ('analogValue', 2), ('analogValue', 3), ('analogValue', 4), ('analogValue', 5), ('analogValue', 6), ('analogValue', 15), ('analogValue', 21), ('analogValue', 22), ('analogValue', 23), ('analogValue', 24), ('analogValue', 25), ('analogValue', 26), ('analogValue', 27), ('analogValue', 28), ('analogValue', 29), ('analogValue', 30), ('analogValue', 31), ('analogValue', 35), ('analogValue', 36), ('analogValue', 40), ('analogValue', 41), ('analogValue', 50), ('analogValue', 51), ('analogValue', 52), ('analogValue', 53), ('analogValue', 54), ('analogValue', 55), ('analogValue', 56), ('analogValue', 57), ('analogValue', 58), ('analogValue', 59), ('analogValue', 61), ('analogValue', 62), ('analogValue', 65), ('analogValue', 66), ('analogValue', 67), ('analogValue', 68), ('analogValue', 70), ('analogValue', 71), ('analogValue', 74), ('analogValue', 75), ('schedule', 0), ('schedule', 1), ('schedule', 2), ('schedule', 3), ('schedule', 4), ('schedule', 5), ('calendar', 0), ('calendar', 1), ('calendar', 2), ('calendar', 3), ('calendar', 4), ('calendar', 5), ('multiStateValue', 0), ('multiStateValue', 1), ('multiStateValue', 3), ('notificationClass', 0), ('notificationClass', 2), ('notificationClass', 3), ('notificationClass', 100), ('notificationClass', 200), ('notificationClass', 300), ('notificationClass', 400), ('loop', 0), ('eventEnrollment', 0), ('eventEnrollment', 1), ('eventEnrollment', 2), ('eventEnrollment', 3), ('eventEnrollment', 4), ('eventEnrollment', 5), ('eventEnrollment', 6), ('eventEnrollment', 7), ('eventEnrollment', 8), ('eventEnrollment', 9), ('eventEnrollment', 10), ('eventEnrollment', 11), ('eventEnrollment', 12), ('eventEnrollment', 13), ('characterstringValue', 0), ('characterstringValue', 1)]
    [FC-102-1/Analog Input 53 : 0.00 percent, FC-102-1/Analog Input 54 : 0.00 percent, FC-102-1/Terminal 42 Output Bus Control : 0.00 percent, FC-102-1/Pulse out #27 Bus Control : 0.00 percent, FC-102-1/Pulse out #29 Bus Control : 0.00 percent, FC-102-1/Speed Act Value : 0.00 percent, FC-102-1/Input Reference 1 : 0.00 percent, FC-102-1/Input Reference 2 : 0.00 percent, FC-102-1/Output Speed : 0.00 percent, FC-102-1/PID Feedback : 0.00 percent, FC-102-1/Motor Current : 0.00 amperes, FC-102-1/Power : 0.00 kilowatts, FC-102-1/Motor Thermal : 0.00 percent, FC-102-1/Operating Hours : 4643.00 hours, FC-102-1/Running Hours : 0.00 hours, FC-102-1/kWh Counter : 0.00 kilowattHours, FC-102-1/Motor Voltage : 0.00 volts, FC-102-1/Frequency : 0.00 hertz, FC-102-1/Torque : 0.00 percent, FC-102-1/DC Link Voltage : 43.00 volts, FC-102-1/Heatsink Temp : 0.00 degreesCelsius, FC-102-1/Inverter Thermal : 0.00 percent, FC-102-1/Setpoint 1 : 0.00 percent, FC-102-1/Bus Feedback 1 : 50.00 percent, FC-102-1/Setpoint 2 : 0.00 percent, FC-102-1/Bus Feedback 2 : 50.00 percent, FC-102-1/Setpoint 3 : 0.00 percent, FC-102-1/Bus Feedback 3 : 50.00 percent, FC-102-1/Alarm Log: Error Code : 80.00 noUnits, FC-102-1/Fault Code : 6.00 noUnits, FC-102-1/PID Start Speed : 0.00 hertz, FC-102-1/On Reference Bandwidth : 2.50 percent, FC-102-1/PID Proportional Gain : 0.50 noUnits, FC-102-1/PID Integral Time : 20.00 seconds, FC-102-1/PID Differentiation Time : 0.00 seconds, FC-102-1/PID Diff Gain Limit : 5.00 noUnits, FC-102-1/Sensorless Readout : 0.00 noUnits, FC-102-1/PID Output : 0.00 percent, FC-102-1/Alarm Word : 0.00 noUnits, FC-102-1/Alarm Word 2 : 0.00 noUnits, FC-102-1/External Reference : 0.00 noUnits, FC-102-1/Warning Word : 4199424.00 noUnits, FC-102-1/Warning Word 2 : 0.00 noUnits, FC-102-1/Warning Word 3 : 0.00 noUnits, FC-102-1/Feedback[Unit] : 0.00 percent, FC-102-1/Air Pressure to Flow Air Flow 2 : 0.00 cubicMetersPerHour, FC-102-1/Ext. Status Word : 6291456.00 noUnits, FC-102-1/Ext. Status Word 2 : 131.00 noUnits, FC-102-1/Digital input Term 33 : False, FC-102-1/Digital input Term 32 : False, FC-102-1/Digital input Term 29 : False, FC-102-1/Digital input Term 27 : False, FC-102-1/Digital input Term 19 : False, FC-102-1/Digital input Term 18 : False, FC-102-1/Digital input Term 37 : True, FC-102-1/Digital Output Term 27 : False, FC-102-1/Digital Output Term 29 : False, FC-102-1/Relay 1 : False, FC-102-1/Relay 2 : False, FC-102-1/RUN/STOP command : False, FC-102-1/REF 1/REF 2 Select : False, FC-102-1/Fault Reset Command : False, FC-102-1/RUN/STOP Monitor : False, FC-102-1/OK/FAULT Monitor : True, FC-102-1/HAND/AUTO Reference : True, FC-102-1/Warning : True, FC-102-1/Trip : False, FC-102-1/Triplock : False, FC-102-1/Coasting : False, FC-102-1/CW/CWW : False, FC-102-1/Jog : False, FC-102-1/Reset : False, FC-102-1/Reset KWh Counter : False, FC-102-1/Reset Running Hours Counter : False, FC-102-1/ReVStatus : False, FC-102-1/Speed = Reference : False, FC-102-1/Bus Control : False, FC-102-1/Running : False, FC-102-1/Ramp 1/Ramp 2 : False, FC-102-1/Smart Logic Controller State : STATE1, FC-102-1/Active Setup : Active Setup 1, FC-102-1/Configuration Mode : Open Loop, FC-102-1/Cascade Status : Disabled, FC-102-1/Pump Status : 1:O 2:O ]
    


```python
print(devices[device_number].points[13])
```

    FC-102-1/Operating Hours : 4643.00 hours
    
