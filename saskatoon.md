### Cenário: "Saskatoon": contagem de IPs.

**Nível: Fácil**
 
**Descrição: Existe um arquivo de log de acesso ao servidor web em /home/admin/access.log. O arquivo consiste em uma linha para cada requisição HTTP, com o endereço IP do solicitante no início de cada linha (primeira coluna).**

**Encontre o endereço IP que possui o maior número de requisições neste arquivo (não há empate; o IP é único). Escreva a solução em um arquivo /home/admin/highestip.txt. Por exemplo, se a sua solução for "1.2.3.4", você pode fazer echo "1.2.3.4" > /home/admin/highestip.txt**

**OBSERVAÇÃO: O IP da solução aparece 482 vezes, ou seja, grep -c -F -f highestip.txt access.logretorna 482, se a sua solução tiver um número diferente (menor), você selecionou o IP mais comum errado.**

**Teste: O checksum SHA1 do endereço IP sha1sum /home/admin/highestip.txté 6ef426c40652babc0d081d438b9f353709008e93(apenas uma forma de verificar a solução sem revelá-la; também aceitamos o IP correto sem quebra de linha no final do arquivo).**

Sistema Operacional: Debian 13

Acesso de root (sudo): Sim

### Solução:

```bash
awk '{print $1}' /home/admin/access.log | sort | uniq -c | sort -nr | head -n 1
```
#### Explicação dos comandos

```bash
awk '{print $1}' /home/admin/access.log
```
Lê o arquivo /home/admin/access.log e extrai apenas a primeira coluna (representada por $1) de cada linha. 

```bash
sort
```
```bash
uniq -c
```
Recebe a lista de IPs do comando anterior e a ordena alfabeticamente.

Remove as linhas duplicadas consecutivas e adiciona o prefixo -c (count), que exibe o número de ocorrências de cada IP à esquerda.

```bash
sort -nr
```
Reordena a lista baseando-se no número de ocorrências gerado no passo anterior.

* -n (numeric): Ordena considerando os valores como números (evita que o número 10 seja lido antes do 2).

* -r (reverse): Inverte a ordem para que o maior número fique no topo (primeira linha).
  
```bash
head -n 1
```
Filtra a saída para exibir apenas a primeira linha (-n 1), que corresponde ao IP com o maior número de requisições no arquivo.

Gerando o arquivo final

```bash
awk '{print $1}' /home/admin/access.log | sort | uniq -c | sort -nr | awk 'NR==1{print $2}' > /home/admin/highestip.txt
```

O operador > é um redirecionamento de saída no Linux, em vez de exibir o resultado do comando na tela do terminal, ele pega o resultado e salva diretamente dentro do arquivo /home/admin/highestip.txt.
