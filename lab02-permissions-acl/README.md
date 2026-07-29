
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


Parte 1 — Revisão das Permissões Linux
Objetivo

Nesta etapa você aprenderá:

Como o Linux controla acesso aos arquivos.
O significado de r, w e x.
A diferença entre Arquivo e Diretório.
Como visualizar permissões.
Como interpretar o resultado do ls -l.
Como alterar permissões utilizando modo simbólico e numérico.
Cenário Corporativo

Você acabou de assumir um servidor RHEL utilizado pela equipe Financeira.

Durante uma auditoria foi identificado que alguns arquivos estão acessíveis para usuários que não deveriam ter acesso.

Sua missão é:

identificar as permissões atuais;
compreender quem possui acesso;
corrigir as permissões seguindo o princípio do menor privilégio.
Teoria

No Linux todo arquivo possui um dono (owner) e um grupo (group).

Além disso existem três categorias de acesso.

Owner (u)

Group (g)

Others (o)

Cada categoria possui três possíveis permissões.

r = read

w = write

x = execute

Logo:

rwx

r--

rw-

r-x

---

são apenas combinações possíveis.

Como visualizar permissões

Comando:

ls -l

Exemplo:

-rwxr-x--- 1 jeff financeiro 2048 Jul 27 20:10 relatorio.sh

Vamos interpretar cada campo.

-rwxr-x---

Separando: - rwx r-x ---
Primeiro caractere

Indica o tipo.

-

arquivo
d

diretório
l

link simbólico

Também existem outros tipos (como dispositivos de bloco e soquetes), mas veremos isso mais adiante.

Owner
rwx

Significa:

read

write

execute

O dono possui controle total.

Group
r-x

Pode:

✔ Ler

✔ Executar

✖ Alterar

Others
---

Não possuem acesso.

Diferença entre Arquivo e Diretório

Esse ponto é frequentemente cobrado em entrevistas.

Arquivo
Permissão	Significado
r	Ler o conteúdo
w	Alterar o conteúdo
x	Executar como programa/script
Diretório

As permissões têm outro significado.

Permissão	Significado
r	Listar arquivos
w	Criar, remover e renomear arquivos (dependendo das permissões do diretório e do sticky bit)
x	Entrar no diretório e acessar seus arquivos

Exemplo:

drwx------

Você consegue:

entrar
listar
criar arquivos
apagar arquivos

Agora veja:

dr--r--r--

Você consegue listar o conteúdo, mas não consegue entrar nos arquivos se faltar permissão de execução no diretório.

Importante: Em diretórios, a permissão x é essencial para navegar (cd) e acessar itens internos. Ter apenas r normalmente permite listar nomes, mas não percorrer o diretório.

Visualizando mais informações
ls -lh

Human readable.

ls -la

Mostra arquivos ocultos.

ls -ld diretorio

Mostra somente o diretório.

Muito utilizado para troubleshooting.



Exercício Prático 1

Crie um ambiente de testes.

mkdir ~/lab-permissoes

cd ~/lab-permissoes

touch arquivo1.txt

touch script.sh

mkdir projetos

ls -l

Resultado esperado:

arquivo1.txt

script.sh

projetos/


Exercício Prático 2

Visualize detalhes.

ls -l

ls -lh

ls -la

ls -ld projetos

Observe:

tipo do arquivo;
permissões;
dono;
grupo;
tamanho;
data de modificação.
Entendendo o chmod

Existem duas formas.

Simbólica
u

g

o

a

Onde:

u = user

g = group

o = others

a = all

Operadores:


(+)  adiciona

(-) remove

(=) define exatamente

Exemplo:

chmod u+x script.sh

Adiciona execução apenas para o dono.

chmod g+w arquivo1.txt

Permite escrita para o grupo.

chmod o-r arquivo1.txt

Remove leitura de outros usuários.

Exercício
chmod u+rwx script.sh

chmod g+rx script.sh

chmod o-rwx script.sh

ls -l

Tente interpretar o resultado antes de prosseguir.

chmod Numérico

O método mais utilizado em servidores Linux.

Cada permissão possui um valor.

Permissão	Valor
r	4
w	2
x	1

Somamos os valores para obter a permissão desejada.

Exemplo:

rwx 4 + 2 + 1  7 

rw- 4 + 2   6

r-x 4 + 1  5

r--  4

---   0

Os exemplos mais comuns
Código	Permissão


777	rwxrwxrwx

755	rwxr-xr-x

750	rwxr-x---

700	rwx------

644	rw-r--r--

640	rw-r-----

600	rw-------



Exercício

Execute:

chmod 700 script.sh

chmod 644 arquivo1.txt

ls -l

Pergunta:

Por que o script.sh deixou de ser executável para outros usuários?

Porque a permissão 700 concede rwx apenas ao proprietário, removendo qualquer acesso para grupo e outros.

Como validar

Ao concluir esta etapa, valide que você consegue:

Explicar a diferença entre arquivo e diretório.
Interpretar uma saída do ls -l.
Dizer quem é o proprietário e o grupo de um arquivo.
Alterar permissões com chmod simbólico.
Alterar permissões com chmod numérico.
Identificar rapidamente permissões como 755, 644, 600 e 700.
Erros comuns

❌ Usar chmod 777 para "resolver" problemas de acesso.

❌ Aplicar permissões de arquivos em diretórios sem entender o efeito.

❌ Conceder permissão de execução a arquivos que não precisam ser executados.

❌ Alterar permissões recursivamente (chmod -R) sem verificar o impacto.

Perguntas de entrevista
Qual a diferença entre permissões em arquivos e diretórios?
O que significa a permissão 755?
Quando você utilizaria 600?
Qual é a diferença entre chmod u+x arquivo e chmod 755 arquivo?
Por que 777 é considerado uma má prática?
O que representa o primeiro caractere na saída do ls -l?

