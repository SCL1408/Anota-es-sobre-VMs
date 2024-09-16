# Passos para a criação das VM

1. Criação da VM e inicialização do Alpine de acordo com documento na BB
2. Mudar controladora IDE, nas configurações da VM: remover disco do drive virtual.[^1]
3. Configurar interfaces de rede
    - Adicionar portas eth0 como host-only[^2] e eth1[^3] como NAT
    - Editar o arquivo `vi /etc/network/interfaces` para:

    ```bash
    auto lo
    iface lo inet loopback

    auto eth0
    iface eth0 inet dhcp

    auto eth1
    iface eth1 inet dhcp
    ```

4. Adicionar VIM
    - `apk update`
    - `apk upgrade`
    - `apk add vim`

5. Editar configurações do VIM para exibir numeração das linhas nos arquivos
    - Abrir arquivo `vim /etc/vim/vimrc`
    - Adicionar `set nu` abaixo da linha `set ruler`
    - É possível adicionar um comentário sobre o significado dessa linha adicionando à mesma linha o trecho `"Mostra numeração das linhas`
6. Edição do arquivo de repositórios[^4]
    - Abrir arquivo `vim /etc/apk/repositories`
    - remover o comentário do repositório de comunidade de modo a ficar:

    ```bash
    #/media/cdrom/apks
    http://dl-cdn.alpinelinux.org/alpine/u3.20/main
    http://dl-cdn.alpinelinux.org/alpine/u3.20/community
    ```

7. Adicionar Virtualbox Guest Additions [^5]
    - `apk update`
    - `apk upgrade`
    - `apk add virtualbox-gust-additions`
    - Reinicar a VM `reboot`
8. Adivar o serviço do Virtualbox Guest Additions
    - `rc-service virtualbox-guest-additions start`
    - Configurar o serviço para iniciar com o boot `rc-update add virtualbox-guest-additions boot`
9. Criar novo usuário
    - Criar grupo de usuários `addgroup <nome_do_grupo>` [^6]
        - `addgroup alunos`
    - Criar usuário propriamente dito `adduser -h <diretorio_home_do_usuario> -g <nome_do_grupo> <nome_do_usuario>` [^7]
        - `adduser -h /home/saler -g alunos saler`
10. Adicionar chave SSH [^8]
    - Encontrar chaves SSH na pasta de usuário no PC.
    - Abrir arquivo `id_rsa.pub` e copiar o conteúdo dele [^9]
    - Fazer login com usuario na VM
    - Criar pasta `mkdir /.ssh` e entrar nela.
    - Criar arquivo `authorized_keys` com o conteúdo copiado do arquivo `id_rsa.pub`
        - `echo "<aqui_voce_cola_sua_chave>" > authorized_keys` [^10]
11. Permitir acesso por chaves (como root user)
    - Abrir arquivo `vim /etc/ssh/sshd_config`
    - Habilitar a propriedade PasswordAuthentication e definir seu valor como não [^11]
        - `PasswordAuthenticatin no`
12. Instalar SUDO e dar permissões de admin para user
    - `apk update`
    - `apk upgrade`
    - `apk add sudo`
    - `visudo`[^12]
    - adicionar na última linha, no final do arquivo a liberação de permição `<nome_user> ALL=(ALL) ALL` (exemplo de: `saler ALL=(ALL) ALL`)[^13]
    - definir senha para o user para poder acessar sudo `passwd <user_name>` exemplo `passwd saler`

[^1]: Remover o disco do drive virtual é necessário para evitar conflitos de boot e liberar a controladora para outros usos..
[^2]: A interface eth0 como host-only permite a comunicação entre a VM e o host sem acesso externo, sendo ideal para testes internos de rede.
[^3]: A interface eth1 como NAT permite à VM acessar a internet por meio da rede do host, útil para atualizações de pacotes e conexão externa.
[^4]: Os repositórios de comunidade no Alpine oferecem pacotes adicionais e necessários para funcionalidades mais avançadas, como o VirtualBox Guest Additions.
[^5]: O VirtualBox Guest Additions melhora a integração e desempenho da VM, com recursos como drivers, pastas e clipboard compartilhados, além de suporte gráfico
[^6]: Criar um grupo de usuários facilita a gestão de permissões e políticas de acesso para múltiplos usuários que compartilham as mesmas funções.
[^7]: O comando `adduser` permite a criação de um novo usuário, especificando o diretório home e o grupo ao qual o usuário será atribuído.
[^8]: A chave SSH permite acesso remoto seguro à máquina sem necessidade de senha, utilizando criptografia de chave pública e privada.
[^9]: O arquivo `authorized_keys` armazena chaves públicas de usuários autorizados a acessar o servidor via SSH, garantindo segurança no acesso.
[^10]: O comando `echo` exibe uma linha de texto ou variável no terminal. No contexto acima, ele é usado para inserir a chave SSH no arquivo `authorized_keys`.
[^11]: Habilitar a autenticação por chave SSH e desabilitar a autenticação por senha aumenta a segurança, prevenindo acessos não autorizados por senhas fracas.
[^12]: O comando `visudo` abre o arquivo de configuração de sudoers de maneira segura, evitando erros de sintaxe que poderiam bloquear o acesso ao sudo.
[^13]: A linha `<nome_user> ALL=(ALL) ALL` no arquivo sudoers permite que o usuário especificado execute qualquer comando como administrador, com privilégios de root.
