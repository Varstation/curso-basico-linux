# Curso de introdução ao Linux para bioinformática clínica - Prática guiada DIA 2

Este curso tem como objetivos abordar de forma teórico-prática os **conceitos e comandos básicos na utilização de um sistema Linux** visando capacitar os alunos a aplicar e analisar dados na prática da bioinformática clínica. Ao final do curso, o aluno deverá ser capaz de compreender a estrutura de um sistema Linux, acesso remoto à servidores, comandos básicos de manipulação de arquivos e pastas, execução de comandos, edição de arquivos e pipeline de análises. Curso desenvolvido pela equipe de bioinformática do Hospital Israelita Albert Einstein.

***

## Atalhos úteis

* **Ctrl+A** Move o cursor para o início da linha de comando.
* **Ctrl+E** Move o cursor para o final da linha de comando.
* **Ctrl+C** Interrompe um comando em andamento e retorna ao prompt
* **Ctrl+L** Limpa a tela do terminal.
* **Ctrl+U** Limpa a linha de comando do terminal.


## Direcionando a saída para escrita, visualizando e manipulando o conteúdo de arquivos

* O caracter **'>'** direciona o que seria impresso na tela para um arquivo. Caso o arquivo já exista, ele SERA SOBRESCRITO.
```
ls > list_dir.txt
```

* Os comandos **'less'** e **'more'** exibem o conteúdo do arquivo na tela. Use ESC para voltar ao seu prompt.
```
less list_dir.txt
more list_dir.txt
ls > list_dir.txt
less list_dir.txt
```

* O comando **'cat'** de concatenate exibe todo o conteúdo de um arquivo na tela. Mesmo arquivos muito grandes!  
Ele também pode ser usado para juntar arquivos.  
O comando **'echo'** imprime o conteúdo fornecido. 
```
cat list_dir.txt
echo "Lista:" > new_file.txt
cat new_file.txt list_dir.txt
cat new_file.txt list_dir.txt > list_dir_new.txt
less list_dir_new.txt
```

* Os caracteres **'>>'** também direcionam o que seria impresso na tela para um arquivo,  mas caso o arquivo já exista, ele NAO SERA SOBRESCRITO e o conteúdo é adicionado ao final do arquivo.
```
ls >> list_dir_new.txt
less list_dir_new.txt
```

* Os caracteres **'2>'** direcionam o erro padrão que seria impresso na tela para um arquivo.
```
ls nao_existe.txt
ls nao_existe.txt 2> log_erro.txt
less log_erro.txt
```

* O comando **'diff'** pode ser usado para identificar rapidamente diferenças entre arquivos.
```
diff new_file.txt list_dir_new.txt
```

* O comando **'paste'** é útil para tabelar listas, juntando arquivos
```
paste new_file.txt new_file.txt
paste new_file.txt list_dir_new.txt
```

## Analisando e alterando as permissões de escrita/leitura/execução

