# Experiments - Sub-Agents

# Benchmark de Produtividade: Linear vs. Subagentes

## *Data:* 2026-04-28
*Modelo:* Claude Sonnet 4.6
*Tarefa:* Criar uma API REST Flask para um Sistema de Gerenciamento de Biblioteca (⁠ [models.py](http://models.py/) ⁠, ⁠ [app.py](http://app.py/) ⁠, ⁠ test_api.py ⁠)

## Metodologia

## O experimento comparou dois modos de geração de código para a mesma tarefa:
•⁠ ⁠*Round 1 — Linear:* todos os arquivos escritos sequencialmente pelo agente principal em uma única passagem de raciocínio, sem delegação.
•⁠ ⁠*Round 2 — Subagentes:* 3 subagentes lançados em paralelo, cada um responsável por um arquivo, com contexto completo fornecido pelo agente orquestrador.
Ambos os rounds foram executados no mesmo ambiente (Arch Linux, Python 3.14.3, Flask + pytest via virtualenv) e validados com ⁠ python -m pytest test_api.py -v ⁠.

## Métricas Coletadas

| Métrica | Round 1 — Linear | Round 2 — Subagentes |
| --- | --- | --- |
| Tempo de geração de código | ~15 s | ~18 s (tempo de parede, paralelo) |
| Tokens totais (input + output) | ~11.200 (estimado¹) | *29.493* (medido via runtime) |
| Chamadas de ferramentas — código | 3 ⁠ Write ⁠ | 4 ⁠ Write ⁠ (distribuídas) |
| Chamadas de ferramentas — total | 9 (3 Write + 6 Bash²) | 12 (3 Agent + 4 Write + 2 Read + 3 Bash) |
| Execução paralela | Não | Sim (3 agentes simultâneos) |
| Testes passando | 3 / 3 ✓ | 3 / 3 ✓ |
| Erros / retrabalho | 0 | 0 |

## ⁠¹ Estimado: o contexto do agente principal inclui system prompt (~6 k tokens) + histórico de conversação + conteúdo dos 3 arquivos. Tokens de subagentes não são expostos pelo runtime no Round 1.
⁠² As 6 chamadas Bash incluem tentativas falhadas de instalação de dependências (ambiente Arch Linux gerenciado exige ⁠ python -m venv ⁠).

## Detalhamento Round 2 por Subagente

| Subagente | Arquivo | Tokens Totais | Ferramentas | Duração |
| --- | --- | --- | --- | --- |
| models-agent | ⁠ [models.py](http://models.py/) ⁠ | 9.246 | 1 Write | 7.410 ms |
| api-agent | ⁠ [app.py](http://app.py/) ⁠ | 9.711 | 1 Write | 8.546 ms |
| test-agent | ⁠ test_api.py ⁠ | 10.536 | 2 Write/Edit | 18.009 ms |
| *Total* | — | *29.493* | *4* | *18.009 ms* (gargalo: test-agent) |

## O tempo de parede do Round 2 é ditado pelo subagente mais lento (test-agent, 18 s). Os outros dois terminaram em ~7–8 s mas ficaram ociosos aguardando a conclusão do gargalo.

## Análise Qualitativa do Código

### ⁠ [models.py](http://models.py/) ⁠ — idêntico nos dois rounds

Ambos geraram a mesma dataclass com ⁠ asdict ⁠:
 
⁠ python
from dataclasses import dataclass, asdict
 
@dataclass
class Book:
    id: int
    titulo: str
    autor: str
    ano: int
 
    def to_dict(self) -> dict:
        return asdict(self)
 ⁠

### ⁠ [app.py](http://app.py/) ⁠ — diferença de estilo nos decoradores

⁠ python

# Round 1 — decoradores modernos Flask 2.0+ (mais concisos)

@app.get("/books")
@app.post("/books")
@app.delete("/books/int:book_id")

# Round 2 — estilo tradicional com @app.route (mais explícito)

@app.route("/books", methods=["GET"])
@app.route("/books", methods=["POST"])
@app.route("/books/int:book_id", methods=["DELETE"])
 ⁠
 
A lógica interna (helper ⁠ _find_book ⁠, validação de campos, retornos HTTP) foi equivalente nos dois rounds.

### ⁠ test_api.py ⁠ — Round 2 produziu testes mais robustos

# Round 1 — comparação de objeto inteiro (frágil à ordem de campos)

assert r_get.get_json() == created

# Round 2 — assertions granulares por campo (mais resilientes e legíveis)

## assert data["titulo"] == "Dom Casmurro"
assert data["autor"] == "Machado de Assis"
assert data["ano"] == 1899 ⁠
O Round 2 também usou nomes de variáveis mais descritivos (⁠ post_response ⁠, ⁠ get_response ⁠, ⁠ delete_response ⁠ vs. ⁠ r_post ⁠, ⁠ r_get ⁠, ⁠ r_del ⁠) e a fixture de reset foi mais enxuta (sem ⁠ yield ⁠ desnecessário).

## Resultado dos Testes

Ambos os rounds produziram código funcional sem erros:

============================= test session starts ==============================
platform linux -- Python 3.14.3, pytest-9.0.3
collected 3 items
 
test_api.py::test_get_all_books_empty        PASSED  [ 33%]
test_api.py::test_create_and_get_book        PASSED  [ 66%]
test_api.py::test_delete_book                PASSED  [100%]
 
============================== 3 passed in 0.06s ===============================

## Tabela Comparativa Final

| Dimensão | Vencedor | Justificativa |
| --- | --- | --- |
| *Eficiência de tokens* | Round 1 | ~2,6× menos tokens (sem overhead de contexto por subagente) |
| *Velocidade (wall clock)* | Empate | R1 ~15 s geração; R2 ~18 s (gargalo no test-agent) |
| *Qualidade dos testes* | Round 2 | Assertions granulares, nomes descritivos, fixture mais limpa |
| *Escalabilidade* | Round 2 | Paralelismo real beneficia tarefas maiores e independentes |
| *Simplicidade operacional* | Round 1 | Sem overhead de orquestração ou risco de incoerência entre agentes |

## Conclusão

O overhead de tokens no Round 2 (~18.300 tokens a mais) é o custo fixo de inicializar 3 contextos isolados com briefing completo. Para uma tarefa desta escala (3 arquivos pequenos, <200 linhas no total), esse custo não se paga em velocidade — o test-agent foi o gargalo e tornou o Round 2 ligeiramente mais lento que a abordagem linear.
 
O padrão de subagentes passa a se justificar quando:
 
1.⁠ ⁠Os arquivos ou módulos são substancialmente maiores (tempo de CPU por subagente > 30 s).
2.⁠ ⁠As dependências entre subtarefas são mínimas (cada agente pode trabalhar de forma verdadeiramente independente).
3.⁠ ⁠A qualidade do output individual importa mais que o custo total de tokens (subagentes tendem a produzir código mais cuidadoso por focarem em um escopo menor).
 
Para tarefas pequenas e coesas como esta, a abordagem linear é mais eficiente em tokens e igualmente rápida — com a vantagem adicional de manter coerência de estilo naturalmente, sem necessidade de briefing entre agentes.
