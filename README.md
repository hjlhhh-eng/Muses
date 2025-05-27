# Fine-grained-classification-test-dataset-of-automatic-driving-system

This paper presents simulation-based benchmark Muses, a comprehensive and diverse dataset designed to test self-driving cars through a combination of factors such as weather, roadside elements, and road structure to identify potential abnormal driving behaviour of self-driving cars, and to monitor the driving behaviour of self-driving cars during data collection through common problems that tend to occur in real car driving, in order to provide future safe driving to provide a solid foundation.

Muses, based on ten simulation maps (Town01-Town12,except Town08,Town09) from Carla simulator and Apollo simulation platform collects data by segmenting driving scenarios at a fine-grained level, enabling researchers to efficiently select relevant scenarios. Our benchmark can categorise road features such as ‘Multi-directional’, ‘Uni-directional’, ‘Ramps’, ‘Tunnels’, ‘circle’, “bridge”, “auxiliary”, “under corridor”, ’ under buildings,’ “hills,” and “pavements between fences,” and combines them with challenging weather and roadside elements, including ’fog-filled bridges ’, ‘puddles’, etc., as well as the presence of roadside buildings, vegetation, fences, etc. This approach supports detailed testing of Apollo in different integrated driving scenarios including road features, roadside elements, weather conditions and driving behaviour. In addition, we provide a large amount of data during Apollo operation to visualise the state of the system under various challenges.

Muses have the characteristics of:

