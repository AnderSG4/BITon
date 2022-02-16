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
![Egurra](https://user-images.githubusercontent.com/99752283/154231856-d20924fc-0dbc-4cbd-bd84-b1373454c1da.PNG)
![Gurpila hoverboard](https://user-images.githubusercontent.com/99752283/154232087-930d0fa6-29e8-4687-a9d9-4166f45c7fa1.PNG)
![Gurpila](https://user-images.githubusercontent.com/99752283/154233297-fea1f2e9-36af-4aef-a730-67772f04ec86.PNG)
![Metakrilatoa](https://user-images.githubusercontent.com/99752283/154233471-c455281d-9527-4545-adc9-ffb7c597948a.PNG)
![Robota](https://user-images.githubusercontent.com/99752283/154233670-c475ba0d-8dbd-4090-8e8c-694bd47d8e22.PNG)



### Hemen kableak ordenatzeko erabili degun PCB-aren eskema eta nola funtzionatzen duen.

PCBeskema:

![PCB eskema](https://user-images.githubusercontent.com/99752283/154231522-c72b7809-1786-4c93-98e6-59d52ae0ebb5.PNG)

PCB:

![PCB](https://user-images.githubusercontent.com/99752283/154231534-cd52d2bc-cb8e-41f7-94ff-5f9593933d78.PNG)


PCB-a bi ataletan banatzen da; goiko atalean motorretik datozen kableak zuzenean hoverboard plakara konektatu beharrean PCB-ko konektoretara koenktatzen fitugu, hauek ordenatzeko eta batzuetan kablea motzegia delako eta ez da konekxioa egiteko iristen. Beheko atalean alimentazioaz arduratzen da. Bateria kargatzeko eta plaka alimentatzeko zirkuitu sinple bat sortu dugu, diodo bat jarriz ekiditen degu korronteak zirkuituan bueltatzeak.

Jetson nanoa 5V-rekin alimenta daiteke eta horretarako pull-down bat erabili genuen bateriako 36V-ak erabiliz. Azkenean atal honek wz digu ezertarako balio, bai 5V-ak lortu ditugu baina Jetson nanoak 4A eskatzen ditu ere eta bateriak emandako korrontea ez da nahikoa hoverboard plaka eta Jetson-a abiarazteko. Azkenean beste bateria batekin alimentatu behar izan dugu.

![Pull-down](https://user-images.githubusercontent.com/99752283/154233854-fd1c8600-6893-400b-955d-3c0f662eff25.PNG)


## Hardware
### STM32 plaka:

![pinout](https://user-images.githubusercontent.com/99752283/154119568-9e194712-77e9-4167-9c38-01d6bcdfd503.png)



Hardware originalak lau orratzeko bi kable eusten ditu, hasieran bi alboetan lotuak. Hoverboard pinak: GND, 12/15V eta USART2 & 3. USART2 & 3, UART, PWM, PPM eta IBUS input. Gainera, USART2 12bit ADC bezala erabil daiteke, eta USART3, berriz, I2C bezala. Kontuan izan USART3 (eskuineko alboko kablea) 5V tolantea den bitartean, USART2 ez dela 5V tolantea.

## JetsonStart
Jetson nanoarekin hasteko SD txartel batean irudia sartu behar zaio.
https://developer.nvidia.com/embedded/downloads

Balena erabili degu.
https://www.balena.io/etcher/



## Flashing
Finwarea biltzeko https://platformio.org/ erabili degu.
gcc-arm-none-eabi deskargutu behar da, 7. bertsioa funtzioakionatzen du https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads
Gcc-ren kokapena Makefilean ezarri beharko da, "PREFIX =..." lekuan.
STM32 zuzenean, GND, 3V3, SWDIO eta SWCLK dituen arazketa-goiburu bat dago. Konektatu GND, SWDIO eta SWCLK zure SWD programatzailera. ST link batekin lan egin dugu. Platformio-k ez badu ST link-a detektatzen konfigurazioetara jun, J link erakurtzen egon daiteke.

Ez elikatu plaka nagusia zure programatzailearen 3,3 V-ekin! Honek oinarrizko plaka batzuk erre ditu!
STM32 plaka alimentatua eta piztuta egoon behar da prozesuan.

## IP

![Pioled](https://user-images.githubusercontent.com/99752283/154234191-716f829e-ae03-4d2b-a46c-8b8991947d56.PNG)


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


