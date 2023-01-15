# Web-Server
Configura servidor web 

Instalação:
```
apt update && apt install apache2 openssl openssl-tool -y
```
Tente executar o apache:
```
apachectl
```

Abra o arquivo ```httpd.conf```
```
nano $PREFIX/etc/apache2/httpd.conf
```

Pressione a tecla ```ctrl+w``` e pesquise por: ```ServerRoot "/data/data/com.termux/files/usr"``` e adicione abaixo dele o comando:
> ServerName "localhost"

Pressione a tecla ```ctrl+w``` e pesquise por:
```ssl``` e descomente esta linha

Pressione a tecla ```ctrl+w``` e pesquise por:
```shmcb``` e descomente esta linha

Pressione a tecla ```ctrl+w``` e pesquise por:
```httpd-ssl``` e descomente esta linha

Pressione a tecla ```ctrl+w``` e pesquise por:
```httpd-vhosts``` e descomente esta linha

Pressione a tecla ```ctrl+x+y``` e enter para salvar o arquivo

Crie um diretório e defina as  permissôes:
```
mkdir -p $PREFIX/etc/apache2/ssl
chmod 700 $PREFIX/etc/apache2/ssl
```

Baixe o certificado ssl:
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout $PREFIX/etc/apache2/ssl/cert.key -out $PREFIX/etc/apache2/ssl/cert.crt
```

Abra o arquivo ```httpd-ssl``` e procure por ```ServerName www.example.com:8443``` e altere de ```www.example.com``` para ```localhost:8443```
```
nano $PREFIX/etc/apache2/extra/httpd-ssl.conf
```

Procure por ```server.crt``` e substitua por ```ssl/cert.crt```

Procure por ```server.key``` e substitua por ```ssl/cert.key```

Salve o arquivo saia.

Adicione o diretório ```htdocs``` ao ```httpd-vhosts```
``

Pressione a tecla ```ctrl+w``` e pesquise por ```DocumentRoot``` e adicione este diretório ```/data/data/com.termux/files/usr/share/apache2/default-site/htdocs```
nano $PREFIX/etc/apache2/extra/httpd-vhosts
```

Em ```ServerName dummy-host2.example.com``` substitua por ```ServerName localhost```

Adicionar este comando ```Redirect / https://localhost:8443``` abaixo do ```CustomLog```

Reinicie o apache e teste no navegador pra ver se ele é redirecionado para https
