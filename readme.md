#  Sistema de Gestão de Caixa
 
Sistema de terminal em **C** para gerenciamento de cédulas e moedas em caixa, com suporte a cadastro de denominações, exibição de extrato e simulação de vendas com troco automático.
 
---
 
##  Funcionalidades
 
- **Cadastrar Notas/Moedas** — Registra a quantidade disponível de cada denominação no caixa.
- **Ver Caixa** — Exibe um extrato detalhado com quantidade, subtotal por denominação e total geral.
- **Simular Venda e Troco** — Calcula e decompõe o troco automaticamente usando as notas/moedas disponíveis no caixa, atualizando o estoque após cada operação.
---
 
##  Denominações Suportadas
 
| Cédulas/Moedas |
|----------------|
| R$ 200,00 |
| R$ 100,00 |
| R$ 50,00 |
| R$ 20,00 |
| R$ 10,00 |
| R$ 5,00 |
| R$ 2,00 |
| R$ 1,00 |
| R$ 0,50 |
| R$ 0,25 |
| R$ 0,10 |
| R$ 0,05 |
| R$ 0,01 |
 
---
 
##  Estrutura do Código
 
```
caixa.c
│
├── Constantes globais
│   ├── VALORES[]   — valores em centavos de cada denominação
│   └── NOMES[]     — rótulos formatados para exibição
│
├── Tipo Caixa (struct)
│   └── quantidade[13] — estoque de cada denominação
│
├── Funções utilitárias
│   ├── limpar_tela()         — compatível com Windows e Unix
│   ├── linha()               — desenha separadores no terminal
│   ├── cabecalho()           — exibe título formatado
│   ├── limpar_buffer()       — limpa o buffer de entrada
│   ├── ler_valor_centavos()  — lê valores monetários com validação
│   ├── ler_inteiro()         — lê inteiros não-negativos com validação
│   └── aguardar()            — pausa aguardando ENTER
│
├── Funções principais
│   ├── saldo_centavos()   — calcula o total em caixa
│   ├── cadastrar()        — registra quantidades no caixa
│   ├── ver_caixa()        — exibe extrato completo
│   ├── decompor()         — algoritmo guloso de decomposição do troco
│   └── simular_venda()    — processa venda e aplica o troco
│
└── main()
    └── Loop do menu principal
```
 
---
 

 
##  Detalhes de Implementação
 
### Algoritmo de Troco — `decompor()`
 
Utiliza uma abordagem **gulosa** (*greedy*): percorre as denominações da maior para a menor, usando o máximo possível de cada uma sem exceder o valor do troco nem o estoque disponível.
 
O algoritmo opera sobre uma **cópia temporária** do estoque. O caixa real só é atualizado se o troco completo puder ser dado — garantindo consistência dos dados em caso de falha.
 
```
troco = valor_pago - valor_compra
 
para cada denominação (maior → menor):
    usar = min(troco / denominação, estoque_disponível)
    troco -= usar * denominação
 
se troco == 0 → sucesso, atualiza caixa
se troco != 0 → falha, caixa não é alterado
```
 
### Armazenamento de Valores Monetários
 
Todos os valores são armazenados e manipulados em **centavos** (`int`/`long long`) para evitar imprecisões de ponto flutuante. A conversão para reais ocorre apenas na exibição (`/ 100.0`).
 
---
 
##  Compatibilidade
 
| Sistema Operacional | Suporte |
|---|---|
| Linux | ✅ |
| macOS | ✅ |
| Windows | ✅ |
 
