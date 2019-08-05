# Virtual Environment Kurulumu
*Bu anlatım `Linux ubuntu 5.0.0-23-generic #24~18.04.1-Ubuntu` üzerinde test edilerek hazırlanmıştır.*
İlk olarak basitçe Virtual Environment'ten bahsedelim, bundan sonra `venv` olarak kullanılacaktır. Python üzerinde proje geliştirirken projenize dahil ettiğiniz kütüphane sayısı çoğalmaya başladıkça, projenin bitimine yakın paketleme konusunda sıkıntı yaşayacağınızı düşünebilirsiniz. Bütün küyüphaneleri projenize statik olarak eklemeniz gerekebilir ya da bir dosya altına gerekli kütüphaneleri yazıp son kullanıcıların bu küyüphaneleri kurmalarını isteyebilirsiniz. 2 yöntemde aslında doğru ve geçerlidir ancak venv ile bu yöntem daha da kolaylaşıyor. 

Python altına bir kütüphane yüklediğimizi düşünelim. `pip install kutuphane` bu kütüphanemiz python yüklü olduğu dizine eklenecektir ve bundan sonra bu kütüphaneyi istediğimiz zaman kullanabileceğiz. venv projenizin çalışmış olduğu dizine aslında bir python yüklüyor. Bu şekilde projemize dahil ettiğimiz kütüphanelerde aslında çalışmakta olduğumuz dizin altına eklenmiş oluyor ve projemizi yüklemiş olduğumuz yeni python ile başlatırsak bir sorun yaşamadan projemizi aslında paketlemiş oluyoruz. 

Projemize dahil ettiğimiz kütüphaneleri başka bir projede kullanamayız çünkü her venv kendi projesine özgüdür. Şimdi aşağıdaki kodları içeren bir python projesi başlatalım.
```py
from flask import Flask
app = Flask(__name__)


@app.route('/')
def hello():
    return "Hello World!"

if __name__ == '__main__':
    app.run()
```
Projemizi başlatalım.
```sh
test@ubuntu:~/Desktop$ python app.py 
Traceback (most recent call last):
  File "app.py", line 1, in <module>
    from flask import Flask
ImportError: No module named flask

```
flask modülünün olmadığını görüyoruz. Şimdi bir venv kurup içerisine flask yüklemeyi deneyelim.
```sh
pip install virtualenv
```
Eğer python3 kullanıyorsanız:
```
pip3 install virtualenv
```
```sh
test@ubuntu:~/Desktop/deneme$ pip install virtualenv
DEPRECATION: Python 2.7 will reach the end of its life on January 1st, 2020. Please upgrade your Python as Python 2.7 won't be maintained after that date. A future version of pip will drop support for Python 2.7. More details about Python 2 support in pip, can be found at https://pip.pypa.io/en/latest/development/release-process/#python-2-support
Requirement already satisfied: virtualenv in /usr/local/lib/python2.7/dist-packages (16.7.2)
```
Bende daha önceden yüklü olduğu için yukarıdaki gibi bir çıktı aldım... venv'i etkinleştitirken kullanmış olduğunuz python sürümünü belirtmeniz gerekiyor. Şimdi proje dizini altında bu işlemi gerçekleştirelim.
```sh
test@ubuntu:~/Desktop/deneme$ whereis python2
python2: /usr/bin/python2 /usr/bin/python2.7 /usr/bin/python2.7-config /usr/lib/python2.7 /etc/python2.7 /usr/local/lib/python2.7 /usr/include/python2.7 /usr/share/man/man1/python2.1.gz
```
python2'nin /usr/bin/python2.7 altında olduğunu gördükten sonra bunu venv'e söylüyorum.
```sh
test@ubuntu:~/Desktop/deneme$ virtualenv -p /usr/bin/python2.7 test_projesi
Running virtualenv with interpreter /usr/bin/python2.7
Already using interpreter /usr/bin/python2.7
New python executable in /home/test/Desktop/test_projesi/bin/python2.7
Also creating executable in /home/test/Desktop/test_projesi/bin/python
Installing setuptools, pip, wheel...
done.
```
İşlem başarıyla tamamlandı. Şimdi venv'i etkinleştirip gerekli kütüphaneleri kuracağım...
```sh
test@ubuntu:~/Desktop/deneme$ source test_projesi/bin/activate
(test_projesi) test@ubuntu:~/Desktop/deneme$
```
Artık terminalimde (test_projesi) başlığı altında tanımlı bir shell ekranı mevcut. Şuanda venv'in içinde olduğumun anlamıdır bu. Python sürümümüzü kontrol edelim.
```sh
(test_projesi) test@ubuntu:~/Desktop/deneme$ python -V
Python 2.7.15+
```
venv altındıylen pip'i venv'e dahil edelim.
```
python -m pip install --upgrade pip`
```
Ve artık kütüphanemizi yükleyelim...
```sh
(test_projesi) test@ubuntu:~/Desktop/deneme$ pip install flask
DEPRECATION: Python 2.7 will reach the end of its life on January 1st, 2020. Please upgrade your Python as Python 2.7 won't be maintained after that date. A future version of pip will drop support for Python 2.7. More details about Python 2 support in pip, can be found at https://pip.pypa.io/en/latest/development/release-process/#python-2-support
Collecting flask
  Using cached https://files.pythonhosted.org/packages/9b/93/628509b8d5dc749656a9641f4caf13540e2cdec85276964ff8f43bbb1d3b/Flask-1.1.1-py2.py3-none-any.whl
