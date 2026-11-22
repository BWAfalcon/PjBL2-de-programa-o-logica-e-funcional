# PjBL2 — Tabela de Classificação de Campeonato em Haskell

Implementação em Haskell puro (utilizando apenas módulos da biblioteca padrão `base`) para cálculo e validação de tabelas de classificação de campeonatos de futebol, organizada em três camadas funcionais.

## Ambiente Utilizado

- **Ambiente principal**: GHC local (instalado via GHCup, compatível com GHC 9.x).
- **Compatibilidade**: O código não utiliza `getLine` nem dependências externas, podendo também ser executado no playground online ([play.haskell.org](https://play.haskell.org)).

## Estrutura do Repositório

```text
projeto/
├── src/
│   ├── Tipos.hs          # Camada 1: tipos algébricos (Resultado, Partida, Linha) e consultas básicas
│   ├── Classificacao.hs  # Camada 2: pontuação, dobras, estatísticas e ordenação multicritério
│   ├── Leitura.hs        # Camada 3: parsing textual, validação (Maybe/Either) e formatação
│   ├── Dados.hs          # Dados de teste (campeonato válido, campeonato com erros e vazio)
│   └── Main.hs           # Demonstração sequencial das três camadas
├── tests/
│   └── Testes.hs         # Suíte de testes automatizados cobrindo as três camadas e casos de borda
├── docs/
│   └── decisoes.md       # Justificativa das decisões de modelagem e limitações conhecidas
└── README.md             # Instruções de compilação, execução e testes
```

## Como Compilar e Executar

A partir da pasta `projeto/`:

### 1. Programa Principal (`Main.hs`)

Compile com `-Wall` para verificar ausência de avisos do compilador e execute:

```bash
ghc -Wall -O2 -isrc src/Main.hs -o campeonato
./campeonato
```

*(No Windows PowerShell ou CMD, execute `campeonato.exe`.)*

### 2. Suíte de Testes (`tests/Testes.hs`)

```bash
ghc -Wall -isrc tests/Testes.hs -o rodar_testes
./rodar_testes
```

*(Ou via interpretador: `runghc -isrc tests/Testes.hs`.)*

## O que o Programa Demonstra

1. **Camada 1**: Extração dos 6 times participantes em ordem alfabética a partir das partidas.
2. **Camada 2 e 3 (Classificação Final)**: Leitura de um campeonato completo de 6 times e 5 rodadas (15 partidas), exibindo o desempate pelo saldo de gols entre **Athletico** (10 pts, 3V, +6 SG) e **Coritiba** (10 pts, 3V, +3 SG).
3. **Classificação Intermediária (`ateRodada 3`)**: Evolução do campeonato após a 3ª rodada, com **Coritiba** na liderança (7 pts).
4. **Estatísticas Gerais**: Cálculo total de `maiorGoleada` (`Athletico 4 x 0 Maringa`), `mediaGols` (`2.733...` gols/partida) e `aproveitamento` do líder (`66.67%`).
5. **Robustez e Tratamento de Erros**:
   - Tratamento seguro de campeonato vazio (`[]`) sem exceções.
   - Validação via `lerCampeonato` (interrompe no primeiro `Left Erro` com número da linha) e via `todosOsErros` (lista todos os erros sintáticos e semânticos encontrados no texto).
