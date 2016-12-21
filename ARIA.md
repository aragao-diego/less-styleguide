# Diretivas de acessibilidade

## Tabela de conteúdo

  1. [Acessibilidade com ngAria](#acessibilidade-com-ngaria)
    - [Incluindo ngAria](#incluindo-ngAria)
  1. [Usando ngAria](#usando-ngAria)
  1. [Diretivas suportadas](#diretivas-suportadas)
    - [ngModel](#ngModel)
    - [ngDisabled](#ngDisabled)
    - [ngRequired](#ngRequired)
    - [ngReadonly](#ngReadonly)
    - [ngChecked](#ngChecked)
    - [ngValue](#ngValue)
    - [ngShow](#ngShow)
    - [ngHide](#ngHide)
    - [ngClick](#ngClick)
    - [ngDblClick](#ngDblClick)
    - [ngMessages](#ngMessages)
  1. [Padrões Universais de Acessibilidade](padroes-universais-de-acessibilidade)

# Acessibilidade com ngAria

O objetivo do ngAria é melhorar nativamente a acessibilidade do AngularJS habilitando
atributos comuns do [ARIA](http://www.w3.org/TR/wai-aria/) que possibilitam/facilitam
as informações para tecnologias assistivas usadas por pessoas com deficiências.

## Incluindo ngAria

Usar o ngAria é simples como incluir o módulo `ngAria` na sua aplicação. ngAria
extende as diretivas padrões do AngularJS e silenciosamente injeta o suporte
à acessibilidade na sua aplicação em tempo de execução.

```js
angular.module('myApp', ['ngAria'])...
```

## Usando ngAria
A maioria das funcionalidades que o ngAria provê só é visível "atrás dos panos".
Para ver o módulo em ação, uma vez que você o injetou como dependência, você pode
testar algumas coisas:
 * Usando seu inspetor de elemento favorito, olhe os atributos adicionados pelo
 ngAria no seu código.
 * Teste usando seu teclado para garantir que o `tabindex` foi usado corretamente.
 * Use um leitor de tela como VoiceOver ou NVDA para checar o suporte ARIA.


## Diretivas suportadas
Currently, ngAria interfaces with the following directives:
Atualmente ngAria interage com as seguintes diretivas

 * [ngModel](#ngModel)
 * [ngDisabled](#ngDisabled)
 * [ngRequired](#ngRequired)
 * [ngReadonly](#ngReadonly)
 * [ngChecked](#ngChecked)
 * [ngValue](#ngValue)
 * [ngShow](#ngShow)
 * [ngHide](#ngHide)
 * [ngClick](#ngClick)
 * [ngDblClick](#ngDblClick)
 * [ngMessages](#ngMessages)

## ngModel

Muito do trabalho pesado do ngAria acontece na diretiva ngModel. Para elementos
que usam o ngModel, uma atenção especial é gasta pelo ngAria se o elemento tem
uma `role` ou os seguintes tipos: `checkbox`, `radio`, `range` or `textbox`.

Para esses elementos usando ngModel, ngAria irá, dinâmicamente, inserir e atualizar
os seguintes atributos ARIA (se eles não forem explicitamente definidos pelo desenvolvedor)

 * aria-checked
 * aria-valuemin
 * aria-valuemax
 * aria-valuenow
 * aria-invalid
 * aria-required
 * aria-readonly
 * aria-disabled


## ngValue
## ngChecked

### Exemplo
```html
<custom-checkbox ng-checked="val"></custom-checkbox>
<custom-radio-button ng-value="val"></custom-radio-button>
```

Se torna:

```html
<custom-checkbox ng-checked="val" aria-checked="true"></custom-checkbox>
<custom-radio-button ng-value="val" aria-checked="true"></custom-radio-button>
```

## ngDisabled

O atributo `disabled` só é valido para certos elementos como `button`, `input` e
`textarea`. Para propriamente desabilitar diretivas, como por exemplo `<md-checkbox>`
ou `<minha-diretiva>`, o ngDisabled também irá adicionar o `aria-disabled`.
Isso dirá para as tecnologias assistivas quando um input não nativo está desabilitado,
ajudando os controles customizados a serem mais acessivéis.

### Exemplo

```html
<md-checkbox ng-disabled="disabled"></md-checkbox>
```

Se torna:

```html
<md-checkbox disabled aria-disabled="true"></md-checkbox>
```

## ngRequired

O atributo booleano `required` é válido para controladores de formulários nativos
como `input` e `textarea`. Para indicar corretamente que um elemento/diretiva não-nativo
é obrigatório, o ngRequired também irá adicionar o `aria-required`. Isso dirá para
as tecnologias acessivas quando um elemento customizado é obrigatório.


### Exemplo

```html
<md-checkbox ng-required="val"></md-checkbox>
```

Se torna:

```html
<md-checkbox ng-required="val" aria-required="true"></md-checkbox>
```

## ngReadonly

O atributo booleano `readonly` é válido para controladores de formulários nativos
como `input` e `textarea`. Para indicar corretamente que um elemento/diretiva não-nativo
é somente para leitura, o ngReadonly também irá adicionar o `aria-readonly`. Isso dirá para
as tecnologias acessivas quando um elemento customizado é somente para leitura.

### Exemplo

```html
<md-checkbox ng-readonly="val"></md-checkbox>
```

Se torna:

```html
<md-checkbox ng-readonly="val" aria-readonly="true"></md-checkbox>
```

## ngShow

A diretiva ngShow exibe ou oculta o elemento HTML baseado na expressão atribuída
ao atributo `ngShow`. O elemento é exibido ou ocultado pela adição da classe CSS
`.ng-hide`.

Na sua configuração padrão, o ngAria para o `ngShow` é atualmente redundante. Ele
alterna `aria-hidden` na diretiva quando ela é exibida ou ocultada. Entretanto, a
classe padrão CSS `display: none !important` já oculta os elementos filhos do
leitor de tela. Isso se torna mais utíl quando o CSS padrão é sobre-escrito por
propriedades que não afetam tecnologias assistivas, tais como `opacity` ou `transform`.
Alternando dinamicamente `aria-hidden`com ngAria, podemos garantir que o conteúdo
visualmente ocultado não será lido pelo leitor de tela.

### Exemplo
```css
.ng-hide {
  display: block;
  opacity: 0;
}
```
```html
<div ng-show="false" class="ng-hide" aria-hidden="true"></div>
```

Se torna:

```html
<div ng-show="true" aria-hidden="false"></div>
```

## ngHide

A diretiva ngHide exibe ou oculta o elemento HTML baseado na expressão atribuída
ao atributo `ngHide`. O elemento é exibido ou ocultado pela adição da classe CSS
`.ng-hide`.


## ngClick e ngDblclick
Se um `ng-click` ou `ng-dblclick` é encontrado, ngAria erá adicionar `tabindex=0`
em todos os elementos que são do tipo:

 * Button
 * Anchor
 * Input
 * Textarea
 * Select
 * Details/Summary

Para resolver problemas de acessiblidade com `ng-click` em elementos `div`,
ngAria irá vincular dinamicamente um evento `keypress` nos elementos que não
estão na lista acima.

Também adicionará ao `button` a `role` para se comunicar com usuários que usam
tecnologias assistivas.

Para o `ng-dblclick` é necessário a inclusão manual da `role` e do evento
`key-press` (para elementos que não estão na lista acima).


### Exemplo
```html
<div ng-click="toggleMenu()"></div>
```

Se torna:

```html
<div ng-click="toggleMenu()" tabindex="0"></div>
```

## ngMessages

O módulo ngMessas torna fácil a exibição de mensagens de validação de formulário
ou outras mensagens com prioridades e animações. Para expor essas mensagens visuais
para os leitores de tela, ngAria injeta `aria-live="assertive"`, fazendo com que
os leitores as leiam assim que a mensagem é exibida, independente do foco atual
do usuário.

### Exemplo

```html
<div ng-messages="myForm.myName.$error">
  <div ng-message="required">You did not enter a field</div>
  <div ng-message="maxlength">Your field is too long</div>
</div>
```

Se torna:

```html
<div ng-messages="myForm.myName.$error" aria-live="assertive">
  <div ng-message="required">You did not enter a field</div>
  <div ng-message="maxlength">Your field is too long</div>
</div>
```

## Padrões Universais de Acessibilidade

As boas práticas de acessibilidade para aplicações web em geral também se
aplicam ao AngularJS.

 * **Alternativas textuais**: Adicionar alternativas textuais para tornar as
 informações visuais acessíveis [guia w3c](http://www.w3.org/TR/html-alt-techniques/).
 A técnica apropriada varia de acordo com o elemento, podendo ser `span` fora da tela,
 `aria-label`, elementos `label`, atributo `alt`, atributo `figcaption`, etc..
 * **HTML Semântico**: Para elementos criados, usar quando possível elementos e
 eventos nativos. Alternativamente usar ARIA para dar o sentido semântico do
 elemento [uso do ARIA](http://www.w3.org/TR/aria-in-html/#notes-on-aria-use-in-html).
 * **Gerenciamento do foco**: Guiar o usuário através da aplicação. O foco do
 usuário *nunca* deve ser mudado, já que isso causa comportamentos inesperados e
 confusão.
 * **Anúncio de mudnaças**: Quando eventos acontecerem longe do foco do usuário,
 anuncia-los com o [ARIA Live Regions](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions).
 * **Contraste de cor e escala**: Certifique-se que o conteúdo é legível e os
 controles interativos são acessíveis em todos os tamanhos de tela. Considere um
 esquema de cores para pessoas com dificuldades visuais.
