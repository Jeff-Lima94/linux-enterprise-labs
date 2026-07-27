
LAB 02 — Linux Permissions & ACL
Objetivo

Aprender a administrar permissões de arquivos e diretórios em Linux utilizando:

chmod
chown
chgrp
umask
Sticky Bit
SUID
SGID
ACL (Access Control Lists)

Ao final deste laboratório você será capaz de controlar acessos de forma granular, aplicar boas práticas de segurança e solucionar problemas comuns relacionados a permissões.

Cenário Corporativo

Imagine que você trabalha como Analista Linux em uma empresa.

Existem três departamentos:

Financeiro
RH
TI

Cada departamento possui usuários próprios.

O gestor solicitou:

Cada equipe deve acessar apenas seus arquivos.
O diretório compartilhado deve permitir colaboração entre membros do mesmo grupo.
Apenas administradores poderão alterar permissões.
Um usuário externo deverá acessar apenas um arquivo específico.
Nenhuma permissão 777 poderá ser utilizada.

Esse é um cenário bastante comum em ambientes corporativos.
