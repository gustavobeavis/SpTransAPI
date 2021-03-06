# Cartão da Sorte API SPTrans - Descontinuado
A SPTRANS passou a fornecer dados por uma API aberta

API Rest que permite consultar linhas, paradas e previsão dos onibus de São Paulo.

## API
Todos os recursos retornam uma representação no formado JSON (`Content-Type: application/json; charset=utf-8`).

## Paradas

Listagem e busca das paradas de ônibus monitoradas pelo programa Olho Vivo. 

    /parada/{termo}

O valor do parâmetro `termo` será comparado com os atributos `Endereco` e `Nome` do recurso, p.ex.:

    http://127.0.0.1/cartaoDaSorte/parada/paulista

retornará o seguinte resultado:

    [
      {
        "CodigoParada": 260015039,
        "Nome": "PAULISTA B/C",
        "Endereco": " AV PAULISTA/ AV REBOUCAS ",
        "Latitude": -23.555883,
        "Longitude": -46.66306
      },
      {
        "CodigoParada": 260016855,
        "Nome": "PAULISTA C/B",
        "Endereco": "AV PAULISTA/ R ANTONIO CARLOS",
        "Latitude": -23.555176,
        "Longitude": -46.66237
      }
    ]

Se o parâmetro `termo` for omitido será retornada a lista completa de paradas.

## Linhas

Busca por linhas de ônibus.

    /linha/{termo}

Nesse recurso o parâmetro `termo` é obrigatório e será comparado ao atributos `DenominacaoTPTS` e `DenominacaoTSTP`, que são os nomes exibidos no letreiro dos ônibus da linha em cada sentido.

Exemplo:

    http://127.0.0.1/cartaoDaSorte/linha/imirim

retornará o seguinte resultado:

    [
      {
        "CodigoLinha": 568,
        "Circular": false,
        "Letreiro": "967A",
        "Sentido": 1,
        "Tipo": 10,
        "DenominacaoTPTS": "PINHEIROS",
        "DenominacaoTSTP": "IMIRIM",
        "Informacoes": null
      },
      {
        "CodigoLinha": 33336,
        "Circular": false,
        "Letreiro": "967A",
        "Sentido": 2,
        "Tipo": 10,
        "DenominacaoTPTS": "PINHEIROS",
        "DenominacaoTSTP": "IMIRIM",
        "Informacoes": null
      }
    ]


## Previsão

Previsão de chegada dos próximos ônibus de cada uma das linhas que atendem essa parada.

    /previsao/{termo}

O parâmetro `termo`, como o nome sugere, deve receber o valor indicado no atributo `CodigoParada` do recurso Parada. Por exemplo, o código da parada do cruzamento da Av. Paulista com a Rebolças é `260015039`, para consultar a previsão dos próximos ônibus nessa parada:

    http://127.0.0.1/cartaoDaSorte/previsao/260015039

O nome dos atributos na representação das previsões são bem menos intuitivos que os demais recursos, então segue a lista dos atributos e respectivos significados:

| atributo          | descrição
|-------------------|-------------------------------------------------------
| `hr`              | hora atual no servidor (HH:MM)
| `p`               | objeto Parada
| `p.cp`            | código da parada
| `p.px`            | longitude da parada
| `p.py`            | latitude da parada
| `p.np`            | nome da próxima parada
| `p.l`             | lista de linhas com previsão
| `p.l[n].c`        | código da linha
| `p.l[n].cl`       | código interno da linha
| `p.l[n].lt0`      | letreiro da linha, sentido 1
| `p.l[n].lt1`      | letreiro da linha, sentido 2
| `p.l[n].qv`       | quantidade de veículos com previsão na lista `vs`
| `p.l[n].sl`       | sentido da linha nas previsões \[1|2\]
| `p.l[n].vs`       | lista dos próximos veículos que passarão na parada
| `p.l[n].vs[m].a`  | acessibilidade \[true\|false\]
| `p.l[n].vs[m].p`  | prefixo do veículo
| `p.l[n].vs[m].px` | longitude atual do veículo
| `p.l[n].vs[m].py` | latitude atual do veículo
| `p.l[n].vs[m].t`  | previsão de chegada na parada (HH:MM)


## Referências 
[marcoshack / sptrans](https://github.com/marcoshack/sptrans)
