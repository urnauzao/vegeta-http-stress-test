- Documentação da ferramenta Vegeta
https://github.com/tsenart/vegeta

- Selecione seu Binários:
https://github.com/tsenart/vegeta/releases

- Vídeo Tutorial
[https://youtu.be/FNj0e1ZT4fI](https://youtu.be/FNj0e1ZT4fI)

- Clonando o binário
> curl -LO https://github.com/tsenart/vegeta/releases/download/v12.11.0/vegeta_12.11.0_linux_386.tar.gz

- Extraindo o binário
> tar -zxvf vegeta_12.11.0_linux_386.tar.gz

- Movendo o binário para pasta do sistema
> sudo mv ./vegeta /usr/bin/vegeta

- Testando o Vegeta
> vegeta --help

- Via terminal
> echo "GET https://nginx.urnau.com.br" | vegeta attack -rate=10 -duration=30s | vegeta report

- Via atack.list
> vegeta attack -duration=5s -rate=5 -targets=atack.list | vegeta report

- Customizar saida
> vegeta attack -duration=30s -rate=100 -targets=atack.list | vegeta encode | jq -c 'del(.body,.headers)' | vegeta encode > results.json

- Gerar gráfico da saída
> vegeta plot -title='Teste de Ataque' results.json > results.html

- Outros comandos
> vegeta attack -duration=30s -rate=1000 -name='Second' -tar
> gets=atack.list | vegeta encode | jq -c 'del(.body,.headers)'
> | vegeta encode > results2.json
> ###
> vegeta plot -title='Teste de Ataque' results.json results2.json > results2.html

> ###
> echo "GET https://nginx.urnau.com.br"| vegeta attack -duration=20s -rate=5000 -name="5k" | vegeta report -type=json
```json
{"latencies":{"total":56333143851600,"mean":563331438,"50th":60648,"90th":1412939033,"95th":1833621254,"99th":4292365164,"max":5075331800,"min":18900},"bytes_in":{"total":41545340,"mean":415.4534},"bytes_out":{"total":0,"mean":0},"earliest":"2023-09-14T23:53:50.4863227Z","latest":"2023-09-14T23:54:10.4866898Z","end":"2023-09-14T23:54:12.6219268Z","duration":20000367100,"wait":2135237000,"requests":100000,"rate":4999.9082266845,"throughput":1858.2732061059949,"success":0.41134,"status_codes":{"0":58866,"200":41134},"errors":["Get \"https://nginx.urnau.com.br\": dial tcp 0.0.0.0:0-\u003e191.101.1.112:443: socket: too many open files"]}
```

# Cheque a quantidade de arquivos que podem estar abertos no seu sistema operacional
- Isto pode causar HTTP Status Code 0
> ulimit -n


# Instalar o JQ para edição do output
> sudo apt install jq


# Relatório [report]:
- Exemplo de retorno ao se utilizar a saída do tipo `report`

Exemplo: `echo "GET https://nginx.urnau.com.br" | vegeta attack -rate=10 -duration=30s | vegeta report`
```txt
    Requests      [total, rate, throughput]         25, 5.21, 2.62
    Duration      [total, attack, wait]             4.971s, 4.8s, 171.63ms
    Latencies     [min, mean, 50, 90, 95, 99, max]  169.844ms, 217.316ms, 170.789ms, 373.49ms, 559.211ms, 598.267ms, 598.267ms
    Bytes In      [total, mean]                     14882, 595.28
    Bytes Out     [total, mean]                     0, 0.00
    Success       [ratio]                           52.00%
    Status Codes  [code:count]                      200:13  404:12
```

O relatório gerado pelo Vegeta fornece informações sobre o resultado do teste de estresse que você executou. Vamos analisar cada seção do relatório:

- Requests [total, rate, throughput]:
Total: O número total de solicitações feitas durante o teste, que é 25 no seu caso.
Rate: A taxa média de solicitações por segundo (RPS) durante o teste, que é 5.21.
Throughput: A taxa média real de solicitações por segundo que foram efetivamente atendidas pelo servidor, que é 2.62. Isso indica quantas solicitações por segundo o servidor conseguiu processar com sucesso.

- Duration [total, attack, wait]:
Total: A duração total do teste, que é 4.971 segundos.
Attack: A duração durante a qual as solicitações foram efetivamente enviadas (o tempo em que o ataque aconteceu), que é 4.8 segundos.
Wait: A duração média que cada solicitação teve que esperar antes de ser atendida pelo servidor, que é 171.63 milissegundos (ms).

- Latencies [min, mean, 50, 90, 95, 99, max]:
Min: A latência mínima observada durante o teste, que é 169.844 ms.
Mean: A latência média de todas as solicitações, que é 217.316 ms.
50: A mediana (percentil 50) das latências, que é 170.789 ms.
90: O percentil 90 das latências, que é 373.649 ms.
95: O percentil 95 das latências, que é 559.211 ms.
99: O percentil 99 das latências, que é 598.267 ms.
Max: A latência máxima observada durante o teste, que também é 598.267 ms. As latências indicam o tempo que as solicitações levaram para serem processadas pelo servidor.

- Bytes In [total, mean]:
Total: O número total de bytes recebidos do servidor durante o teste, que é 14,882 bytes.
Mean: A média de bytes recebidos por solicitação, que é 595.28 bytes.

- Bytes Out [total, mean]:
Total: O número total de bytes enviados para o servidor durante o teste, que é 0 bytes. Isso pode indicar que as solicitações não incluíram corpo (payload) de saída.
Mean: A média de bytes enviados por solicitação, que é 0 bytes.

- Success [ratio]:
Ratio: A porcentagem de solicitações bem-sucedidas em relação ao total de solicitações, que é 52%. Isso significa que 52% das solicitações resultaram em uma resposta com código de status 200 (sucesso).

- Status Codes [code:count]:
Aqui são apresentados os códigos de status das respostas recebidas durante o teste, juntamente com o número de ocorrências de cada código. No seu caso, 13 solicitações resultaram em um código de status 200 (sucesso) e 12 solicitações resultaram em um código de status 404 (não encontrado).
Essas informações do relatório ajudam a entender como o servidor respondeu durante o teste de estresse, incluindo métricas de desempenho, latência, sucesso e códigos de status. Você pode usar essas informações para avaliar o desempenho do servidor sob carga e identificar possíveis problemas.