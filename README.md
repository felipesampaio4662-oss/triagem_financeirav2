# Clara — Triagem Financeira

Demonstração local com dados fictícios para organizar solicitações de pagamento e reembolso antes da conferência humana.

## O que faz

- padroniza os campos de entrada;
- verifica obrigatoriedade, valor em centavos, datas válidas e declaração manual de recebimento de documento;
- sinaliza possíveis duplicidades na fila e no histórico pago;
- mantém um histórico local das decisões;
- importa CSV com prévia (até 500 registros / 1 MB), sem importar aprovações;
- permite editar solicitações, exigindo nova conferência;
- registra conferente autodeclarado e justificativa;
- permite registrar uma baixa fictícia somente após a conferência, sem executar pagamento;
- exclui registros da fila com confirmação e permite restaurá-los para nova triagem;
- revalida os itens antes de exportar CSV de conferidos;
- produz backup JSON dos dados deste navegador;
- mantém descartes acessíveis para consulta; não afirma economia de tempo comprovada.

## Limites de segurança

Não aprova, não paga, não lança em ERP e não interpreta anexos. Alertas e documentos recebidos exigem registro manual antes da conferência. Para uso real, as regras precisam ser validadas pela empresa e a persistência local deve ser substituída por autenticação, banco de dados, perfis de acesso, retenção e backups apropriados.

## Verificação

Execute `node --test test.cjs`. A aplicação abre em `dist/index.html` e funciona sem rede.

## Como experimentar a versão 2

1. Dê dois cliques em ABRIR_CLARA.bat. Use somente dados fictícios.
2. Baixe **Modelo CSV**, preencha seguindo o cabeçalho e importe para ver a prévia. Valores como 120,50; datas como 2026-09-30. Use UTF-8 e ponto e vírgula.
3. Importar não confere registros: campos inválidos e duplicidades ficam bloqueados.
4. Abra um registro e use **Editar** para corrigir. Para conferir, informe um nome fictício, justificativa com pelo menos 10 caracteres e marque a declaração.
5. **Exportar conferidos** inclui todos os elegíveis, independentemente do filtro atual. Inclui conferente, justificativa e data da conferência.
6. Em um item conferido, **Registrar baixa fictícia** pede data e referência. O registro sai da fila ativa e aparece em **Pagos (simulação)**. Não executa pagamento.
7. **Excluir da fila** envia o item para **Descartados**, onde pode ser restaurado para nova triagem.
8. Faça **Backup JSON** antes de fechar. O backup é para preservação/consulta; restauração pela interface ainda não está implementada.

## Critério de duplicidade e limites

Compara favorecido normalizado, referência do documento (sem espaços, pontos e hífens) e valor em centavos, na fila ativa e no histórico fictício. Não compara CPF/CNPJ nem conteúdo de arquivos. Pode gerar falsos positivos e não encontrar variações de grafia. A data não elimina um alerta.

Não existe botão para ignorar duplicidade. Confira a origem: corrija erros genuínos ou descarte a entrada repetida com identificação e motivo. Nunca altere campos só para contornar o bloqueio.

Não há anexos armazenados, OCR, leitura de e-mail ou autenticação. O nome do conferente é autodeclarado. O histórico local não é inviolável. Trabalhe em uma única aba: não há controle de concorrência entre usuários/abas. Exportações CSV não devem ser reimportadas como modelo de entrada.

Conferências da versão anterior são invalidadas na migração, pois não continham evidência suficiente. As chaves antigas do navegador são preservadas. Um armazenamento inválido não é sobrescrito; nesse caso, a sessão usa dados demonstrativos em memória e exibe aviso.

## Validação desta entrega

20 testes automatizados das regras aprovados: centavos, datas impossíveis, obrigatoriedade, duplicidades, conferência sem evidência, alterações posteriores, baixa fictícia, CSV com aspas/quebras, limite de lote, estrutura do armazenamento e neutralização de fórmulas.
Sintaxe JavaScript verificada. Abertura da fila e do formulário de conferência inspecionada no navegador local. Isso não equivale a homologação de produção ou auditoria de segurança.
