# Extrato de devoluções · Grupo WH / Itambé

Consulta pública com filtros múltiplos, extrato, detalhes, CSV para Excel e impressão/PDF com logos. Login por e-mail/senha apenas para importar uma planilha Excel completa. Projeto Firebase: `consulta-dev`.

## Ativação no Firebase

1. No console do projeto, criar o Cloud Firestore em modo produção.
2. Publicar o conteúdo de `firestore.rules` na aba Regras do Firestore. Alternativa com Firebase CLI autenticado: `firebase deploy --only firestore:rules --project consulta-dev`.
3. Em Authentication > Sign-in method, ativar E-mail/senha. Criar a conta do responsável em Authentication > Users. Não há cadastro público no app.
4. Copiar o UID da conta. Criar no Firestore a coleção `dev_importers`, documento com esse UID, campo `enabled` do tipo boolean com valor `true`. Apenas o administrador do console pode conceder essa permissão.
5. Adicionar o domínio final do Netlify em Authentication > Settings > Authorized domains.

As regras permitem leitura pública somente dos dados da versão ativa. Importadores autorizados podem criar versões e trocar a versão publicada. Versões antigas ficam preservadas, sem leitura pública. Não publique regras genéricas de escrita pública. A configuração web do Firebase no código é pública; nenhuma chave de serviço é usada.

## Publicação no Netlify

Importar o repositório `lucaswhrep-del/extratodev` no Netlify. Diretório base: vazio. Comando de build: vazio. Diretório de publicação: `.`. O arquivo `netlify.toml` já contém essa configuração. O código usa módulos JavaScript nativos, Firebase SDK 12.18.0 e SheetJS 0.20.3 por seus CDNs oficiais. Abrir via HTTPS ou servidor local, não via `file://`.

## Atualização da base

Abrir Atualizar base, entrar com a conta autorizada, selecionar o Excel e conferir o resumo. O arquivo deve ter uma aba `Planilha1` ou apenas uma aba. Publicar base conferida substitui integralmente o extrato. Use a base completa, não apenas as notas alteradas. O app valida colunas, datas, valores e CNPJ com 14 dígitos (não calcula dígitos verificadores). Limites: 20 MB e 100 mil registros.

Dados são carregados em partes imutáveis; só após o envio completo a versão ativa é alterada por transação. Uma publicação concorrente interrompe a troca e pede nova conferência. Erros durante o envio mantêm a versão pública anterior. Partes de tentativas incompletas ficam no Firestore e podem exigir limpeza administrativa. Atualize a página para consultar a publicação mais recente.

`Falta PG` e `Sem Data` são tratados como ausência de pagamento e data. Os códigos de referência não são exibidos. Chaves repetidas são preservadas; chaves que já vieram numéricas no Excel são sinalizadas na conferência, pois podem ter perdido precisão na origem. Valor pago aparece em módulo no extrato, com sinal original no detalhe. Diferença entre nota e pagamento não comprova saldo devedor nem quitação integral.

Nenhuma planilha ou dado de cliente está embutido neste repositório. A primeira importação é feita no app após ativar as regras e a conta. Não foram executados testes contra o Firebase real nem uma publicação no Netlify durante a preparação; esses passos dependem do acesso administrativo às contas.

## Referências

- https://firebase.google.com/docs/web/alt-setup
- https://firebase.google.com/docs/firestore/security/get-started
- https://docs.sheetjs.com/docs/getting-started/installation/standalone/
- https://docs.netlify.com/start/quickstarts/deploy-from-repository/
