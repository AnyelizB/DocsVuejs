
## VUE JS ##

Es un framework progresivo para construir inyterfaces de usuarios. Enfocado en la vista de las capas is easy to pick up integrando otras librerias o proyectos existente
Nota: es recomendable el conocimiento en HTML, CSS Y Javascript 


```
<!-- script para incluir el vue -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

```

ó 

```
<!-- production version, optimized for size and speed -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>

```

la sintaxis sería de la siguiente:

HTML 
```
<div id="app">
  {{ message }}
</div>
```
JS

```
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})

```
Agregando un text de interpolación:

HTML 
```
<div id="app-2">
  <span v-bind:title="message">
    Hover your mouse over me for a few seconds
    to see my dynamically bound title!
  </span>
</div>
```
