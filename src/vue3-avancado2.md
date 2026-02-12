# Apostila Avançada Vue 3 + Quasar
## Arquitetura, Estado, Comunicação e Performance

> Esta apostila **não é introdutória**. Ela existe para responder perguntas que surgem em **projetos reais e grandes**, quando o código começa a crescer e decisões erradas custam caro.

---

# 1. Composables

## 1.1 O que é um composable

Um **composable** é uma função que:
- usa a API de composição (`ref`, `computed`, `watch`…)
- encapsula **estado + regras de negócio**
- pode ser reutilizada em múltiplos componentes

Ele **não é um helper genérico**, nem um serviço qualquer.

```ts
export function useCounter() {
  const count = ref(0)

  function increment() {
    count.value++
  }

  return { count, increment }
}
```

---

## 1.2 Quando criar um composable

Crie um composable quando:

- 🔁 A mesma lógica aparece em **mais de um componente**
- 🧠 Existe **estado reativo** envolvido
- 📏 O componente está ficando grande demais
- 🔗 A lógica **não pertence visualmente** ao componente

Exemplos reais:
- controle de filtros
- paginação
- formatação de dados complexos
- regras de permissão

---

## 1.3 Quando NÃO criar um composable

❌ NÃO crie um composable quando:

- a lógica é usada **uma única vez**
- é apenas uma função pura (use `utils`)
- você está tentando “organizar demais” cedo

Erro clássico:
> transformar qualquer função em composable sem estado

---

## 1.4 Erros comuns com composables

### ❌ Criar composable para tudo
Isso gera:
- indireção excessiva
- leitura difícil
- debugging complexo

### ❌ Compartilhar estado sem perceber

```ts
const shared = ref(0)
export function useBadComposable() {
  return { shared }
}
```

Isso cria **estado global acidental**.

---

## 1.5 Exemplo real bem feito

```ts
export function usePagination() {
  const page = ref(1)
  const perPage = ref(10)

  const offset = computed(() => (page.value - 1) * perPage.value)

  return { page, perPage, offset }
}
```

---

# 2. Arquitetura de Estado com Pinia

## 2.1 Estado local vs estado global

### Estado local
- pertence a um componente
- não precisa ser compartilhado
- vive e morre com o componente

```ts
const isOpen = ref(false)
```

### Estado global
- compartilhado
- precisa sobreviver a navegação
- representa **regra do sistema**

---

## 2.2 Quando usar Pinia

Use Pinia quando:
- vários componentes dependem do mesmo estado
- existe sincronização entre telas
- regras precisam ser centralizadas

---

## 2.3 Anti-patterns comuns

### ❌ Store gigante

Uma store com:
- dados
- UI
- regras
- side-effects

Tudo misturado.

### ❌ Store como backend

A store **não é API**, nem cache infinito.

---

## 2.4 Stores grandes bem organizadas

```ts
export const useUserStore = defineStore('user', {
  state: () => ({
    user: null,
    loading: false
  }),
  getters: {
    isLogged: state => !!state.user
  },
  actions: {
    async fetchUser() {}
  }
})
```

Separe:
- estado
- leitura
- ações

---

# 3. Comunicação entre Componentes

## 3.1 Props drilling

Problema:
- props passando por muitos níveis

Soluções:
- slots
- composables
- provide/inject

---

## 3.2 Emits

```vue
<Child @save="handleSave" />
```

Emits:
- comunicação **filho → pai**
- explícita
- previsível

---

## 3.3 Slots avançados

```vue
<Modal>
  <template #header>...</template>
  <template #default="{ close }">...</template>
</Modal>
```

Slots:
- resolvem composição
- evitam props infinitas

---

## 3.4 Provide / Inject

```ts
provide('theme', theme)
```

Use quando:
- muitos níveis
- contexto compartilhado

Evite usar como estado global.

---

# 4. Performance e Reatividade

## 4.1 Quando o Vue recalcula

Vue recalcula quando:
- uma dependência reativa muda
- o template depende dela

---

## 4.2 Armadilhas com computed

❌ Computed pesado
❌ Computed com efeitos colaterais

Computed deve ser:
- determinístico
- rápido

---

## 4.3 Watch: quando usar

Use `watch` quando:
- precisa reagir a mudanças
- precisa chamar API
- precisa executar side-effect

Não use para derivar valores.

---

## 4.4 Renders desnecessários

Causas comuns:
- props novas a cada render
- objetos criados inline
- watchers demais

---

# 5. Padrões de Projeto Grande

## 5.1 Organização de pastas

```text
components/
composables/
stores/
views/
services/
utils/
```

---

## 5.2 Responsabilidades claras

- Componentes: UI
- Composables: regra
- Stores: estado global
- Services: API

Nunca misture tudo.

---

## 5.3 Onde colocar cada regra

| Tipo de regra | Lugar correto |
|--------------|---------------|
| Formatação | utils |
| Regra reutilizável | composable |
| Estado global | store |
| UI | component |

---

## 5.4 Regra de ouro

> Se você não sabe onde colocar algo, o problema **não é a pasta**, é a responsabilidade.

---

## Encerramento

Essa apostila foi feita para:
- evitar retrabalho
- evitar refatorações dolorosas
- te dar segurança arquitetural

Ela não ensina truques — ensina **decisão técnica**.

