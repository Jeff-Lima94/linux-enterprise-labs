
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


O que aprenderemos

O laboratório será dividido em etapas para facilitar o aprendizado e a documentação.

Parte 1 — Revisão de permissões Linux
rwx
Usuário
Grupo
Outros
chmod numérico
chmod simbólico

Parte 2 — Ownership
chown
chgrp

Parte 3 — umask
teoria
cálculo
impacto em arquivos novos

Parte 4 — Sticky Bit

Exemplos:

/tmp

chmod +t

Quando utilizar.

Quando NÃO utilizar.

Parte 5 — SGID

Diretórios compartilhados.

Herança de grupos.

Parte 6 — SUID

Funcionamento.

Riscos.

Segurança.

Parte 7 — ACL

O grande diferencial deste laboratório.

Você aprenderá:

getfacl
setfacl

ACL padrão

ACL recursiva

Máscara

Remover ACL

Parte 8 — Cenário Corporativo

Implementaremos todo o ambiente.

Será algo parecido com:

/empresa

    financeiro

    rh

    ti

    compartilhado

Com usuários reais criados no Lab 01.

Parte 9 — Troubleshooting

Problemas como:

"Usuário pertence ao grupo mas não acessa."

"Permissão aparentemente correta."

"ACL sobrescrevendo chmod."

"Máscara ACL bloqueando acesso."

"SGID não funcionando."

Todos esses casos serão reproduzidos e resolvidos.

Parte 10 — Entrevista Técnica

Perguntas como:

Qual a diferença entre chmod e ACL?

Quando utilizar ACL?

O que faz o Sticky Bit?

Diferença entre SGID e SUID?

Como descobrir porque um usuário não consegue acessar um diretório?

Cronograma do Lab

Vamos construir em pequenas entregas, como um projeto real:

Etapa 1 ✅
Revisão das permissões Linux

Etapa 2
chmod
chown
chgrp

Etapa 3
umask

Etapa 4
Sticky Bit

Etapa 5
SGID

Etapa 6
SUID

Etapa 7
ACL

Etapa 8
Cenário corporativo completo

Etapa 9
Troubleshooting

Etapa 10
Boas práticas e perguntas de entrevista
Um usuário externo deverá acessar apenas um arquivo específico.
Nenhuma permissão 777 poderá ser utilizada.

