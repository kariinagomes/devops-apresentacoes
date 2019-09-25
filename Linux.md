<h1>DevOps</h1>
Apresentação sobre Linux

<h2>Atalhos de teclado</h2>

```
tab
ctrl + r
```

<h2>Comandos Linux</h2>

Listar o conteúdo de um diretório

```
ls -la
```
> -l = traz detalhes dos arquivos como datas de criação, permissões, nome de proprietário e grupo, e tamanho.

> -a ou -all = inclui os arquivos ocultos na listagem

Verificar o diretório que o usuário esta situado.

```
pwd
```

Mudando de diretório

```
cd [diretório]
cd .. - Volta um diretório
cd / -  Volta para a raíz
cd ~ -  Volta para a home do usuário atual
cd - -  Volta para o diretório anterior
```

Ajuda sobre um comando

```
man [comando]
[comando] --help
```

Criar um diretório

```
mkdir [diretorio]
```

Remove um diretório

```
rmdir [diretorio]   
rmdir -r [diretorio]
```

Criando um arquivo

```
touch texto.txt          - Cria um arquivo vazio
> texto.txt              - Cria um arquivo vazio. Se o arquivo existir, esse comando limpa o conteúdo do arquivo.
echo "texto" > texto.txt - Cria o arquivo texto.txt com o conteúdo "texto".
vim texto.txt            - Abre uma instância do vim editando um arquivo vazio chamado texto.txt.
```
Concaternar um texto no final do arquivo

```
echo "texto" >> texto.txt - concatena no final do arquivo
```

Exibe o conteúdo de um arquivo

```
cat texto.txt
```

Criando um alias

```
alias [alias]="[comando]"
```

Removendo um alias

```
unalias [alias]
```

Exibir os processos em execução

```
ps 
ps -ef | grep [nome do processo]
```

> -a = Exibe todos os processos existentes.

> -e = Exibe as variáveis de ambiente relacionadas aos processos.

> -f = Exibe a árvore de execução dos processos.

> -l = Exibe mais campos no resultado.

> -m = Exibe a quantidade de memória ocupada por cada processo.

> -u = Exibe o nome do usuário que iniciou determinado processo e a hora em que isso ocorreu.

> -x = Exibe os processos que não estão associados a terminais.


Terminar um processo

```
kill -9 [PID/Nome do processo]
killall -9 [PID/Nome do processo]
```

<h2>Sistema de permissões</h2>
<b>-rwxrwxrwx</b>

<h4>Primeiro caractere = Natureza do arquivo:</h4>
<table>
  <thead>
    <th>Caractere</th>
    <th>Significado</th>
  </thead>
  <tbody>
    <tr>
      <td>-</td>
      <td>Arquivo regular</td>
    </tr>
    <tr>
      <td>d</td>
      <td>Arquivo de diretório</td>
    </tr>
    <tr>
      <td>l</td>
      <td>Link</td>
    </tr>
    <tr>
      <td>b</td>
      <td>Arquivos de dispositivo orientado a bloco</td>
    </tr>
    <tr>
      <td>c</td>
      <td>Arquivos de dispositivo orientado a caractere</td>
    </tr>
    <tr>
      <td>s</td>
      <td>Socket</td>
    </tr>
    <tr>
      <td>p</td>
      <td>Named pipe (ou FIFO)</td>
    </tr>
  </tbody>
</table>

<h4>Os próximos 3 grupos de 3 caracteres são as permissões referentes a Dono(UID)/Grupo/Outros</h4>
<table>
  <thead>
    <th>Caractere</th>
    <th>Significado</th>
    <th>Função</th>
  </thead>
  <tbody>
    <tr>
      <td>r</td>
      <td>Read</td>
      <td>Leitura</td>
    </tr>
    <tr>
      <td>w</td>
      <td>Write</td>
      <td>Escrita</td>
    </tr>
    <tr>
      <td>x</td>
      <td>Execute</td>
      <td>Execução</td>
    </tr>
  </tbody>
</table>

É possível alterar as permissões de um arquivo através do seguinte comando:

```
chmod -R [u/g/o][+/-][r/w/x] arquivo/diretório
Exemplo:
chmod -R u+rwx g-rwx o+rwx arquivo/diretorio
chmod -R 777 arquivo diretorio
```
Exemplo:
```
chmod -R g-x o-wx arquivo/diretorio
```
O exemplo acima remove a permissão de execução do grupo, como também remove as permissões de escrita e exeução de outros usuários.

A o primeiro grupo de opções ([u/g/o]) é referente ao tipo de usuário, onde:
<table>
  <thead>
    <th>Caractere</th>
    <th>Tipo de usuário</th>
  </thead>
  <tbody>
    <tr>
      <td>u</td>
      <td>Dono</td>
    </tr>
    <tr>
      <td>g</td>
      <td>Grupo</td>
    </tr>
    <tr>
      <td>o</td>
      <td>Outros</td>
    </tr>
  </tbody>
</table>

O segundo grupo de opções ([+/-]) indica a inclusão (+) ou remoção (-) das permissões indicadas no próximo grupo de opções ([r/w/x]).

Também é possível fazer a mudança de permissões através da indicação de um número octal entre 0 e 7. A tabela a seguir mostra o valor octal e suas repectivas permissões e valores em binário.
<table>
  <thead>
    <th>Dígito octal</th>
    <th>Permissões</th>
    <th>Valor binário</th>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>---</td>
      <td>000</td>
    </tr>
    <tr>
      <td>1</td>
      <td>--x</td>
      <td>001</td>
    </tr>
    <tr>
      <td>2</td>
      <td>-w-</td>
      <td>010</td>
    </tr>
    <tr>
      <td>3</td>
      <td>-wx</td>
      <td>011</td>
    </tr>
    <tr>
      <td>4</td>
      <td>r--</td>
      <td>100</td>
    </tr>
    <tr>
      <td>5</td>
      <td>r-x</td>
      <td>101</td>
    </tr>
    <tr>
      <td>6</td>
      <td>rw-</td>
      <td>110</td>
    </tr>
    <tr>
      <td>7</td>
      <td>rwx</td>
      <td>111</td>
    </tr>
  </tbody>
</table>

```
chmod -R 777 arquivo/diretorio
```

É possível mudar o dono e/ou o grupo de um arquivo/diretório com o seguinte comando:
```
chown novousuario arquivo/diretorio
chown novousuario:novogrupo arquivo/diretorio
chown :novogrupo arquivo/diretorio
```
<h2>Curiosidades</h2>

https://github.com/torvalds/linux - github do linux torvalds

https://www.kernel.org/doc/html/latest/ - documentacao do kernel

man 7 ascii - Lista a tabela ascii

[ whereis my brain?  

sudo apt-get install sl

sudo apt-get install cowsay

sudo apt-get install fortune

fortune | cowsay

sudo apt-get install cmatrix

telnet towel.blinkenlights.nl
