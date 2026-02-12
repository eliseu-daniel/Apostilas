# Apostila Completa de Vue 3 + Quasar

> Material **didático, progressivo e profundo**, pensado para formar **base sólida** em Vue 3 + Quasar, indo do zero até padrões profissionais usados em projetos grandes.

---

## 1. Fundamentos que você PRECISA entender

### 1.1 O que é Vue (de verdade)
Vue **não é só um framework de template**. Ele é um sistema reativo.

O ponto central do Vue é:
👉 **estado → reatividade → renderização automática**

Você NÃO manda atualizar a tela.  
Você **muda o estado**, e o Vue decide o que precisa ser atualizado.

---

### 1.2 O que é um Componente

Um componente Vue é uma **função de UI**:
- recebe dados (props)
- possui estado interno
- reage a mudanças
- emite eventos

Estrutura mental correta:
```
Componente = Estado + Regras + Template
```

---

### 1.3 Estrutura de um componente Vue 3

```vue
<template>
  <!-- HTML declarativo -->
</template>

<script setup lang="ts">
// lógica e estado
</script>

<style scoped>
/* estilos isolados */
</style>
```

Por que isso importa?
- separação de responsabilidades
- previsibilidade
- facilidade de manutenção

---

## 2. Reatividade (o coração do Vue)

### 2.1 ref

```ts
const count = ref(0)
```

- usado para valores primitivos
- acessado com `.value` no script
- no template, o Vue faz unwrap automático

❌ erro comum:
```ts
count++ // errado
```

---

### 2.2 reactive

```ts
const user = reactive({ name: 'Ana', age: 20 })
```

- usado para objetos
- não precisa `.value`
- perde reatividade se desestruturar errado

---

### 2.3 Computed (conceito crítico)

Computed NÃO é conveniência.  
Computed é **regra de negócio derivada**.

```ts
const fullName = computed(() => {
  return `${first.value} ${last.value}`
})
```

Use computed quando:
- depende de outro estado
- precisa ser cacheado
- NÃO é ação

Nunca use computed para:
- requisição HTTP
- efeitos colaterais

---

### 2.4 Watch

Watch observa mudanças e executa efeitos.

```ts
watch(tab, (newVal, oldVal) => {
  console.log(newVal)
})
```

Use watch quando:
- precisa reagir a mudanças
- sincronizar com API
- executar lógica imperativa

---

## 3. Template Vue (além do básico)

### 3.1 Template é lógica declarativa

```vue
<div v-if="isLogged">Bem-vindo</div>
```

Você NÃO controla fluxo manualmente.
Você descreve estados possíveis.

---

### 3.2 v-if vs v-show (decisão importante)

| v-if | v-show |
|----|----|
| Remove do DOM | Mantém no DOM |
| Mais caro | Mais barato |

Regra prática:
- alternância rara → `v-if`
- alternância frequente → `v-show`

---

### 3.3 Uso correto do <template>

`<template>` é **container lógico**, não HTML.

```vue
<template v-if="isAdmin">
  <h1>Admin</h1>
  <p>Acesso total</p>
</template>
```

Use quando:
- aplicar diretiva em múltiplos nós
- evitar div extra
- trabalhar com slots

---

## 4. Props, Emits e Contratos

### 4.1 Props são contratos

```ts
defineProps<{ title: string; disabled?: boolean }>()
```

Props devem:
- ser previsíveis
- ser validadas
- NÃO ser mutadas

---

### 4.2 Emits

```ts
defineEmits<{ (e: 'save', id: number): void }>()
```

Emit NÃO é callback.
Emit é **evento sem acoplamento**.

---

## 5. Código Defensivo (nível profissional)

### 5.1 Fallback

Fallback = plano B.

```ts
const name = user.name ?? 'Visitante'
```

---

### 5.2 ?? vs || (obrigatório saber)

- `??` → null / undefined
- `||` → valores falsy

Erro clássico:
```ts
page || 1 // quebra se page = 0
```

---

## 6. Quasar – Conceitos Estruturais

### 6.1 Quasar não é só componente

Quasar fornece:
- Layout
- Breakpoints
- UX mobile
- Infraestrutura

---

### 6.2 QLayout e QPage

```vue
<q-layout>
  <q-header />
  <q-page-container>
    <q-page />
  </q-page-container>
</q-layout>
```

Tudo no Quasar gira em torno disso.

---

## 7. Navegação e Componentes Dinâmicos

### 7.1 router-link dinâmico

```vue
<component :is="linkComponent" />
```

Usado para:
- link condicional
- reduzir duplicação

---

## 8. Tabs, Panels e Estado

Tabs são **estado**, não UI.

```ts
const tab = ref('dados')
```

Nunca trate tab como string solta.

---

## 9. Scroll, Layout e UX

Scroll só existe com altura definida.

```vue
<q-scroll-area class="fit" />
```

---

## 10. QTable (uso real)

Tabela NÃO é mobile-first.

Regra:
- Desktop → tabela
- Mobile → cards

```vue
<q-table grid />
```

---

## 11. Organização de Projeto

Sugestão:
```
components/
composables/
stores/
utils/
```

Nunca misture:
- regra de negócio
- UI

---

## 12. Mentalidade Correta

Vue bem feito é:
- previsível
- reativo
- defensivo
- simples

Se está complexo demais, está errado.

---

## 13. Próxima Evolução

Após dominar isso:
- Composables
- Pinia
- Permissões
- Performance
- Testes

---

📘 Esta apostila é **base estrutural**, não receita pronta.

