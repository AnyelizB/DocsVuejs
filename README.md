
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
## Renderización declarativa

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

## Directiva

El atributo `v-bind` es referida a una directiva. Estas directivas son prefijos con `v-` para indicar que provienen de atributos de Vue.

Del ejemplo anterior nos referimos básicamente a: " mantener el atributo `title` de este elemento actualizado con la propiedad `message` en la instancia vue "

Si colocamos en la consola ` app2.message = 'some new message' ` observaremos la actualización del atributo `title`

### Condicionales y lazos ###

## Directiva v-if

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


## Directiva v-for

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


## Manejo de entrada de usuario 

Para la interacciòn de los usuarios con la aplicación, podemos hacer uso de ` v-on `

HTML

```
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>

```

JS

```
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})

```
Resultado

```
Hello Vue.js!

<Button "Reverse Message">

```
Resultado de nuestro evento clic:

```
!sj.euV olleH 

<Button "Reverse Message">

```

`split` : se usa para separar argumentos string

En este método actualizamos el estado de nuestra aplicación sin tocar el DOM - todas las manipulaciones de DOM son manejadas por Vue.


## Directiva v-model 

Enlace bireccional entre la entrada y el estado de la aplicaciòn.


HTML

```
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>

```

JS

```
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})

```
Resultado

```
Hello Vue!

<input " Hello Vue! ">

```

## Composición con Componentes

El sistema de componentes es otro concepto importante en Vue, porque es una abstracción que nos permite construir aplicaciones a gran escala compuestas de componentes pequeños, autónomos y a menudo reutilizables. Si pensamos en ello, casi cualquier tipo de interfaz de aplicación puede ser abstraída en un árbol de componentes. 

En Vue, un componente es esencialmente una instancia de Vue con opciones predefinidas. Registrar un componente en Vue es muy sencillo: 

JS

```

// Definir una componente llamada todo-item
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})

```

Ahora se puede componer en la plantilla con otro componente:

HTML

```
<ol>
  <!-- CREAR UNA INSTANCIA DEL COMPONENTE todo-item -->
  <todo-item></todo-item>
</ol>

```

Pasar datos de los componentes padres a los componentes hijos:

JS 

```
Vue.component('todo-item', {
  // El componente todo-item acepta un
  // "prop", la cual es un atributo personalizado.
  // El prop es llamado todo.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})

```
HTML

```
<div id="app-7">
  <ol>
    <!--
    Ahora proporcionamos cada una de `todo-item` con el objeto `todo`, para que su contenido sea dinámico.
    La componente "key"  será explicado más adelante.
    -->

    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>

```
Resultado

```

      1. Vegetables'
      2. Cheese
      3. Whatever else humans are supposed to eat


```

## La instancia Vue ## 

## Creando una instancia Vue

Cada aplicación Vue comienza creando una nueva instancia de Vue con la función Vue:

JS

```
var vm = new Vue({
  // options
})

```
El diseño de Vue esta parcialmente relacionado con MVVM (Modelo- vista- vista modelo). Suele usarse la variable `vm` (ViewModel) para referirnos a nuestra instancia Vue.

Cuando se crea la instancia de Vue, se pasa a un objeto.
Una aplicación Vue consiste en una instancia de la raíz Vue creada con `new Vue`, organizado opcionalmente en un árbol de componentes anidados y reutilizables. 


    

        Root Instance
    ├── blog
    │   ├── migrations
    │   ├── static
    │   └── templates
    └── mysite


   TodoList
   ├── TodoItem
   │  ├── DeleteTodoButton
   │  └── EditTodoButton
   └── TodoListFooter
      ├── ClearTodosButton
      └── TodoListStatistics


## Data y Metodos

Cuando se crea una instancia de Vue, este añade todas las propiedades encontradas en su objeto de `data` al sistema de reactividad de Vue. Cuando los valores de esas propiedades cambian, la vista "reaccionará", actualizándose para que coincida con los nuevos valores. 

JS

```
// Our data object
var data = { a: 1 }

// The object is added to a Vue instance
var vm = new Vue({
  data: data
})

// Getting the property on the instance
// returns the one from the original data
vm.a == data.a // => true

// Setting the property on the instance
// also affects the original data
vm.a = 2
data.a // => 2

// ... and vice-versa
data.a = 3
vm.a // => 3

```


Cuando estos datos cambien, la vista se volverá a mostrar. Debe tenerse en cuenta que las propiedades en la `data` sólo son reactivas si existían cuando se creó la instancia. Esto significa que si agregamos una nueva propiedad, como:

JS

```
vm.b = 'hi'

```


Entonces los cambios a `b` no activarán ninguna actualización de las vistas. Si no se conoce que se necesitará una propiedad más adelante, pero comienza vacía o inexistente, tendrá que establecer algún valor inicial. Por ejemplo:

JS

```
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}

```

La única excepción a esto es el uso de `Object.freeze()`, que evita que se modifiquen las propiedades existentes, lo que también significa que el sistema de reactividad no puede rastrear los cambios. 

