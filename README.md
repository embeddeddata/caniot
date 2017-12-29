# caniot
CAN IoT Example Project

# Preliminary Outline
* Using a CAN Based Sensor like: https://www.stw-technic.com/wp-content/uploads/2015/08/T01_ManualJ1939.pdf
* Hardware setup with Raspberry Pi and CAN board: http://skpang.co.uk/catalog/pican2-canbus-board-for-raspberry-pi-2-p-1475.html
* Testing CAN interface via can-utils: https://github.com/linux-can/can-utils
* Python interaction on CAN Bus with python-can: https://github.com/hardbyte/python-can
* SocketCANd setup/compilation and configuration for Raspberry Pi: https://github.com/dschanoeh/socketcand
* Automating Raspberry Pi as SocketCAN gateway on boot
* Setting up a simple socket interface with Python from another device/computer
* Scaling raw CAN data to J1939 or other format
* Sending scaled data to a TSDB and basic queries: https://www.influxdata.com/
* Setting up Grafana and linking to InfluxDB: https://grafana.com/