* O comando **'ls -l'** exibe informações detalhadas de arquivos e diretórios.
```
ls -l
```
![alt text](https://github.com/Varstation/curso-basico-linux/blob/master/img/files_linux.png "Linux")  
**Figura 2:** Padrão de permissões no Linux

* O comando **'chmod'** permite alterar permissões para os usuários. Vamos adicionar permissão de escrita para todos os usuários no arquivo criado?  
ugo -> user / group / others  
\+ - -> adicionar ou remover permissão  
rwx -> Read, write, execute  
  
ou  
  
4 = Read  
2 = Write  
1 = execute  
```
chmod o+w list_dir_new.txt
ls -l
chmod 755 list_dir_new.txt
ls -l
```

* O caracter `*` é utilizado como coringa, significa 'qualquer caracter'
```
ls *.txt
```

## Analisando o espaço em disco
* O comando **'df'** ou Display Free exibe os detalhes da utilização dos espaço em disco. 
```
df
df -h
```

## Verificando o uso e os processos ativos na máquina

* O comando **'top'** pode ser utilizado para verificar o uso da máquina. Use ESC para sair.
```
top
top -c
top -c -u user
``` 

Para terminar um processo em andamento, identifique-o no top pelo seu PID,  
pressione a tecla 'k' de kill, insira o PID e depois pressione enter.
```
sleep 120 &
top -c
``` 
## Enviando processos para background
As vezes queremos executar comandos mas não esperar até que terminem, travando o uso do terminal.  
Nesse caso, podemos enviar-los para background com '&'.
O comando **'time'** também pode ser sempre usado para exibir o tempo (em segundos) gasto em cada processamento.

```
sleep 10
sleep 10 &
time sleep 5
```

* O comando **'screen'** também é útil para rodar comandos no background. O usuário pode encerrar a conexão com  
o servidor sem correr o risco de interromper o processamento. Ao executar o comando, um novo terminal é aberto  
onde os comandos normais podem ser executados.  
Para sair do screen, use 'Ctrl + A + D'.

```
screen
```

Para ver os screens ativos (caso exista somente um, retorna automaticamente)
```
screen -rf
```

Para retornar para um screen especifico, utilize seu ID listado no comando anterior.
```
screen -r ID
```

## Fazer o download de arquivos

* O comando **'wget'** serve para baixar o conteúdo presente em arquivos presentes na web através do seu link.  
Muito usado para baixar arquivos, bancos de dados e repositórios, como FTP. Conhece o FTP do NCBI?  
O comando **'zcat'** pode abrir um arquivo compactado para visualização sem precisar descompactá-lo.
```
cd
mkdir newDir
cd newDir
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/genbank/fungi/Aspergillus_flavus/latest_assembly_versions/GCA_000952835.1_ASM95283v1/GCA_000952835.1_ASM95283v1_genomic.fna.gz
zcat GCA_000952835.1_ASM95283v1_genomic.fna.gz | less

wget https://github.com/Varstation/curso-basico-linux/raw/master/tarfile.tar.gz
```
* O comando **'git clone'** pode ser usado para copiar TODO o conteúdo disponível em um repositório no GitHub:
```
git clone https://github.com/Varstation/curso-basico-linux
```

## Organizando arquivos

* O comando **'tar'** é muito útil para agrupar arquivos e criar backups.  
Observe que fizemos o download de um arquivo compactado e zipado. Vamos descompacta-lo na pasta newDir.  
-x -> extrair do arquivo compactado  
-z -> zipar o arquivo com gzip  
-v -> imprimir na tela lista de arquivos processados  
-f -> para definir arquivos  
-c -> criar arquivo compactado  

```
ls *.gz
tar -xzvf tarfile.tar.gz
ls
```

Para criar arquivos tar.gz, usamos o comando abaixo de acordo com os arquivos de interesse:
```
tar -czvf newFile.tar.gz sample.fasta sample.sam
```
  
ou  

```
tar -cvf newFile.tar file1 file2
gzip newFile.tar
```

* Os comandos **'zip'** e **'unzip'** também são comuns para zipar.


## Formatos dos arquivos baixados:
![alt text](https://github.com/Varstation/curso-basico-linux/blob/master/img/fasta_file.png "FASTA")  
**Figura 3:** Formato FASTA padrão

![alt text](https://github.com/Varstation/curso-basico-linux/blob/master/img/fastq_file.png "FASTQ")  
**Figura 4:** Formato FASTQ padrão

![alt text](https://github.com/Varstation/curso-basico-linux/blob/master/img/bam_file.png "BAM")  
**Figura 5:** Formato BAM padrão

![alt text](https://github.com/Varstation/curso-basico-linux/blob/master/img/vcf_file.png "VCF")  
**Figura 6:** Formato VCF padrão

![alt text](https://github.com/Varstation/curso-basico-linux/blob/master/img/bed_file.png "BED")  
**Figura 7:** Formato BED padrão

## Visualizar somente partes de arquivos maiores 

* Os comandos **'head'** e **'tail'** mostram apenas as primeiras e últimas linhas de um arquivo.
```
head sample.fasta
head -n 5 sample.fasta
tail sample.fasta
```

## Pesquisando conteúdo dentro de arquivos

* O comando **'grep'** ou Global Regular Expression Print é muito útil para pesquisas rápidas de conteúdo de arquivos.  
-i -> insensitive  
-A -> imprimir numero linhas depois (after) da linha encontrada  
-B -> imprimir numero linhas antes (before) da linha encontrada  
-v -> inverte, imprime tudo que NAO foi encontrado  
-R -> todos arquivos de um diretório  
```
grep ">" sample.fasta
grep ">" sample.fasta -c
```

Também é possível fazer buscas com expressões regulares (REGEX), com **grep -E** ou **'egrep'**.  
Alguns caracteres especiais para REGEX, ou seja, eles são interpretados com outro significado.  

\+ -> Equivale a uma ou mais occorrências do caracter anterior: Ex: a+ procura 'a', 'aa', 'aaa'.  
\? -> Até uma repetição do caracter anterior, que é opcional: Ex: a? procura 'a' ou nada.  
\( -> Início da expressão.  
\) -> Fim da expressão.  
\| -> Procurar um termo OU outro separados pelo pipe '|'. Ex: '(D|R)NA' encontra 'DNA' ou 'RNA'.  
\{ -> Início de uma quantidade específica.  
\} -> Fim de uma quantidade específica. Ex 'a{2}' encontra exatamente 'aa'.  
\^ -> Representa início da linha.  
\$ -> Representa final da linha.  
\[ -> Início de uma lista para encontrar qualquer dos termos.  
\] -> Fim da lista. Ex: [aeiou].  
\\ -> Barra invertida, ou "escapar", desliga o caracter especial e ele é interpredado como tal. Ex: '\\[' é lido como '['  

```
grep -E 'NC' sample.fasta
egrep 'NC' sample.fasta
egrep '(poly|glyco)protein' sample.fasta
egrep '^>' sample.fasta
egrep ']$' sample.fasta
egrep '7{2}' sample.fasta
egrep '[' sample.fasta
egrep '\[' sample.fasta
```

## Juntando comandos em um pipeline

* O caracter **'|'** ou pipe é utilizado para concatenar/juntar comandos, onde a saída de um serve como entrada do próximo.  
Vamos começar com um exemplo simples de dois comandos apenas:

```
grep ">" sample.fasta | less
history | less
```
## Usando o editor de texto vi
Vamos editar o arquivo sample.fasta usando um editor de texto simples direto no terminal, chamado **'vi'**.  
Muito comum e geralmente disponível em todas as distribuições linux.  

Vamos editar os identificadores das sequências.  
No editor ativo, você pode navegar pelo arquivo livremente. Caso queira editar algo manualmente, pressione  
a tecla 'i'. Observe o surgimento da palavra "INSERT" no final da tela, é o indicativo de que você ativou corretamente  
essa função e pode editá-lo normalmente. Poderíamos editar cada identificador um a um, no entanto  
esse ferramenta também permite alterar todo o conteúdo de uma vez. Pressione 'ESC' para resetar o editor,  
depois shift' + ':'. Isso irá ativar o modo de inserção de comandos. Digite '%s/lcl|//' e pressione 'enter'.  
% -> editar todo o arquivo  
s/// -> substituir o conteúdo entre as duas primeiras barras pelo o que estiver entre as duas últimas.  
  
* Principais comandos do vi:  
shift' + ':' e digite 'wq!' -> Para sair salvando as alterações  
shift' + ':' e digite 'w' -> Para salvar as alterações e continuar com arquivo aberto  
shift' + ':' e digite 'q' -> Para sair sem salvar as alterações  
i -> mode inserção  
ESC -> Terminar modo inserção  
u -> Desfazer última modificação (undo)  
dd -> Deletar linha inteira  
dw -> Deletar palavra  

```
vi sample.fasta
```
