# Comandos de configuração da VM do banco de dados

1. Clonar VM base
2. Instalar mariadb
    - `sudo apk update`
    - `sudo apk upgrade`
    - `sudo apk add mariadb mariadb-client`[^1]
3. Configurar mariadb
    - Adicionar serviço na inicialização da máquina
        - `sudo apk update mariadb default`
    - Configurar setup inicial
        - `sudo /etc/init.d/mariadb setup`
    - Entrar no banco e atribuit senha
        - `sudo mysql -u root`
        - `ALTER USER 'root'@'localhost' IDENTIFIED BY '<sua_senha>';`
    - Ligar serviço de banco
        - `sudo rc-service mariadb start`
4. Criar banco de dados com nome de acordo com backend

[^1]: footnote.
