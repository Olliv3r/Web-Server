# Configurar o apache
![Main](https://github.com/Olliv3r/Web-Server/raw/main/media/main.jpg)

### Etapa 1:
#### Instalar dependencias:
```
apt update -y && apt upgrade -y && apt install nano apache2 openssl openssl-tool -y
```

Execute o apache:
```
apachectl
```

Abra o link:
```
termux-open-url http://localhost:8080
```

### Etapa 2:
#### Alterar o arquivo httpd.conf

Abra o arquivo `httpd.conf` faremos alteraçôes nele:
```
nano $PREFIX/etc/apache2/httpd.conf
```

Dentro do arquivo digite a sequência de teclas `ctr+w` e pesquise essa linha e der enter:
> ServerRoot "/data/data/com.termux/files/usr"

Abaixo deste comando pesquisado, adicione este comando:
```
ServerName "localhost"
```

Ficando assim:
![Main](https://github.com/Olliv3r/Web-Server/raw/main/media/httpd.conf.jpg)

Digite `ctr+w` e pesquise por `ssl` e der enter e descomente esta linha
![Main](https://github.com/Olliv3r/Web-Server/raw/main/media/httpd.conf-ssl.jpg)

Digite `ctr+w` e pesquise por `shmcb` e der enter e descomente esta linha
![Main](https://github.com/Olliv3r/Web-Server/raw/main/media/httpd.conf-shmcb.jpg)

Digite `ctr+w` e pesquise por `httpd-ssl` e der enter e descomente esta linha
![Main](https://github.com/Olliv3r/Web-Server/raw/main/media/httpd.conf-httpd-ssl.jpg)

Digite `ctr+w` e pesquise por `httpd-vhosts` e der enter e descomente esta linha
![Main](https://github.com/Olliv3r/Web-Server/raw/main/media/httpd.conf-httpd-vhosts.jpg)

Nesse arquivo ja terminamos, agora salve as alteraçôes no arquivo digitando: `ctr+x+y` e der enter. 

### Etapa 3:
#### Configurar o certificado ssl

Crie um diretório chamado `ssl` para armazenar as certificaçôes e der as parmissôes necessárias a ele:
```
mkdir $PREFIX/etc/apache2/ssl
chmod 700 $PREFIX/etc/apache2/ssl
```

Baixe o certificado ssl e no final da transferência vai dando ENTER quando perguntar:
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout $PREFIX/etc/apache2/ssl/cert.key -out $PREFIX/etc/apache2/ssl/cert.crt
```

#### Alterar o arquivo httpd-ssl.conf
Abra o arquivo `httpd-ssl.conf` que faremos altereçôes nele:
```
nano $PREFIX/etc/apache2/extra/httpd-ssl.conf
```

Dentro deste arquivo digite `ctr+w` e pesquise por `ServerName` e substitua somente onde está escrito: `www.example.com` ou outro valor qualquer por `localhost` que ficará assim: `ServerName localhost:8443`
![Main](https://github.com/Olliv3r/Web-Server/raw/main/media/httpd-ssl.jpg)

Digite `ctr+w` e pesquise por `server.crt` e substitua por `ssl/cert.crt` que ficará assim:
> SSLCertificateFile "/data/data/com.termux/files/usr/etc/apache2/ssl/cert.crt"
![Main](https://github.com/Olliv3r/Web-Server/raw/main/media/httpd-ssl-cert.crt.jpg)

Digite `ctr+w` e pesquise por `server.key` e substitua por `ssl/cert.key` que ficará assim:
> SSLCertificateKeyFile "/data/data/com.termux/files/usr/etc/apache2/ssl/cert.key"
![Main](https://github.com/Olliv3r/Web-Server/raw/main/media/httpd-ssl-cert.key.jpg)

Digite `ctr+w` e pesquise por `DocumentRoot` e substitua o seu valor ou outro qualquer por:
> /data/data/com.termux/files/usr/share/apache2/default-site/htdocs`

Ficando assim:
![Main](https://github.com/Olliv3r/Web-Server/raw/main/media/httpd-ssl-DocumentRoot.jpg)

Nesse arquivo ja terminamos, agora salve as alteraçôes no arquivo digitando: `ctr+x+y` e der enter.

#### Alterar o arquivo httpd-vhosts.conf
Abra o arquivo `httpd-vhosts.conf` que faremos altereçôes nele:
```
nano $PREFIX/etc/apache2/extra/httpd-vhosts.conf
```

Digite `ctr+w` e pesquise por `DocumentRoot` e substitua o seu valor ou outro qualquer por:
> /data/data/com.termux/files/usr/share/apache2/default-site/htdocs

Ficando assim:
![Main](https://github.com/Olliv3r/Web-Server/raw/main/media/httpd-vhosts-DocumentRoot.jpg)

Logo após o `DocumentRoot` em `ServerName` substitua o seu valor `dummy-host.example.com` ou outro qualquer por `localhost` que ficará assim: `ServerName localhost`
![Main](https://github.com/Olliv3r/Web-Server/raw/main/media/httpd-vhosts-ServerName.jpg)

No final da tag `<VirtualHost>` antes do fechamento da tag `</VirtualHost>` adicione esta linha com o comando:  `Redirect / https://localhost:8443` 
![Main](https://github.com/Olliv3r/Web-Server/raw/main/media/httpd-vhosts-Redirect.jpg)

Repita essas alteraçôes também no bloco de baixo, adicionando um novo valor nas palavras `DocumentRoot`, `ServerName` e não esqueça de adicionar o redirecionamento com `Redirect / https://localhost:8443`

No final das alteraçôes feitas nos blocos `<VirtualHost> ` ficarão assim:  
```
<VirtualHost *:8080>
    ServerAdmin webmaster@dummy-host.example.com
    DocumentRoot "/data/data/com.termux/files/usr/share/apache2/default-site/htdocs"
    ServerName localhost
    ServerAlias www.dummy-host.example.com
    ErrorLog "var/log/apache2/dummy-host.example.com-error_log
    CustomLog "var/log/apache2/dummy-host.example.com-access_log" common
    Redirect / https://localhost:8443
</VirtualHost>
```
![Main](https://github.com/Olliv3r/Web-Server/raw/main/media/httpd-vhosts-VirtualHost.jpg)

Nesse arquivo ja terminamos, agora salve as alteraçôes no arquivo digitando: `ctr+x+y` e der enter.

Reinicie o apache com o comando:
```
apache2 -k restart
```
Atualize a página acessada anteriormente e verás que o apache te redirecionará para uma conexão https com certificado ssl.

## Proximas etapas (opcional)
<a href="https://github.com/Olliv3r/Web-Server-Php">1 - Configurar apache pra rodar scripts PHP</a>  
<a href="https://github.com/Olliv3r/Web-Server-Mysql">2 - Configurar phpmyadmin pra rodar no apache</a>

