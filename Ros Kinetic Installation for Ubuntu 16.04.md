# **Ros Kinetic Ubuntu 16.04'ya kurulumu**

>### Not: Ros kinetic sadece Ubuntu 16.04'de kurulabilir.

### [Türkçe Ros Jade Kurulum Sayfası](http://wiki.ros.org/tr/jade/Kurulum/Ubuntu)

Kurulum
İlk önce ubuntudaki repo konfigürasyonlarının yapılması gerek
Sources.list indirilmesi
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

Key kurulumu
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116

### Kurulum

    sudo apt-get update

    sudo apt-get install ros-kinetic-desktop-full

    apt-cache search ros-kinetic

### Rosdep İlk Kurulumu
ROS'u kullanmadan önce rosdep'i başlatmanız gerekir. rosdep, derlemek istediğiniz kaynak için sistem bağımlılıklarını kolayca kurmanızı sağlar ve bazı temel bileşenleri ROS'ta çalıştırmak için gereklidir.

    sudo rosdep init

    rosdep update

    Environment Kurulumu
    echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc

    source ~/.bashrc

    source /opt/ros/kinetic/setup.bash

### Paketleri build etmek için Bağımlılıklar
    
    sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential

## Ros İle Lidar Kurulumu

1. Bilgisayarınızın Velodyne sensore ile iletişim kurmak için ayarlanması
Verilen adaptör aracılığıyla LIDAR'a güç verin
LIDAR'ı bilgisayarınızdaki bir Ethernet bağlantı noktasına bağlayın.
Şimdilik, bilgisayarınızdaki WiFi bağlantısını devre dışı bırakın.

    1.1 Gnome arabirimiyle bilgisayarınızın IP adresini yapılandırın
    Gnome Menüsüne (Süper key) erişin, "Networks Connections" yazıp çalıştırın. 
    Bağlantı adını seçin ve "edit" e tıklayın. IPV4 Ayarları sekmesini seçin ve “Method” alanını “Manual” olarak değiştirin.
    "add" düğmesine tıklayın ve IP adresi alanını 192.168.1.100 olarak ayarlayın (“100”, 201 hariç 1 ile 254 arasında bir sayı dışında herhangi bir sayı olabilir).
    “Netmask” ı 255.255.255.0 ve “Gateway” i 0.0.0.0 olarak ayarlayın.
    Bitirmek için "kaydet" üzerine tıklayın.

    1.2 Bilgisayarınızı terminal aracılığıyla LIDAR'a bağlama
     
     Verilen adaptör aracılığıyla LIDAR'a güç verin LIDAR'ı bilgisayarınızdaki bir Ethernet portuna bağlayın. Statik olarak 192.168.3.x aralığında bu bağlantı noktasına bir IP atayın.

        sudo ifconfig eth0 192.168.3.100

    Not: "eth0" sizde farklı olabilir. "ifconfig" yazıp ethernet ismimizi öğrenmemiz gerek. Mesela bende "enp3s0" dir.

    LIDAR'ın IP adresine statik bir yol ekleyin. IP adresi LIDAR ile birlikte gelen CD kutusunda bulunabilir.

        sudo route add 192.168.XX.YY eth0

    Not: Ben genel kullanım olarak "192.168.1.201" yaptım. sudo nmap -sS 192.168.1.* yazıp bu ip aralıktaki ip lere ping atıp ayakta olan bağlantıları kontrol edebiliriz.

    1.3 Yapılandırmaları kontrol etme

    Bağlantıyı kontrol etmek için web tarayıcınızı açın ve aşağıdaki sensörün ağ adresine erişin: 192.168.XX.YY mesela 192.168.1.201.


2. ROS bağımlılıklarını yükleme

        sudo apt-get install ros-kinetic-velodyne

3. VLP16 sürücüsünü yükleme

Terminalde, ROS çalışma alanınızın içinde “src” klasörünü bulun ve aşağıdaki komutu çalıştırın:
        
    cd ~/catkin_ws/src/ && git clone https://github.com/ros-drivers/velodyne.git

Bundan sonra, terminalde, çalışma alanınızın içinde tüm bağımlılıkları güncelleyin:
rosdep install --from-paths src --ignore-src --rosdistro YOURDISTRO -y

Not: "YOURDISTRO" kısmı sizin yükleme yapacağınız path oluyor. Ben o an "catkin_ws/src" path'indeydim. Bir üst path e çıktım "cd ..". sonra şu komutu çalıştırdım. rosdep install --from-paths src --ignore-src --rosdistro . -y "YOURDISTRO" yerine "." yazdım.

