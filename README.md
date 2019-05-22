
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
        TodoList
    ├── TodoItem
    │   ├── DeleteTodoButton
    │   └── EditTodoButton
    │   
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


JS

```
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})

```

HTML

```
<div id="app">
  <p>{{ foo }}</p>
  <!-- this will no longer update `foo`! -->
  <button v-on:click="foo = 'baz'">Change it</button>
</div>

```
Además de las propiedades de los datos, las instancias Vue exponen una serie de propiedades y métodos útiles de las instancias. Estos se prefijan con `$` para diferenciarlos de las propiedades definidas por el usuario. Por ejemplo:


JS

```

var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch es un metodo de la instancia
vm.$watch('a', function (newValue, oldValue) {
  // Este callback será llamado cuando cuando `vm.a` cambie
})

```

## Ciclos de vida de la instancia

Cada instancia de Vue pasa por una serie de pasos de inicialización cuando se crea - por ejemplo, necesita configurar la observación de datos, compilar la plantilla, montar la instancia en el DOM y actualizar el DOM cuando los datos cambian. A lo largo del camino, también ejecuta funciones llamadas `hook` de ciclo de vida, dando a los usuarios la oportunidad de añadir su propio código en etapas específicas. 

`created` hook puede ser creada para correr código despues de una instancia creada: 

JS

```
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` points to the vm instance
    console.log('a is: ' + this.a)
  }
})


```

Resultado:

```
"a is: 1"

```

Existen tambien otros hooks las cuales pueden ser llamadas en estados de ciclos de vida de la instancia, como lo son `mounted` , `update`  y `destroyed` .

## Sintaxis de la plantilla ##


Vue. js utiliza una sintaxis de plantilla basada en HTML que le permite vincular declarativamente el DOM renderizado a los datos de la instancia de Vue subyacente. Todas las plantillas Vue. js son HTML válidas que pueden ser analizadas por navegadores y analizadores HTML compatibles con las especificaciones. 

## Interpolación 


### Text

El formulario más básico de enlace de datos es la interpolación utilizando la sintaxis "Mustache"


HTML 

```
<span>Message: {{ msg }}</span>

```

La etiqueta del mustache (doble llave) será reemplazada con los valores de la propiedad `msg` en el objeto de la data correspondiente. Este tambien actualizará cada vez que los objetos de la data de la propiedad `msg` cambie.

 ## Directiva v-once

Tambien se puede ejecutar una interpolación un vez , el  mismo no actualiza los cambios de la data usando la directiva `v-once`, pero esto tambien puede afectar cualquier enlace en el mismo nodo

HTML

```
<span v-once>This will never change: {{ msg }}</span>

```
### Raw HTML

Las dobles llaves interpreta la data como texto plano, no HTML. Para la salida en html se necesitará utilizar la directiva `v-html`:

HTML

```
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>

```

Resultado

```
Using mustaches: <span style ="color:red">This should be red.</span>
Using v-html directive: This should be red.

```

### Attibutes

Las dobles llaves o mustaches no pueden ser usadas dentro de atributos HTML. En su lugar usar la directiva: ` v-bind`

HTML 

```
<div v-bind:id="dynamicId"></div>

```

En el caso de atributos booleanos, donde su exitencia implica `true` , `v-bind` :

HTML

```
<button v-bind:disabled="isButtonDisabled">Button</button>

```
Si `isButtonDisabled` tiene el valor de `null` , `undefined` , ó `false`, el atributo `disabled` no incluirá en la vista el elemento `<button>`

### Uso de expresiones javascript


HTML

```