Driving scenario refinement: We redefine driving scenarios by categorising them according to road structure/features, roadside elements and weather. In contrast to the traditional approach of broadly categorising scenarios into urban or rural environments, we decompose these scenarios into more detailed categories such as BFPRS(MD, BCFLMPRS(AAMRBMD, FLMPRS(AAMRMD, BCLPRS(MDUTC), etc. , so that a more comprehensive presentation of driving scenarios can be made, and thus a more refined test of ADS can be conducted for verifying the safety and reliability of ADS. Among them, the string before the parentheses is the abbreviation of the roadside element word.  
The complete abbreviation list of the roadside elements is:  
![Roadedge Element](examples/roadside.jpg)

The abbreviations in parentheses are road structure words.  
The complete list of road structure word abbreviations is:  
![Road Structure](examples/road.jpg)

The final classification result is:  
![Category Result](examples/categories.jpg)

Fine-grained multi-scenario benchmark: We constructed a fine-grained benchmark, Muses, specifically for Apollo. The benchmark is organised by road structure, roadside elements and map categories (representing weather), and each piece of data consists of a video file, a playback log file, a car driving information file, an actor information file, and a folder documenting whether there is a problem with Apollo in eight ways. 

Diverse driving behavioral anomalies and Visualization: Our benchmark includes a visualisation feature where each data contains a subfolder recording the occurrence of the ‘eight major faults/abnormal driving behaviours’: collision, driving in reverse,  speeding, Red light running, turning too sharp/fast or oversteering/Rapid cornering, crossing lane lines, no response, rapid/sudden acceleration, rapid/sudden deceleration, and so on. These unexpected behaviours are recorded frame by frame to ensure comprehensive fault tracking.
## Table of Contents

1.[Introduction](#introduction)  
2.[Data Download](#data-download)  
3.[Data Use](#data-use)  
4.[Citation](#citation)  

## Introduction

The dataset consists of: video files (played using kazam software), car driving data over time, actor data (including sensor data), various problem exposure records, and log files that can be used for playback. All data in the driving scenarios are saved in a timestamped folder. Driving scenarios are categorised according to road structure. The map will store all types of road structures. In addition, a csv file records the start and end point of each map and can record all driving scenarios on the map.

### Driving scene display
Some example videos are shown below:

### BFLPRS(MD)
![](./examples/straight.gif)  

### BFMPRS(MD)
![](./examples/common_road.gif)  

### BFPRS(MDRT)
![](./examples/ramp.gif)  

### BFPRS(MDT)
![](./examples/Tunnel.gif)  

### BFPRS(MDT)
![](./examples/circle.gif)
## Data download

### Installation of Bridge
It was tested with Carla 0.9.14 and the Apollo v8.0.0 (v8.0.0),the operating system uses Ubuntu 20.04.6 LTS.
Refer to these link：  
https://blog.csdn.net/weixin_46336532/article/details/135017708,  
https://github.com/guardstrikelab/carla_apollo_bridge/blob/master/docs/GettingStarted.md

#### Prerequisites

* docker
```
sudo apt-get install docker.io
```
* NVIDIA Container Toolkit
```
curl https://get.docker.com | sh \
&& sudo systemctl --now enable docker
```
```
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
    && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
    && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
          sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
          sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```
```
sudo apt-get update
```
```
sudo apt-get install -y nvidia-docker2
```
```
sudo systemctl restart docker
```
 * docker-compose
```
 sudo curl -L "https://github.com/docker/compose/releases/download/v2.0.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
* Change File Permission
```
sudo chmod +x /usr/local/bin/docker-compose
```
#### Build And Run Apollo  
* Clone apollo v8.0.0
```
# Using SSH
git clone -b v8.0.0 git@github.com:ApolloAuto/apollo.git

#Using HTTPS
git clone -b v8.0.0 https://github.com/ApolloAuto/apollo.git
```
* Build Apollo
```
cd apollo
echo "export APOLLO_ROOT_DIR=$(pwd)" >> ~/.bashrc  && source ~/.bashrc
```
run:
```
sudo rm -rf /apollo/.cache
bash docker/scripts/dev_start.sh
```
Upon successful execution, you will see the following message
```
[ OK ] Congratulations! You have successfully finished setting up Apollo Dev Environment.
[ OK ] To login into the newly created apollo_dev_lei container, please run the following command:
[ OK ] bash docker/scripts/dev_into.sh
[ OK ] Enjoy!
```
Above If you occured error as "ERROR: Config value 'cuda' is not defined in any .rc file",you can try
```
./apollo.sh config -n
```
Run this command to enter the container
```
bash docker/scripts/dev_into.sh
```
Make the GPU version:
```
./apollo.sh build_gpu
```
After successful compilation, the following will be printed:
```
==============================================
[ OK ] Done building apollo. Enjoy!
==============================================
```
Run this command in the container to start Dreamview
```
./scripts/bootstrap.sh
```
Finally, open the link in your browser
```
http://localhost:8888/
```
#### Run Carla
* Clone the carla_apollo_bridge project outside Apollo container
```
# Using SSH
git clone git@github.com:guardstrikelab/carla_apollo_bridge.git

#Using HTTPS
git clone https://github.com/guardstrikelab/carla_apollo_bridge.git
```
Pull carla image and run
```
cd carla_apollo_bridge
./scripts/docker_run_carla.sh
```
#### Run carla_apollo_bridge
* Copy the src folder into Apollo container
```
docker cp carla_bridge <apollo_container_name>:/apollo/modules/carla_bridge
```
* Install carla_bridge  
Enter the Apollo container and run:
```
cd /apollo/modules/carla_bridge
chmod +x install.sh
./install.sh
source ~/.bashrc
```
* Start the bridge
```
python main.py
```

### Data Use
#### start a co-simulation
* Choose a path to save the downloaded dataset
* Open DOS in the selected path and use the command:
```
git clone https://github.com/hjlhhh-eng/Fine-grained-classification-test-dataset-of-automatic-driving-system.git
```
* According to the Baidu Netdisk link provided in /dataset/map.txt, obtain the map.zip (the map format applicable to Apollo, including all maps of Carla simulator Town01-Town12 (except for Town08 and Town09)), and copy the source files of the Carla maps you want to test to /apollo/modules/ map/data for a testable driving scenario.
* Set the map you want to test in /apollo/modules/carla_bridge/config.
* Open the command prompt window:

1.Open the Apollo container and run it:
```
docker start <apollo_container_name>  
docker exec -it <apollo_container_name> /bin/bash
./scripts/bootstrap.sh
```
2.Enter it in the website area of the browser to open the Dreamview website.

3.Open apollo client: http://localhost:8888.

4.Select the map you want to test in Dreamview.

* If dreamview doesn't display the map, switch to carla_town04 and then switch back.
  
5.(Optional) Select "Task" in the sidebar and turn on "Camera Sensor" in "Others".

6.(Optional) Select "Layer Menu" in the sidebar and turn on "Point Cloud" in "Perception".

7.Select "Module Controller" in the sidebar and turn on "Routing", "Planning", "Control", "Prediction" module.(The purpose of opening the module is to adjust and test the module function based on the running trajectory of the historical dataset and the current situation. If there is no need to test Apollo, but only to view the historical data trajectory, the module button does not need to be opened.)

If you want to close Dreamview, run:
```
./scripts/bootstrap.sh stop
```
8.Open a new command prompt window:

9.Open the Carla container,run:
```
docker start <carla_container_name>
```
10.Open a new command prompt window, run:
```
docker exec -it <apollo_container_name> /bin/bash
```
11.Run the bridge:
```
cd /apollo/modules/carla_bridge  
python main.py
```
If an error occurs, you can try using the statement: 
```
source /apollo/cyber/setup.bash
```
#### Experiment
Next, experiments can be conducted.
* Based on /dataset/dataset.txt, go to Baidu Netdisk and paste the address and enter the extraction link to obtain the complete dataset, and select the timestamp folder you want to use to test the auto-drive-system according to the classified dataset.
* Place the timestamp file in the path:/apollo/modules/carla_bridge, copy the log file of the timestamp folder corresponding to the dataset you want to use, such as 20240810145622_recording.log, to a certain location in the Carla container， We need to first open the Carla container,and then
```
docker cp /apollo/modules/carla_bridge/20240810145622_recording.log <carla_container_name>:/home/carla/.config/Epic/CarlaUE4/Saved/
```
* Copy the code folder to the path:
```
/apollo/modules/carla_bridge
```
* Open a new command prompt,enter the Apollo container,
* Open the replay.py file in the 'code' folder and replace the numbers in the 'replay_file=20240917133158_recording.log' sentence with the timestamp numbers you want,and then:
```
cd /apollo/modules/carla_bridge/code
```
and run
```
python replay.py
```
Timestamps folder also record the problems that occurred with Apollo, the information data of the cars controlled by Apollo, and the actor data in this timestamp folder.

Due to actor ID matching issue,after using a data, it is necessary to Use 'Ctrl+C' to disconnect the bridge, exit the Apollo container where the bridge connection is located (step 10), and stop carla:
```
docker stop <carla_container_name>
```
repeat steps 9-11.
And select the desired data to experiment again.

## Citation
```
@inproceedings{dosovitskiy2017carla,
  title={CARLA: An open urban driving simulator},
  author={Dosovitskiy, Alexey and Ros, German and Codevilla, Felipe and Lopez, Antonio and Koltun, Vladlen},
  booktitle={Conference on robot learning},
  pages={1--16},
  year={2017},
  organization={PMLR}
}
```
```
@inproceedings{huang2018apolloscape,
  title={The apolloscape dataset for autonomous driving},
  author={Huang, Xinyu and Cheng, Xinjing and Geng, Qichuan and Cao, Binbin and Zhou, Dingfu and Wang, Peng and Lin, Yuanqing and Yang, Ruigang},
  booktitle={Proceedings of the IEEE conference on computer vision and pattern recognition workshops},
  pages={954--960},
  year={2018}
}
```
```
https://developer.apollo.auto/Apollo-Homepage-Document/Apollo_Doc_CN_6_0/%E4%B8%8A%E6%9C%BA%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/%E4%B8%8A%E6%9C%BA%E5%AE%9E%E8%B7%B5Apollo%E8%A7%84%E5%88%92%E8%83%BD%E5%8A%9B/apollo%E8%A7%84%E5%88%92%E6%A8%A1%E5%9D%97%E5%AE%9E%E8%B7%B5
```
```
https://blog.csdn.net/weixin_46336532/article/details/135017708
```
```
https://github.com/guardstrikelab/carla_apollo_bridge/blob/master/docs/GettingStarted.md  
```
```
https://blog.csdn.net/condom10010/article/details/130386323
```
```
https://zhuanlan.zhihu.com/p/369780423
```
```
https://blog.csdn.net/weixin_44177594/article/details/126890190?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522BBE5EE0F-D9FF-40C4-8659-EAD3F37C8A28%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=BBE5EE0F-D9FF-40C4-8659-EAD3F37C8A28&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-10-126890190-null-null.142^v100^pc_search_result_base9&utm_term=carla%E5%9C%B0%E5%9B%BE%E8%BD%ACapollo&spm=1018.2226.3001.4187
```