Sonra çalışma alanınızı oluşturun:

    cd ~/catkin_ws/ && catkin_make yada direk catkin_make i çalıştırabilirsiniz.

Şimdi, Velodyne paketiniz çalışmaya hazır.

4. Verileri Görüntüleme

Terminalde aşağıdaki komutu çalıştırın:
        
    roslaunch velodyne_pointcloud VLP16_points.launch

Şimdi gerekli düğümler çalışıyor. Bunu aşağıdaki komutla kontrol edebilirsiniz.

    rosnode list

Yayınlanan ve aşağıdaki konulara abone olan mesajları görebileceksiniz:

    rostopic echo /velodyne_points

Bundan sonra başka bir terminal açıp, sabit bir çerçeve olarak "velodyne" ile rviz'i başlatın:

    rosrun rviz rviz -f velodyne

"displays" panelinde, "Add" ye tıklayın, sonra "Point Cloud2" yi seçin, ardından "OK" a basın.
Yeni "Point Cloud2" sekmesinin "Topic" alanında "/ velodyne_points" ifadesini girin.
Tebrikler. Şimdi, Velodyne'niz sisteminizde "real" dünyayı inşa etmeye hazır. Tadını çıkar.

### ROS Nedir ?

    Robot Operating System(ROS), bilgisayar üzerinden robotlarımızı ve robot bileşenlerimizi kontrol etmemizi sağlayan BDS lisanslı, açık kaynak kodlu bir yazılım sistemidir. Dil bağımsız bir mimariye sahiptir.

### ROS Ne Değildir?

    Gerçek bir işletim sistemi değildir.

    Bir programlama dili değildir.

    Geliştirme ortamı/IDE değildir.

### ROS Ne İşe Yarar?

    Robotunuzdaki bilgisayar ve ana bilgisayarınız arasında köprü kurmanızı sağlar.

    ROS sayesinde başka bir robot için yazılmış standart bir algoritmayı yükleyerek direk olarak kullanabilirsiniz.

    Başlıklar ve mesajlar yardımı ile sensör verilerinizi ve işlenmiş verilerinizi rahatlıkla gereken yere ulaştırabilirsiniz.

    Gazebo ve Rviz ortamlarıyla tam uyumlu çalışması sayesinde kodlarınızı simülasyon ortamında test edebilirsiniz, böylece hızlı bir geliştirme sağlayabilirsiniz.

### Neden ROS Kullanmalıyım?

    Açık kaynak kodlu olması.

    Kod reusablity olması

    Python, C++, java, lisp, lua gibi bir çok popüler dille kodlama yapılabilmesi.

    Modüler yapısı sayesinde bir bileşen çökmesinde tüm sisteminiz çökmez.

    Gazebo ve Rviz ile beraber kullanabilirsiniz, böylece görselleştirme ve simülasyon sistemlerinizi kolayca kurabilirsiniz.

    Bir çok kurum, kuruluş, üniversite tarafından kullanıldığı için ve yarışmaların bir çoğu ROS alt yapısını kullandığı için robot dünyasında standart konumunu almaktadır.

    Her geçen gün daha fazla robotu destekler hale gelmektedir.

    İki işletim sistemi arasında çalışabilir. Örneğin; robot üzerinde oluşan bir veriyi rosmaster aracılığıyla Windows kurulu bir bilgisayardaki MATLAB’a gönderebilir, orada verileri işleyip robota geri yollayabilirsiniz. Başka olarak Unity3d'de de bazı simulasyonlar yapılabilir. Bu yapısı sayesinde sizi bir işletim sistemine bağımlı kalmaktan kurtarır.

### Alternatif Robot Yazılımları

ROS’a alternatif olabilecek başka robot yazılımları da mevcuttur. Bunlar Player, YARP, Orocos, Carmen, Orca, MOOS, Microsoft Robotic Studio.

## Kaynakça

[Ros Nedir? - 1](http://dusunenrobot.com/ros-robot-operating-system-nedir/)

[Ros Nedir? - 2](https://ozguradem.net/robotik/2015/03/10/ros-robot-operation-system-nedir/)

[Ros Nedir? - 3](https://www.bilimcag.com/nedir/ros-robot-operating-system-nedir/)

[Ros & Lidar](http://wiki.ros.org/velodyne/Tutorials/Getting%20Started%20with%20the%20Velodyne%20VLP16)