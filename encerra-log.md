### **Cenário:** "São João": o que está sendo gravado neste arquivo de log?

**Nível:** Fácil

**Tipo:** Fixar

**Etiquetas:** python bash

**Acesso:** Público

**Descrição:** Um desenvolvedor criou um programa de teste que está escrevendo continuamente em um arquivo de log */var/log/bad.log* , preenchendo o disco. Você pode verificar isso, por exemplo tail -f /var/log/bad.log, com o

comando `ls -l`. Este programa não é mais necessário. Localize-o e encerre-o. Não exclua o arquivo de log.

**Acesso de root (sudo):** Verdadeiro

**Teste:** O tamanho do arquivo de log não muda (dentro de um intervalo de tempo maior que a taxa de variação do arquivo de log).


### Solução: 

Verificar o log com o caminho informado pelo problema

```bash
tail -f /var/log/bad.log
```

Verificar arquivo

```bash
admin@ip-10-1-13-35:~$ ls
```

Tentativa de zerar o arquivo no diretório do usuário (nota: para zerar o log real e parar o preenchimento, é necessário matar o processo e aplicar o truncate em /var/log/bad.log

```bash
truncate -s 0 badlog.py
```

Confirmar se ele continuou gerando log

```bash
tail -f /var/log/bad.log
```

Descubrir qual é o ID do processo (PID) do script que está rodando:

```bash
ps aux | grep badlog
```

Resultado:

```bash
admin        588  0.0  1.7  12508  8200 ?        S    17:49   0:00 /usr/bin/python3 /home/admin/badlog.py
admin        742  0.0  0.1   5264   640 pts/1    S<+  17:54   0:00 grep badlog
```

Finalizar o processo utilizando o comando `kill` seguido do PID encontrado no passo anterior:
```bash
kill <PID>
```

```bash
kill 588
```

Confirmar que o processo foi encerrado

```bash
tail -f /var/log/bad.log
```
