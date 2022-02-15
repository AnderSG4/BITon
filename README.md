# BITon
Ros hardware kontrolatzailea hoverboard plakarentzako UART bidez. 
Robotak kamara bat eta lidar bat izango du ingurunea ezagutzeko eta kanpoko ordenagailu bat erabiliz komandoak emango zaizkio mugitzeko.
## Atalak

* [Txasis](#txasis)
* [Hardware](#hardware)
* [JetsonStart](#jetsonstart)
* [Flashing](#flashing)
* [IP](#ip)
* [Lidar](#lidar)
* [Kamara](#kamara)
* [Problema soluzioak](#problema soluzioak)

## Txasis
![IMG_20211217_134908](https://user-images.githubusercontent.com/99752283/154117982-706ab00e-2f8e-4979-b3e6-c8924d6b362e.jpg)
![IMG_20211217_134835](https://user-images.githubusercontent.com/99752283/154118868-161837d9-8745-4cf7-b09d-423688301a34.jpg)


## Hardware
![pinout](https://user-images.githubusercontent.com/99752283/154119568-9e194712-77e9-4167-9c38-01d6bcdfd503.png)
STM32 plaka.

## JetsonStart
Jetson nanoarekin hasteko SD txartel batean irudia sartu behar zaio.
https://developer.nvidia.com/embedded/downloads

Balena erabili degu.
https://www.balena.io/etcher/



## Flashing
Finwarea biltzeko https://platformio.org/ erabili degu.
gcc-arm-none-eabi deskargutu behar da, 7. berts funtzioakionatzen du https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads
Gcc-ren kokapena Makefilean ezarri beharko da, "PREFIX =..." lekuan.
STM32 zuzenean, GND, 3V3, SWDIO eta SWCLK dituen arazketa-goiburu bat dago. Konektatu GND, SWDIO eta SWCLK zure SWD programatzailera. ST link batekin lan egin dugu. Platformio-k ez badu ST link-a detektatzen konfigurazioetara jun, J link erakurtzen egon daiteke.

Ez elikatu plaka nagusia zure programatzailearen 3,3 V-ekin! Honek oinarrizko plaka batzuk erre ditu!
STM32 plaka alimentatua eta piztuta egoon behar da prozesuan.

## IP
Jetson nano-a WIFI-ra konektatzerakoan bere IP-a jakiteko pioled display bat erabili dugu. https://github.com/JetsonHacksNano/installPiOLED , hemen dago erabilitako kodea.
```
git clone https://github.com/JetsonHacksNano/installPiOLED
```
-Karpeta honetan sartu.
```
cd installPiOLED/
```
-Pioled erabiltzeko hau deskargatu.
```
./installPiOLED.sh
```
-Karpeta honetan sartu.
```
cd pioled
```
-Pioled-a abiatu.
```
sudo python3 stats.py
```
-Modifikatu etherneta ez ikusteko eta wlan agertzeko. Ethernet textu bezala jarri eta wlan alderantziz.
```
gedit stats.py
```
-Berriro abiatu.
```
sudo python3 stats.py
```
-Serbizio bat sortu, jetson nano pizterakoan Pioled batera pizteko.
```
./createService.sh
```
-Reboot bat egin.


## Lidar
Lehenengo gauza ROS instalatzea da.
```
git clone https://github.com/JetsonHacksNano/installROS.git
```
-Karpeta honetan sartu
```
cd installROS/
```
-ROS instalatu
```
./installROS.sh
```
-Nahi den ROS paketea instalatu
```
./installROS.sh -p ros-melodic-desktop
```
-Skript bat sortu ruta espezifiko batekin.
```
./setupCatkinWorkspace.sh [optionalWorkspaceName]
```
-Hemen ikusi dezakegu zer konexio behar den.
```
gedit ~/.bashrc
```
-Exekutatzeko
```
source ~/.bashrc
```
```
roscore
```

## Kamara
Jetson nanoa eta kamararen arteko komunikazioa egiteko https://www.stereolabs.com/developers/release/ instalatu behar da. Gero komando hauek sartu terminaletik.
-Zoaz instalatzailea deskargatu duten karpara.
```
cd path/to/download/folder
```
-Instalatzaileari exekuzio baimena gehitu, chmod +x agindua erabiliz. Instalatzailearen izena eta deskargatutako bertsioa aldatu.
```
chmod +x ZED_SDK_JP4.3_v3.0.run
```
-ZED SDK instalatzailea abiatu.
```
./ZED_SDK_JP4.3_v3.0.run
```
Instalazioaren hasieran, Software License erakutsiko dute. Q heman dute irakurri ondoren.
Instalazioan, menpekotasunei, erremintei eta laginen instalazioari buruzko galderak erantzun beharko dituzu. Bai eta ez, eta heman Enter. Jo Enter akatsen aukera aukeratzeko.
Jetsonen tauletan, CUDA automatikoki instalatzen da JetPackekin. Beraz, orain prest zaude ZED SDK erabiltzeko.



## 


