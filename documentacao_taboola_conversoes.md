# Documentação de Rastreamento Taboola Pixel & Conversões Manuais

## 1. Identificação da Conta
* **Taboola Account ID:** `2101593`

---

## 2. Eventos Configurados no Painel e Implementados no Código

### A. Visualização de Conteúdo / Artigo
* **Nome de Conversão no Taboola:** `entrou na pagina`
* **Tipo de Conversão:** `EVENT`
* **Nome do Evento no Código:** `view_content`
* **Disparo:** Automático no carregamento das páginas junto ao `page_view`.
```javascript
window._tfa = window._tfa || [];
window._tfa.push({
  notify: 'event',
  name: 'view_content',
  id: 2101593
});
```

### B. Clique no WhatsApp / Contato
* **Nome de Conversão no Taboola:** `botao_whatsapp`
* **Categoria:** `Contato`
* **Tipo de Conversão:** `EVENT`
* **Nome do Evento no Código:** `contact`
* **Disparo:** Acionado dinamicamente ao clicar em qualquer botão ou link direcionando para o WhatsApp (`wa.me`).
```javascript
window._tfa = window._tfa || [];
window._tfa.push({
  notify: 'event',
  name: 'contact',
  id: 2101593
});
```

---

## 3. Captura e Persistência do Taboola Click ID (`tblci`)
Para rastreamento resiliente contra ad-blockers e perda de cookies:
* Quando o anúncio traz o parâmetro `?tblci=...`, o script salva em cookie primário (`tb_click_id`, validade de 30 dias) e no `localStorage`.

---

## 4. Como Validar no Navegador
1. Instale a extensão **Taboola Pixel Helper** na Chrome Web Store.
2. Acesse a landing page.
3. Verifique se o ícone da extensão indica:
   - Account ID: `2101593`
   - `page_view`: Verde / Ativo
   - `view_content`: Verde / Ativo
4. Clique em qualquer botão de WhatsApp e observe o disparo imediato do evento:
   - `contact`: Verde / Ativo

---

## 5. Fila de Rotação Igualitária de WhatsApp (Round-Robin)
O tráfego dos cliques em WhatsApp é distribuído de forma 100% alternada e sequencial entre os 3 números:

* **Atendente 1:** `+55 21 96975-5691` (`5521969755691`)
* **Atendente 2:** `+55 21 98080-5566` (`5521980805566`)
* **Atendente 3:** `+55 21 98080-6961` (`5521980806961`)

**Ciclo de rotação:**
* Clique 1 ➔ Atendente 1
* Clique 2 ➔ Atendente 2
* Clique 3 ➔ Atendente 3
* Clique 4 ➔ Atendente 1 (reinicia o ciclo de forma ininterrupta)

O índice da fila é armazenado no `localStorage` do navegador (`tb_wa_queue_idx`), preservando as mensagens customizadas de cada botão e disparando a conversão do Taboola Pixel em cada chamada.

