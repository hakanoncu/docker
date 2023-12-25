# Docker

Docker Notlarım

## Docker Kurulumu
burada anlatılacak
## Podman ile Docker-CLI Taklit Etme
1. **podman** i kurun.

	    sudo pacman -S podman

2.  **podman-docker** paketini kurun.

	    sudo pacman -S podman-docker

3.  *Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.*
uyarısını gidermek için

	    sudo touch /etc/containers/nodocker

	komutu ile boş bir nodocker dosyası oluşturun.
	
4. 	**/etc/containers/registries.conf** dosyasının en altına

	    unqualified-search-registries = ["docker.io"]
	eklemeleri yapın. Bunun için:
	
		sudo nano /etc/containers/registries.conf
		    
	ile dosyanın en altına inin ve eklemeleri yapıştırın. ctrl+x , e, enter diyerek kaydedin.
	
	> [[registry]] 
	> location = "docker.io" 
	> [registries.search] 
	> registries =['docker.io']
	> bu kodlar hataya sebep oluyor. bunları eklemeyin.

Bu, **varsayılan docker yapılandırması**na eşdeğerdir. Artık tüm podman komutlarını docker olarak kullanabilirsiniz.

    podman pull docker.io/library/redis:6

yerine

    docker pull redis:6

gibi kullanabilirsiniz.


## docker pull
[dockerhub](https://hub.docker.com) üzerinden ubuntu, node, redis, mongo gibi imajları indirmek için:

    docker pull ubuntu
    docker pull node
    docker pull redis
    docker pull mongo
imajlar locale indirilir ama çalıştırılmaz.

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
**bash_ubuntu** isimli container **-it** parametresi ile birlikte kaydedilmişti. O yüzden bu kod aslında bizim için **ubuntu** imajını çalıştırırken  **-it** parametresi ile birlikte çalıştırmış oldu.

## docker rm
**rm**: remove

Çalıştırılan her container fiziksel olarak diskimizde yer kaplar. ihtiyacımız olmayan containerları fiziksel olarak silmek için **container_name** veya **container_id** ile çağırılabilir.

    container rm bash_ubuntu

tüm containerları tek seferde silmek için:

    docker container rm $(docker container ls -aq)
**docker container ls -aq** sadece **container_id** lerini dördürür.

## docker tag

    docker pull redis
    docker run redis

komutları bize tag yapısı **latest** olan sürümü indirilir ve çalıştırılır. 
Fakat 

    docker run redis:6
dersek biz dockerhub üzerinde tag değeri 6 olan redis 6.0.14 sürümünü indirmiş oluruz.

    docker images
 altında 

| REPOSITORY | TAG |
|--|--|
| redis  | 6 |
| redis | latest |

> **Podman kullanımı için uyarı:**
> Error: short-name "redis:6" did not resolve to an alias and no unqualified-search registries are defined in "/etc/containers/registries.conf"

**podman**   veya  **podman-docker** kullanıyorsanız yukarıdaki şekilde hata verecektir. Bunu düzeltmenin 2 yolu var.
1. yöntem
	/etc/containers/registries.conf dosyasına

	    unqualified-search-registries = ["docker.io"]

	    [[registry]]
	    location = "docker.io"
	    
	    [registries.search]
	    registries = ['docker.io']

	eklemeleri yapın. Bu, **varsayılan docker yapılandırması**na eşdeğerdir.
	
	    sudo nano /etc/containers/registries.conf
	ile dosyanın en altına yapıştırın. ctrl+x , evet, enter diyin.
	
2. yöntem
	
	    docker run redis:6

	yerine

	    docker run docker.io/library/redis:6 

	olarak kullanmaktır.
	
	Her zaman kayıt defteri sunucusu (tam dns adı), ad alanı, görüntü adı ve etiketi 
	registry server (full dns name), namespace, image name, and tag
	Örneğin;
	registry.redhat.io/ubi8/ubi:latest
	docker.io/library/redis:6  
	gibi tam nitelikli görüntü adlarının kullanılmasını önerilir.


## docker image tag
Kendime ait bir tag vermek istersek

