
## VUE JS ##

Es un framework progresivo para construir interfaces de usuarios. Vue está diseñado desde cero para ser adoptado de forma incremental, es fácil de recoger e integrar con otras librerias o proyetos existentes.
![Nota]: es recomendable el conocimiento en HTML, CSS Y Javascript 


```
<!-- script para incluir el vue en el html-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

```

ó 

```
<!-- production version, optimized for size and speed -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>

```
#Renderización declarativa

La sintaxis sería lo siguiente:

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

```
Hello Vue!

```
Además de la interpolación de texto, también podemos enlazar atributos de elementos como éste: :

HTML 
```
<div id="app-2">
  <span v-bind:title="message">
    Hover your mouse over me for a few seconds
    to see my dynamically bound title!
  </span>
</div>
```

JS
```
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'You loaded this page on ' + new Date().toLocaleString()
  }
})

``` 
```
Hover your mouse over me for a few seconds
    to see my dynamically bound title!
```

#Directiva

El atributo `v-bind` es referida a una directiva. Estas directivas son prefijos con `v-` para indicar que provienen de atributos de Vue.

Del ejemplo anterior nos referimos básicamente a: " mantener el atributo `title` de este elemento actualizado con la propiedad `message` en la instancia vue "

Si colocamos en la consola ` app2.message = 'some new message' ` observaremos la actualización del atributo `title`

##Condicionales y lazos##

#Directiva v-if
HTML 

```
<div id="app-3">
  <span v-if="seen">Now you see me</span>
</div>

```
JS 

```
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})

```

```

Now you see me 

```
Si en su lugar se coloca `app3.seen = false` en la terminal el mensaje desaparece.

Demostrando que podemos enlazar datos no sólo con texto y atributos sino tambien con la estructura del DOM. Además, Vue también proporciona un potente sistema de efectos de transición que se puede aplicar automáticamente cuando los elementos son insertados/actualizados/quitados por Vue. 


#Directiva v-for

Este tipo de directivas puede ser utilizada para mostrar una lista de items proveniente de un arreglo:

HTML

```
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>

```

JS 

```
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})

```
Resultado:

```
1. Learn JavaScript
2. Learn Vue
3. Build something awesome

```

Al colocar en la terminal `app4.todos.push({ text: 'New item' }) ` se debería observar un nuevo elemento añadido a la lista.