{{ number + 1 }}
{{ ok ? 'YES' : 'NO'}}
{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>

```
Estas expresiones serán evaluadas como javascript de la propia instancia Vue. Una restricción es que cada enlace puede solamente contar con un expresión simple. Entonces el siguiente código no trabajará: 

HTML 

```
<!-- this is a statement, not an expression: -->

{{ var a = 1 }}

<!-- flow control won't work either, use ternary expressions -->

{{ if (ok) { return message } }}

```

## DIRECTIVAS

Las directivas son atributos especiales con el prefijo `v-`. Los valores de los atributos de las directivas se espera para ser un expresión simple o única de javascript(exepto de `v-for`).

HTML

```
<p v-if="seen">Now you see me</p>

```

La directiva `v-if` podría remover ó insertar el elemento `<p> ` basado en la veracidad del valor de la expresión `seen` .

## Arguments 

Algunas directivas pueden tomar un argumento, denotado por `:` despues del nombre de la directiva. Por ejemplo, la directiva `v-bind` es usado para indicar actualización en atributo HTML:

HTML

```
<a v-bind:href="url"> ... </a>

```

`href` es el argumento, la cual la llama la directiva `v-bind` para vincular el atributo `href` del elemento para el valor de la expresión `url`.


Otro ejemplo es la directiva `v-on`, la cual escucha los eventos del DOM.

HTML 

```
<a v-on:click="doSomething"> ... </a>

```

### Argumentos Dinamicos

### New in 2.6.0+

En la versión 2.6.0 tambien es posible usar expresiones javascript, con el uso de los corchetes `[]`:

HTML 

```
<a v-bind:[attributeName]="url"> ... </a>

```

`attributeName` se evaluará dinamicamente como un expresión de javascript, y su valor evaluado se utilizará como valor final para el argumento. Por ejemplo, si su instancia de Vue tiene una propiedad de datos, `attributeName`, cuyo valor es `"href"`; entonces este enlace será equivalente a `v-bind:href`.

similarmente, se puede usar argumentos dinámicos para enlazar un handler a un nombre de evento dinámico 

HTML

```
<a v-on:[eventName]="doSomething"> ... </a>

```
Los argumentos dinamicos son esperados para evaluar a un string, con la excepción de `null`. El valor `null` puede ser usado para remover explicitamente el enlace. cualquier otro valor `non-string` accionará un `warning`


Las expresiones de argumento dinámicos tienen algunas restricciones de sintaxis porque ciertos caracteres no son válidos dentro de los nombres de atributos HTML, como espacios y comillas. También debe evitar las claves en mayúsculas cuando utilice plantillas en DOM. 

Por ejemplo, el siguiente es invalido:


HTML

```
<!-- This will trigger a compiler warning. -->
<a v-bind:['foo' + bar]="value"> ... </a>

```

Además, si estas utilizando plantillas `in-DOM` (plantillas escritas directamente en un archivo HTML), debe tener en cuenta que los navegadores coaccionarán los nombres de atributos en minúsculas: 


HTML

```
<!-- This will be converted to v-bind:[someattr] in in-DOM templates. -->
<a v-bind:[someAttr]="value"> ... </a>

```

### Modificadores

Los modificadores son postfijos especiales denotado por un punto, que indica que una directiva debe estar vinculada de alguna manera especial. Por ejemplo, el modificador  `.prevent` le dice a la directiva `v-on` que llame a `event.preventDefault()` en el evento desencadenado: 

HTML

```
<form v-on:submit.prevent="onSubmit"> ... </form>

```

### Shorthands

El prefijo `v-` sirve como una pista visual para identificar atributos específicos de Vue en sus plantillas. Esto es útil cuando está usando Vue. js para aplicar un comportamiento dinámico a algunas marcas existentes, pero puede parecer "verbose" para algunas directivas usadas con frecuencia. Al mismo tiempo, la necesidad del prefijo v- se hace menos importante cuando se está construyendo un SPA, donde Vue gestiona cada plantilla. Por lo tanto, Vue proporciona abreviaturas especiales para dos de las directivas más utilizadas, v-bind y v-on: 



HTML `v-bind` (abreviado)

```
<!-- full syntax -->
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a :[key]="url"> ... </a>

```

HTML `v-on` (abreviado)

```
<!-- full syntax -->
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a @[event]="doSomething"> ... </a>

```


Pueden parecer un poco diferente al HTML normal, pero `:` y `@` son caracteres válidos para nombres de atributos y todos los navegadores compatibles con Vue pueden analizarlos correctamente. Además, no aparecen en el marcado final renderizado. La sintaxis abreviada es totalmente opcional.