Esse é um cenário bastante comum em ambientes corporativos.


LAB 02 — Parte 2: Ownership (chown e chgrp)
Objetivo

Nesta etapa você aprenderá a:

Entender o conceito de proprietário (owner) e grupo (group).
Alterar a propriedade de arquivos e diretórios.
Alterar o grupo associado.
Aplicar alterações recursivamente.
Identificar e corrigir problemas de acesso causados por ownership incorreto.
Aplicar boas práticas utilizadas em ambientes Enterprise.
Cenário Corporativo

Você é responsável por um servidor Oracle Linux utilizado por três equipes:

Financeiro
RH
TI

O usuário joao, do departamento Financeiro, criou um diretório com relatórios mensais.

Ele saiu de férias e outro colaborador precisa continuar o trabalho.

Atualmente os demais usuários recebem:

Permission denied

Sua missão é descobrir o motivo e corrigir o acesso sem conceder permissões excessivas.

Revisando o ls -l

Execute:

ls -l

Exemplo:

-rw-r----- 1 joao financeiro 2548 Jul 28 10:35 relatorio.xlsx

Vamos interpretar:

Campo	Significado
-rw-r-----	Permissões
1	Número de links físicos
joao	Proprietário (owner)
financeiro	Grupo
2548	Tamanho
Jul 28 10:35	Última modificação
relatorio.xlsx	Nome do arquivo

Importante: as permissões (rwx) sempre são avaliadas considerando, nesta ordem: proprietário, grupo e outros. Por isso, o owner e o group influenciam diretamente o acesso.

Conceito de Owner

Todo arquivo possui um proprietário.

Normalmente, quem cria um arquivo torna-se seu dono.

Exemplo:

touch contrato.pdf

Agora:

ls -l contrato.pdf

Saída:

-rw-r--r-- 1 jeff financeiro 0 Jul 28 11:20 contrato.pdf

Nesse caso:

Owner: jeff
Grupo: financeiro
Alterando o proprietário (chown)

Sintaxe:

sudo chown novo_usuario arquivo

Exemplo:

sudo chown maria contrato.pdf

Valide:

ls -l contrato.pdf

Resultado esperado:

-rw-r--r-- 1 maria financeiro ...
Alterando proprietário e grupo ao mesmo tempo

Essa é uma prática muito comum em ambientes corporativos.

sudo chown maria:financeiro contrato.pdf

Agora:

Owner = maria
Grupo = financeiro
Alterando apenas o grupo (chgrp)

Também é possível alterar somente o grupo.

sudo chgrp rh contrato.pdf

Resultado:

-rw-r--r-- 1 maria rh ...



Exercício Prático 1

Crie um diretório para simular um compartilhamento:

mkdir ~/empresa

touch ~/empresa/relatorio1.txt

touch ~/empresa/relatorio2.txt

ls -l ~/empresa

Observe quem é o proprietário e o grupo.



Exercício Prático 2

Altere o grupo:

sudo chgrp financeiro ~/empresa/relatorio1.txt

Valide:

ls -l ~/empresa
Exercício Prático 3

Altere o proprietário:

sudo chown maria ~/empresa/relatorio2.txt

Valide novamente.

Alteração recursiva

Imagine um diretório com milhares de arquivos.

Alterar um por um seria inviável.

Para isso usamos:

sudo chown -R maria:financeiro /dados/projetos

O parâmetro -R significa recursivo, aplicando a alteração ao diretório e a todo o seu conteúdo.

Atenção com o -R

Antes de executar:

sudo chown -R

sempre confirme o caminho informado.

Um erro de digitação pode alterar a propriedade de grandes áreas do sistema e causar indisponibilidade de serviços.

Em ambientes corporativos, mudanças recursivas devem ser planejadas e, quando possível, testadas primeiro em um ambiente de homologação.



Troubleshooting
Problema 1

O usuário pertence ao grupo correto, mas ainda recebe:

Permission denied

Verifique:

id usuario

Exemplo:

uid=1002(maria)

groups=financeiro,rh

Confirme também as permissões do diretório:

ls -ld /dados

Lembre-se de que é necessário ter permissão de execução (x) em todos os diretórios do caminho para conseguir acessar um arquivo.

Problema 2

O proprietário está correto, mas o grupo não.


Verifique:

ls -l


Corrija:

sudo chgrp financeiro arquivo
Problema 3

As permissões parecem corretas.

Verifique se existe ACL aplicada.

getfacl arquivo

Na próxima parte do laboratório estudaremos ACL em profundidade.

Boas práticas

✔ Utilize grupos para controlar acesso sempre que possível.

✔ Evite alterar proprietários desnecessariamente.

✔ Documente mudanças de ownership em ambientes produtivos.

✔ Prefira conceder acesso por grupo em vez de compartilhar credenciais.

✔ Valide as alterações com ls -l após cada mudança.

✔ Use chown -R apenas quando tiver certeza do impacto.

Como validar

Ao final desta etapa você deve conseguir responder:

Quem é o proprietário de um arquivo?
Qual a função do grupo associado?
Quando usar chown?
Quando usar chgrp?
Qual a diferença entre alterar permissões e alterar ownership?
Quando utilizar a opção -R?



Perguntas de entrevista
Qual a diferença entre chmod, chown e chgrp?

Em qual ordem o Linux verifica permissões de acesso?

O que acontece se um usuário for owner de um arquivo, mas não pertencer ao grupo associado?

Quais os riscos de executar chown -R em um caminho incorreto?

Por que, em ambientes corporativos, normalmente utilizamos grupos em vez de conceder permissões individualmente?
