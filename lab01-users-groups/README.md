Lab 01
Gerenciamento de Usuários e Grupos
Objetivo

Aprender a criar, modificar e administrar usuários e grupos em ambientes Linux Enterprise.

Cenário

Imagine que um novo colaborador entrou na empresa.

Precisamos:

✔ Criar usuário

✔ Criar grupo

✔ Definir senha

✔ Expiração

✔ Diretório home

✔ Shell

✔ Permissões

Ambiente

Ubuntu Server 24

RHEL 9

Oracle Linux 9

Comandos utilizados
useradd

passwd

usermod

groupadd

groupmod

id

groups

whoami

chage

vipw
Exercício

Criar usuário

useradd jeff

Definir senha

passwd jeff

Criar grupo

groupadd cloud

Adicionar usuário

usermod -aG cloud jeff

Verificar

id jeff

Resultado

uid=1001

gid=1001

groups=1001,1002

Alterar shell

usermod -s /bin/bash jeff

Expiração

chage -l jeff

Bloquear usuário

passwd -l jeff

Desbloquear

passwd -u jeff

Excluir

userdel -r jeff
Desafio

Crie

3 usuários

2 grupos

Coloque usuários em grupos diferentes.

Resultado esperado

Mostrar saída do

id

groups

cat /etc/passwd

cat /etc/group
Conhecimento adquirido

Ao concluir este laboratório você saberá:

✔ Administração de usuários

✔ Administração de grupos

✔ Controle de acesso

✔ Expiração de contas

✔ Shell padrão

✔ Permissões

✔ Conceitos básicos de IAM em Linux
