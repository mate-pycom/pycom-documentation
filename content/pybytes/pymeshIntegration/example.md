## Pymesh example code

After Pymesh provisioning, the devices have the correct Pymesh firmware.

Once Pymesh is provisioned with Pybytes it starts automatically during the Pybytes initialization, but it is not sending any data yet.

Below there is a code example that should be implemented in the main.py file. This example sends data trough the Pymesh every 20 seconds.

```python
import time
import pycom

if pybytes is not None:
    if pybytes.__pymesh:
        pymesh = pybytes.__pymesh
        while True:
            free_mem = pycom.get_free_heap()
            pkt = "Hello, from " + str(pymesh.__pymesh.mac()) + ", time " + str(time.time()) + ", mem " + str(free_mem)
            pybytes.send_signal(1, pkt)
            time.sleep(20)
```

Every time a data is sent trough Pymesh, the node's monitoring data is also sent. This monitoring data contains information as the number of neighbors, loRa mac,  IP, role, age, and location.

Some of this information can be seen in the Pymesh Monitoring view or in section Signal, in the device interface in Pybytes.