# Exemplos e Referências

Esta pasta contém exemplos de implementação para cada etapa do desafio.

## Passo a Passo: Do Setup ao Pitch

Aqui está o roteiro resumido para implementar e avaliar a solução completa:

1. **Pré-requisitos e Setup**: Instale o Python (>= 3.8) e o [Ollama](https://ollama.com). Baixe o modelo `gpt-oss` executando `ollama pull gpt-oss` e inicie o serviço local com `ollama serve`. Instale as dependências via `pip install streamlit pandas requests`.
2. **Documentação**: Definição clara do caso de uso (educador financeiro), público-alvo, persona (**Edu**), tom de voz didático e as regras de segurança/limitações do agente.
3. **Base de Conhecimento**: Carregamento estruturado de arquivos JSON e CSV contendo o perfil do investidor, transações recentes, histórico de atendimentos e os produtos financeiros ofertados.
4. **Prompts**: Construção do prompt do sistema com regras anti-alucinação rígidas (ex: não recomendar ativos diretamente) e exemplos *few-shot* para direcionamento de tom.
5. **Aplicação**: Implementação do chatbot em Streamlit integrando o contexto formatado e realizando chamadas HTTP para o endpoint local do Ollama.
6. **Métricas**: Avaliação sistemática do agente em termos de assertividade, segurança e coerência usando testes estruturados (como validação de gastos e edge cases).
7. **Pitch**: Construção de um roteiro estratégico de 3 minutos demonstrando a dor, a solução local/privada do Edu, casos práticos e o diferencial de impacto social do projeto.

---

## Guias e Arquivos de Referência

Abaixo estão os links para a documentação técnica detalhada de cada etapa desenvolvida:

| Etapa | Descrição | Link |
|-------|-----------|------|
| Documentação | Como definir o caso de uso e arquitetura | [docs/01-documentacao-agente.md](../docs/01-documentacao-agente.md) |
| Base de Conhecimento | Como usar os dados mockados | [docs/02-base-conhecimento.md](../docs/02-base-conhecimento.md) |
| Prompts | Como criar prompts eficazes | [docs/03-prompts.md](../docs/03-prompts.md) |
| Aplicação | Como configurar e rodar o chatbot local | [src/README.md](../src/README.md) |
| Métricas | Como avaliar seu agente e registrar resultados | [docs/04-metricas.md](../docs/04-metricas.md) |
| Pitch | Roteiro completo da apresentação e resultados | [docs/05-pitch.md](../docs/05-pitch.md) |

---

## Exemplo de Implementação Simples

Confira na pasta `src/` o código do protótipo funcional em [app.py](../src/app.py) usando Streamlit.