Collecting click>=5.1 (from flask)
  Using cached https://files.pythonhosted.org/packages/fa/37/45185cb5abbc30d7257104c434fe0b07e5a195a6847506c074527aa599ec/Click-7.0-py2.py3-none-any.whl
Collecting Werkzeug>=0.15 (from flask)
  Using cached https://files.pythonhosted.org/packages/d1/ab/d3bed6b92042622d24decc7aadc8877badf18aeca1571045840ad4956d3f/Werkzeug-0.15.5-py2.py3-none-any.whl
Collecting itsdangerous>=0.24 (from flask)
  Using cached https://files.pythonhosted.org/packages/76/ae/44b03b253d6fade317f32c24d100b3b35c2239807046a4c953c7b89fa49e/itsdangerous-1.1.0-py2.py3-none-any.whl
Collecting Jinja2>=2.10.1 (from flask)
  Using cached https://files.pythonhosted.org/packages/1d/e7/fd8b501e7a6dfe492a433deb7b9d833d39ca74916fa8bc63dd1a4947a671/Jinja2-2.10.1-py2.py3-none-any.whl
Collecting MarkupSafe>=0.23 (from Jinja2>=2.10.1->flask)
  Using cached https://files.pythonhosted.org/packages/fb/40/f3adb7cf24a8012813c5edb20329eb22d5d8e2a0ecf73d21d6b85865da11/MarkupSafe-1.1.1-cp27-cp27mu-manylinux1_x86_64.whl
Installing collected packages: click, Werkzeug, itsdangerous, MarkupSafe, Jinja2, flask
Successfully installed Jinja2-2.10.1 MarkupSafe-1.1.1 Werkzeug-0.15.5 click-7.0 flask-1.1.1 itsdangerous-1.1.0
(test_projesi) test@ubuntu:~/Desktop/deneme$
```
Artık venv'in içinden çıkabiliriz.
```sh
(test_projesi) test@ubuntu:~/Desktop/deneme$ deactivate
test@ubuntu:~/Desktop/deneme$ 
```
Evet şimdi proje dizinimizin altına kurulmuş olan python altından projemizi başlatalım.

```sh
test@ubuntu:~/Desktop/deneme$ ls
app.py  test_projesi
test@ubuntu:~/Desktop/deneme$ cd test_projesi/
test@ubuntu:~/Desktop/deneme/test_projesi$ ls
bin  include  lib  local
test@ubuntu:~/Desktop/deneme/test_projesi$ cd bin/
test@ubuntu:~/Desktop/deneme/test_projesi/bin$ ls
activate  activate.csh  activate.fish  activate.ps1  activate_this.py  easy_install  easy_install-2.7  flask  pip  pip2  pip2.7  python  python2  python2.7  python-config  wheel
test@ubuntu:~/Desktop/deneme/test_projesi/bin$ ./python ../../app.py 
 * Serving Flask app "app" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)

```
Evet projemiz başarılı bir şekilde çalışıyor!
