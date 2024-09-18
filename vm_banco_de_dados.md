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
5. Verificar se o banco está ouvindo a porta correta (3306)
    - `netstat -tuln | grep 3306`[^2]
6. Editar o arquivo /etc/my.cnf modificar a seção [mysqld] para ficar:[^3]

    ```bash
    [mysqld]
    bind-address = 0.0.0.0
    port = 3306
    ```

7. Editar o arquivo `/etc/my.cnf.d/mariadb-server.cnf` Remova ou comente qualquer menção a skip-networking[^4]
8. Abrir banco e liberar geral as permissões para o user root
    - `GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY 'root' WITH GRANT OPTION;`[^5]
    - `FLUSH PRIVILEGES;`[^6]

[^1]: footnote.
