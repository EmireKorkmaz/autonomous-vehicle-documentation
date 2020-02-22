# **Google Colaboratory ile Çalışmaya Giriş*

>Bu yazıda Google Colab ile nasıl çalışıldığı anlatıldı.

1. Öncelikle Google Drive'ı açmalı, +yeni > Diğer > Colaboratory seçeneğini seçmelisiniz.

>Eğer daha önce Colaboratory bağlantısı yapmamışsanız Diğer > +Daha fazla uygulama bağla seçeneğinden Colaboratory'i arayarak bulabilirsiniz.

2. Ardından .ipynb uzantılı notebook açılacak. Burada Runtime > Change runtime type seçeneği ile python versiyonu ve CPU/GPU/TPU seçeneklerinden birini seçebilirsiniz.

3. Yazdığımız şeyleri çalıştırabilmek için bir doğrulama işlemi yapılmalıdır.:

        !apt-get install -y -qq software-properties-common python-software-properties module-init-tools

        !add-apt-repository -y ppa:alessandro-strada/ppa 2>&1 > /dev/null

        !apt-get update -qq 2>&1 > /dev/null

        !apt-get -y install -qq google-drive-ocamlfuse fuse

        from google.colab import auth

        auth.authenticate_user()

        from oauth2client.client import GoogleCredentials

        creds = GoogleCredentials.get_application_default()

        import getpass

        !google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret} < /dev/null 2>&1 | grep URL

        vcode = getpass.getpass()
        
        !echo {vcode} | google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret}

    Bu işlemi çalıştırdıktan sonra gönderilen doğrulama kodunu "Enter verification code" yazan kısma yazmalı ve bu işlemi 2 kere tekrarlamalısınız.

4. Doğrulama işlemi tamamlandıktan sonra çalıştırılması gereken ilk komut şu şekildedir:

        !mkdir -p drive

        !google-drive-ocamlfuse drive

5. Collab içinde "mkdir", "rm", "mv", "ls" ve benzeri linux terminal komutları başında '!' işareti ile kullanılabilmektedir. Ancak bir klasöre girmek için "cd" komutu yerine python kodu kullanılmaktadır:

        import os
        os.chdir('drive')

6. Drive'ınıza bir github reposunu indirmek ve onunla çalışmak için bir terminal komutu olan "git clone" kullanılabilir. Biz deneme yapmak için MLND-Capstone Repository'sini kullandık:

        git clone https://github.com/mvirgo/MLND-Capstone.git

7. Kodunuzda kullandığınız kütüphaneleri yükleme işlemi şu şekilde yapılılmalıdır. Biz çalıştıracağımız kod için keras ve opencv indirdik:

        !pip install -q keras
        !apt-get -qq install -y libsm6 libxext6 && pip install -q -U opencv-python

8. Bulunduğunuz yerden python dosyasını çalıştırmak şu şekildedir:

        !python3 MLND-Capstone/draw_detected_lanes.py

Eğer çalıştıracağınız python dosyası MLND-Capstone klasörünün içinden bazı kaynakları kullanıyorsa, mesela klasördeki bir modeli yüklüyorsa, bu şekilde bir çalıştırma hataya sebep olacaktır. Klasörün içine girip çalıştırmanız gerekmektedir.

        os.chdir('MLND-Capstone')
        !python3 draw_detected_lanes.py