# H.A.L.T
## Holistic Traffic Management System

The project had multiple (Proximity sensors embedded on the road to determine the density of traffic waiting on a particular signal and accordingly control the traffic lights. Apart from this the red-light sensors also had a RFID receiver which detected a passing emergency vehicle such as an ambulance and appropriately changed the traffic lights.In addition to this grafana was used to represent data in a dynamic way.

![Holistic traffic management system](https://user-images.githubusercontent.com/84952780/173198548-de8e2d2e-53cd-4c11-8c58-bd520c489844.png)

### Problem
***

- Need for an effective way to manage traffic efficiently.
- To help increase the efficiency of Emergency Services.

### Proposed Solution
***

- IoT based Traffic Management System
- We have aimed at adding multiple sensors embedded on the road to determine the
  density of traffic waiting on a particular signal and accordingly control the traffic lights.
- Apart from this the red-light sensors also
has a receiver which detects a passing
emergency vehicle such as an ambulance
and get changed appropriately.

![halt1.jpg](https://i.postimg.cc/NMTZFLPq/halt1.jpg)

### Tech Stacks used
***

This is an IoT enabled Traffic Management System. In this project we have used multiple Tech. stacks like:

- Django
- Grafana
- Fusion 360
- Prometheus
- C++
- Python
- Arduino IDE
- 
Furthermore we have used:

- RFID Sensor
- Servo
- IR sensor
- LCD

We even used Open cv for better detection of traffic.

### Data Visualisation using GRAFANA
***

Grafana is a multi-platform open source analytics and interactive visualization web application. It provides charts, graphs, and alerts for the web when connected to supported data sources.

We are using it to show the data coming in from the sensors to monitor the flow of traffic in a better and visually appealing way.

![image](https://user-images.githubusercontent.com/84952780/174129103-674f4962-555e-42ad-991f-1a19d06c43e4.png)

![image](https://user-images.githubusercontent.com/84952780/174129139-455fc0a9-feb7-487e-b070-9e7009cf126d.png)

### Future Aspects 
***

- Implementation of OpenCV.
- The solution is proposed to reduce the initial
implementation and setup cost of the hardware
replacing costly sensors with the pre-installed cctv
cameras at the junction.

![OpenCV](https://user-images.githubusercontent.com/84952780/170505446-30d74291-6b9c-4d08-a657-64af858bcfe1.png)

Installation
Installing python dependencies
pip3 install -t requirements.txt
Installing redis-server
sudo apt install redis-server
Installing Grafana
sudo apt-get install -y apt-transport-https
sudo apt-get install -y software-properties-common wget
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt-get update
sudo apt-get install grafana
Starting the Grafana-server

sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl status grafana-server
sudo systemctl enable grafana-server.service
(last line configures it to run at boot)

Now you can head over to http://localhost:3000/login (the default username and password is 'admin')

Installing Prometheus
sudo apt update && sudo apt install wget -y
sudo useradd --system --no-create-home --shell /usr/sbin/nologin prometheus
cd ~/downloads
wget https://github.com/prometheus/prometheus/releases/download/v2.28.0/prometheus-2.28.0.linux-amd64.tar.gz
tar xvzf prometheus-2.28.0.linux-amd64.tar.gz
sudo mv -v prometheus-2.28.0.linux-amd64 /opt/prometheus
sudo chown -Rfv root:root /opt/prometheus
sudo chmod -Rfv 0755 /opt/prometheus
sudo mkdir -v /opt/prometheus/data
sudo chown -Rfv prometheus:prometheus /opt/prometheus/data
sudo nano /etc/systemd/system/prometheus.service
Now paste the code given below.

[Unit]
Description=Monitoring system and time series database

[Service]
Restart=always
User=prometheus
ExecStart=/opt/prometheus/prometheus --config.file=/opt/prometheus/prometheus.yml --storage.tsdb.path=/opt/prometheus/data
ExecReload=/bin/kill -HUP $MAINPID
TimeoutStopSec=20s
SendSIGKILL=no
LimitNOFILE=8192

[Install]
WantedBy=multi-user.target
Press ctrl+o and then press the Enter key, followed by ctrl+x.

Starting the Prometheus-server

sudo systemctl daemon-reload
sudo systemctl start prometheus.service
sudo systemctl enable prometheus.service
(last line configures it to run at boot).

Prometheus is now Installed, navigate to the URL http://localhost:9090/targets from your favorite web browser and all the targets that you’ve configured should be displayed. You should be able to see that the prometheus target is in the UP state.

Adding custom-exporter port to prometheus
sudo /opt/prometheus/prometheus.yml
Create a new target by pasting this under 'static_configs:'

targets: [localhost:8085]
note(restart the prometheus service after adding the above lines to the yml file)

Installing Arduino IDE
You can easily install Arduio IDE through the Ubuntu software store or download the tar file through https://www.arduino.cc/en/software.

tar -xvf <package_name> #i.e arduino-1.6.10-linux64.tar.xz>
cd <directory_name> #i.e arduino-1.6.10>
./install.sh
ls -l /dev/ttyACM*
sudo usermod -a -G dialout <username>
5

Usage
Check status of redis-server, prometheus and grafana by typing

sudo systemctl status <service_name>
At first you are required to manually set values in redis with the help of redis-cli, you can do that by

redis-cli
set <variable_name> <value>
If any service is not active you can start it by typing

sudo systemctl start <service_name>
(you can exit the code by pressing q) (similarly start can be replaced by stop to disable the active service)

Using esp32 with Arduino IDE
Install arduino ide and add esp32 to the ide by adding the given url to File > Preferences > setings > Additional Boards Manager Urls
https://dl.espressif.com/dl/package_esp32_index.json, http://arduino.esp8266.com/stable/package_esp8266com_index.json
Open the Boards Manager. Go to Tools > Board > Boards Manager...

Search for esp32 and press install

Now choose esp32 dev module in Tools > Board > esp32 Arduino

Search and install Adafruit MFRC630 RFID library and Wifi library from Tools > Manage Libraries

Open the esp32 file from your directory (or ctrl+O)

Change ssid and password as per your Wifi.

Changing the server name in the esp32.ino file:

if on the same network:
ifconfig
type in https then inet ip followed by ':8000', example: "https:/192.168.0.1:8000"

if different network you can do port forwarding with ngrok. type ngrok http -region in 8000 type in http forwarding link, example: "tcp://0.tcp.in.ngrok.io:18832"

(note this ip will change everytime you run ngrok command so youll have to change the ino code.)

Compile and upload the file on the esp32 module

Staring fast-api
Open your terminal and go to the cloned directory, inside the grafana_site directory and run
uvicorn main:app --host 0.0.0.0 --reload
this will start the rest api server

Starting the prometheus node-exporter
python3 prom-exporter.py
Open your web browser and head over to http://localhost:8085. You should be able to see some metrics being published over there by your prometheus exporter.
when you run the files at the first time you need to manually set value in redis, you can do that by

redis-cli
SET <your_metric_name> <value>
The metrics that you need to set here are: tlimit(for Time Limit), esevices(for Emergency Services, vden(for Vehicular Density) (you can find these variables being defined in main.py and used in prom-exporter.py)

You can also check the whether the values are being published or not by

redis-cli
GET <your_metric_name>
(you can exit it by pressing using exit command)

Using Grafana
Now you can log on to your grafana server, type in http://localhost:3000 in your browser. Both username and password would be 'admin'.

Go to the explore panel (Compas icon on the left)

Now select Metric Browser>Halt_total then select your metric/job. file:///home/divi/Pictures/grafana.pngimage file:///home/divi/Pictures/grafana1.pngimage

FINAL PROJECT
image

OPEN CV
With the boom of artificial intelligence and machine learning, this project can be upgraded to the next level with the integration of computer vision (OpenCV). OpenCV works on image capturing technology and calculating the contour area for every frame and passing it on further. Based on the area of contours we can further manipulate the duration of green signal for subsequent cycles 6

STEPS
  pip install opencv
  cv2.imread()
  cv2.imshow() 
