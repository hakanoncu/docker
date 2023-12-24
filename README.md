# Docker

Docker Notlarım

## Docker Kurulumu
burada anlatılacak

## docker pull
[dockerhub](https://hub.docker.com) üzerinden ubuntu, node, redis, mongo gibi imajları indirmek için:

    docker pull ubuntu
    docker pull node
    docker pull redis
    docker pull mongo


## docker images
local olarak indirilmiş olan tüm image dosyalarını listeleme:

    docker images


## docker run
İmajların çalıştırılarak container elde edilmesi:

    docker run redis
    
redis yükü değilse önce **pull** eder. daha sonra da **run** eder. 
image ile container oluşturur.

    docker run -it ubuntu

**-it** parametresi : interactive terminal
ubuntu üzerinde terminal çalıştırır.


## docker ps
**ps**: process
ps bilgisayarda çalışan processler.

Terminal üzerinde:

    ps -ef

ile linux üzerinde çalışan tüm processler görüntülenebilir.

Şu anda ayakta olan, çalışır durumdaki tüm docker containerları listelemek için:

    docker ps
    docker container ls

Şu anda ayakta olan, çalışır durumdaki ve **geçmişteki** tüm docker containerları listelemek için:

    docker ps -a
    docker ps --all
    docker container ls -a

## docker run --name
name parametresi container için bir isim oluşturur.
Ayrıca verdiğimiz **tüm parametreleri de kaydeder.**
Yani name parametresi bizim için bir template, şablon oluşturur. 

    docker run -it --name bash_ubuntu ubuntu

## docker start stop
daha önce oluşuturulmuş herhangi bir container için başlatma veya durdurma işlemi yapar. **container_name** veya **container_id** ile çağırılabilir.

name parametresi ile isimlendirilmiş bash_ubuntu container çalıştırmak için:

    docker start bash_ubuntu
**bash_ubuntu** isimli container **-it** parametresi ile birlikte kaydedilmişti. O yüzden bu kod aslında bizim için **ubuntu** imajını çalıştırırken 
bu kodu çalıştırmış oldu.
