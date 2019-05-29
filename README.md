
## VUE JS ##

Es un framework progresivo para construir interfaces de usuarios. Vue está diseñado desde cero para ser adoptado de forma incremental, es fácil de recoger e integrar con otras librerias o proyectos existentes.
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


### Propiedades Calculadas y Observadores

## Propiedades calculadas

Las expresiones en plantillas son muy convenientes, pero ellos son concebidas por operaciones simples.

HTML 

```
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>

```
### Ejemplo básico

HTML

```
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>

```

JS

```

var vm = new Vue ({
  el: 'example',
  data: {
     
     message: 'Hola Mundo'

  },
  computed:{
    //a compured getter

    reservedMessage: function (){

        // this -> point to the vm intance

        return this.message.split('').reverse().join('')

    }
  }

}) 

```
Resultado: 

```
Original message: " Hola Mundo "
Computed reversed message: " odnuM aloH "

```

Aquí hemos declarado la propiedad calculada `reversedMessage` . La función será usada como la función getter para la propiedad `vm.reversedMessage` :

JS

```
console.log(vm.reversedMessage) // => 'odnuM aloH'
vm.message = 'Adios'
console.log(vm.reversedMessage) // => 'soidA'

```

El valor de `vm.reversedMessage` es simpre dependiente del valor de `vm.message`

### Caché calculado vs Método

Se puede notar que se puede alcanzar el mismo resultado al invocar el método en la expresión:

HTML

```
<p> Reversed message: " {{ reverseMessage() }} " </p>

```

JS  

```
// in component

methods: {

  reverseMessage: function(){

    return this.message.split('').reverve().join('')

  }

}

```

En el lugar de la propiedad calculada, nosotros podemos definir la misma función como un método. La diferencia es que las propiedades calculadas es basada en el caché de sus dependencias reactivas.

Una propiedad calculada se reevalua solamente cuando algunas de sus dependencias reactivas han cambiado.
Esto significa que mientras `message` no cambié, los accesos multiples a `reversedMessage` de la propiedad calculada returnará inmediatamente al resultado previamente calculado sin tener que correr la función otra vez.

La siguiente propiedad calculada nunca actualizará, porque `Date.now()` esta no es una dependencia reactiva.

JS

```
compured:{
  now: function (){
    return Date.now()
  }
}

```

¿Por que necesitamos cachear?

Imaginemos que tenemos una costosa propiedad computarizada `A`, que requiere hacer un lazo a través de una enorme matriz y hacer muchos cálculos. Entonces podemos tener otras propiedads calculadas que a su vez depende de `A`. Sin cachear estaríamos ejecutando el getter de `A` muchas mas veces de las necesarias , en los casos que no se requieran almacenar caché, se utilizará un método en su lugar.

### Calculo vs Propiedades Observadas

Vue proporciona una forma más genérica de observar y reaccionar a los cambios de datos en una instancia de Vue: las propiedades del reloj. 

Cuando se tiene algunos datos que necesitan ser cambiados en base a otros datos, es tentador abusar de `watch`- especialmente si usted viene de un entorno AngularJS. Sin embargo, a menudo es una mejor idea usar una propiedad computarizada en lugar de una llamada de "callback" `watch` imperativa. Considere este ejemplo: 

HTML

```
<div id="demo">{{ fullName }}</div>

```

JS

```

var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})


```

El código anterior es imperativo y repetitivo. Compárelo con una versión de propiedad computarizada: 

JS

```

var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})

```

### Calculo Setter 

JS

```
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...

```


Ahora cuando se corre `vm.fullName = 'John Doe'`, el setter será invocado y `vm.firstName` y `vm.lastName` será actualizada.

## Observadores

Aunque las propiedades calculadas son más apropiadas en la mayoría de los casos, hay ocasiones en las que se necesita un observador personalizado. Es por eso que Vue proporciona una manera más genérica de reaccionar a los cambios de datos a través de la opción de reloj. Esto es muy útil cuando se desea realizar operaciones asincrónicas o costosas en respuesta a la modificación de datos. 

Por ejemplo:

HTML

```
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>

```

HTML

```
<!-- Since there is already a rich ecosystem of ajax libraries    -->
<!-- and collections of general-purpose utility methods, Vue core -->
<!-- is able to remain small by not reinventing them. This also   -->
<!-- gives you the freedom to use what you're familiar with.      -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // whenever question changes, this function will run
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    // _.debounce is a function provided by lodash to limit how
    // often a particularly expensive operation can be run.
    // In this ca.$watch API., we want to limit how often we access
    // yesno.wtf/.$watch API.i, waiting until the user has completely
    // finished t.$watch API.ing before making the ajax request. To learn
    // more about.$watch API.he _.debounce function (and its cousin
    // _.throttle.$watch API. visit: https://lodash.com/docs#debounce.$watch API.
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Questions usually contain a question mark. ;-)'
        return
      }
      this.answer = 'Thinking...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Error! Could not reach the API. ' + error
        })
    }
  }
})
</script>

```

Resultado:

```
Ask a yes/no question:

<input/>

```

En este caso, usando la opción `watch` nos permite alcanzar una operaciòn asincrona, limitar la frecuencia con la que realizamos esa operación, y establecer estados intermedios hasta obtener una respuesta final. Nada de eso sería posible con una propiedad computarizada. 

Además la opción `watch` se puede usar el imperactivo `vm.$watch API`.

### Class y estilos vinculantes ###

## Clases HTML vinculantes

### Sintaxis de Objectos

Podemos pasar un objeto a `v-bind:class` para cambiar clases dinamicamente:


HTML
```
<div v-bind:class="{ active: isActive }"></div>

```
La sintaxis anterior significa que la presencia de la clase activa estará determinada por la veracidad de la propiedad de los datos `isActive`.

Se puede tener clases múltiples alternadas al tener más campos en el objeto. Además, la directiva `v-bind:class` también puede coexistir con el atributo plano `class`. Así que, dada la siguiente plantilla: 

HTML

```
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>

```

Y la data siguiente: 

JS

```
data: {
  isActive: true,
  hasError: false
}

```

Esto renderizará:

HTML

```
<div class="static active"></div>

```

Cuando cambia `isActive` o `hasError`, la lista de clases se actualizará en consecuencia. 
Por ejemplo, si `hasError` se convierte en `true`, la lista de clases se convertirá en `"static active text-danger"`. 

El objeto no tiene que estar alrededor de la línea: 

HTML

```
<div v-bind:class="classObject"></div>

```
JS

```
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}


```

Esto dará el mismo resultado. También podemos enlazar a una propiedad calculada que devuelva a un objeto. Esto es un patrón común y poderoso: 

HTML

```
<div v-bind:class="classObject"></div>

```

JS

```

data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}


```

### Sintaxis de arreglo

Podemos pasar un arreglo a `v-bind:class` para aplicar una lista de clases:

HTML

```
<div v-bind:class="[activeClass, errorClass]"></div>

```

JS

```
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}

```

La cual renderizará:

HTML

```
<div class="active text-danger"></div>

```

También podríamos cambiar una clase en una lista condionalmente, podemos hacerlo con una expresión ternaria:

HTML

```
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>

```

Esto nos dará siempre `errorClass`, sin embargo podemos aplicarlo con `activeClass` cuando `isActive` es verdadero.

No obstante esto puede ser un poco engorroso si tenemos multiples clases condionales. Esto es por que es posible usar la sintaxis del objeto dentro de la sintaxis del arreglo:

HTML

```
<div v-bind:class="[{ active: isActive }, errorClass]"></div>

```

### Con componentes

Cuando se utiliza el atributo `class` sobre componentes personalizados, estas clases serán agregadas a los elementos raíz de los componentes. Las clases existentes en estos elementos no serán sobreescritos.

Por ejemplo, si se declará esta componentes:

 JS

```

Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})

```

Entonces agregamos alguna clase usando:

HTML

```

<my-component class="baz boo"></my-component>

```

El HTML renderizado será:

HTML

```
<p class="foo bar baz boo">Hi</p>

```

Lo mismo ocurre con las vinculaciones las de clases: 

HTML

```
<my-component v-bind:class="{ active: isActive }"></my-component>

```

Cuando ` isActive ` es verdadero, el HTML renderizado será:

HTML

```
<p class="foo bar active">Hi</p>

```

## Estilos de vinculación en línea

### Objeto sintaxis


La sintaxis de los objetos para `v-bind:style` es bastante sencilla - se parece casi a CSS, excepto en los objetos JavaScript. Podemos nosotros usar `camelCase` o `kebab-case` (usar comillas con kebab-case) para los nombres de las propiedades CSS: 

HTML

```
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>

```

JS

```
data: {
  activeColor: 'red',
  fontSize: 30
}

```

Es una buena idea vincular a un objeto de estilos directamente para que la plantilla este más limpia:

HTML

```

<div v-bind:style="styleObject"></div>

```

JS 

```
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}

```

De nuevo, la sintaxis del objeto es a menudo usado en la conjunción con propiedades caculadas que retornan a un objeto.

### Sintaxis del arreglo

La sintaxis del arreglo por `v-bind: style` nos permite aplicar objetos de multiples estilos a el mismo elemento:

HTML

```
<div v-bind:style="[baseStyles, overridingStyles]"></div>

```

### Auto-prefijo

Cuando usamos una propiedad del CSS este requiere  `vendor prefixes` en `v-bind: styles`. Por ejemplo:
`transform`, Vue automaticamente detecta y agrega prefijos apropiados a los estilos aplicados.


### Valores Multiples

2.3.0

Comenzando en 2.3.0+ se puede proporcionar una matriz de valores múltiples (prefijados) a una propiedad de estilo, por ejemplo: 

HTML

```
<div v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>

```

Esto sólo renderizará el último valor en la matriz soportada en el navegador. En este ejemplo, mostrará: flex para navegadores que soporten la versión no prefija del flexbox. 


## Registro de Componentes

Cuando se registra un componente, a este siempre se le dará un nombre. Por ejemplo, en el registro global que hemos visto hasta ahora veremos lo siguiente:

JS

```
Vue.component('my-component-name', {/* ... */})

```

El nombre del componente es el primer argumento de `Vue.component`.

El nombre que se le da al componente podría depender del lugar donde se requiera utilizar.
Cuando usas un componente directamente en el DOM (en lugar de en una plantilla de cadena o en componente de archivo unico), nosotros estrictamente recomendamos seguir las reglas de W3C para personalizar los nombre de las etiquetas (todas-minusculas, deben contener un guión ). Te ayuda a evitar conflictos con los elementos actuales y futuros del HTML.

Puedes ver otras recomendaciones para el nombre de las name en el [Style Guide](https://vuejs.org/v2/name/#Base-component-names-strongly-recommended)
name

### Casos de Nombres (Name Casing)

Se tinen dos opciones cuando definimos nombre de los componentes:

**Con kebab-case**

JS

```
Vue.component('my-component-name', { /* ... */ })

```

Cuando definimos una componente con **kebab-case**, tambien se debe usar **kebab-case** cuando nos referimos a elementos personalizados, tal como `<my-component-name>`

**Con PascalCase**

Vue.component('MyComponentName', { /* ... */})

Cuando definimos un componente con `PascalCase` , podemos usar cualquier caso cuando nos referimos a sus elementos personalizados. Esto significa que `<my-component-name>` y `<MyComponentName>` son aceptados.

**Nota: sin embargo, el nombre kebab-case es valido directamente en el DOM**


## Registro Global

Hemos creado componentes usando `Vue.component` :

JS
```
Vue.component('my-component-name', { // ... options ... })

```
Estas componentes estan registradas globalmente. Esto significa que estos pueden ser usadas en las plantillas de cualquier raíz de la instancia de Vue (new Vue) creadas despues del registro. Por ejemplo:

JS
```
Vue.component('component-a', { // ... options ... })
Vue.component('component-b', { // ... options ... })
Vue.component('component-c', { // ... options ... })

new Vue({el: '#app'})

```
HTML

```
<div id="app">
  <component-a></component-a>
  <component-b></component-b>
  <component-c></component-c>
</div>

```

Esto incluso aplica a todos los subcomponentes, es decir que los tres componentes tambien estan disponibles uno dentro de otro.

## Registro Local

El registro Global en ocaciones no es ideal. Por ejemplo, si queremos usar un sistema de compilación como Webpack, el registro global de todos los componentes significa que incluso si deja de utilizar un componente, éste podría incluirse en su compilación final. Esto aumenta innecesariamente la cantidad de Javascript que sus usuarios tienen que descargar.


JS

```
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }

```

Luego:

JS

```
new Vue({

  el: 'app',
  components:{
    'component-a: ComponentA,
    'component-b: ComponentB
  }
})

```

Para cada propiedad en el componente del objeto, la clave será el nombre del elemento personalizado, mientras que el valor contendrá el opciones del objeto para el componente.

Tenga en cuenta que los componentes registrados localmente no están disponibles en los subcomponentes. Por ejemplo, si quisiera que `ComponentA` estuviera disponible en `ComponentB`, tendría que utilizarlo: 

JS

```
var ComponentA = {/* ... */}
var ComponentB = {/* ... */}

var ComponentB = {
  components: {
    'component-a: ComponentA'
  },
  // ...
}

```

ó si estamos usando modelos ES2015, tales como el Babel y Webpack,podría verse como sigue:

JS

```
import ComponentA from './ComponentA.vue'


export default {
  components:{
    ComponentA
  },
  // ...
}

```

Note que en ES2015+, colocar un nombre de variable como `ComponenteA` dentro de un objeto es la abreviatura de `ComponenteA: ComponenteA`, lo que significa que el nombre de la variable son ambas:

1. el nombre del elemento personalizado que se utilizará en la plantilla, y
2. el nombre de la variable que contiene las opciones del componente


## Modulos del sistema

Si no estas usando algun módulo del sistema `import/require`, se puede problablemente saltar esta sección por ahora. Al contrario, tenemos algunas instrucciones y tips:

### Registro Local dentro de un modulo del sistema

Se recomienda crear un directorio o carpeta `components`, y cada componente estará dentro de su propio archivo.

Luego se necesitará importar cada componente que se podría utilizar, despues de registrar el mismo localmente.
Por ejemplo, en un archivo hipotètico ComponentB.js ò ComponentsB.vue:

JS 

```

import ComponetnA from './ComponentA'
import ComponentC from './ComponentC'

export default {
  components: {
    ComponentA,
    ComponentC
  },
  // ...
}

```

Ahora ambos componentes `ComponentA` y `ComponentC` pueden ser usados dentro de la plantilla `ComponentB`.

### Registro global Automático de componentes base

Muchos de nuestros componentes serán relativamente genéricos, posiblemente sólo involucrando un elemento como una entrada o un botón. Nosotros nos referimos a estos como componentes base y ellos tienden a ser usados frecuentemente a través de los componentes.
El resultado es que muchos de estos componentes podrian incluir una lista extensa de componentes base:
 
 
 JS

```
import BaseButton from './BaseButton.vue'
import BaseIcon from './BaseIcon.vue'
import BaseInput from './BaseInput.vue'



export default {
  components: {
    BaseButton,
    BaseIcon,
    BaseInput
  }
}

```

HTML

```
<BaseInput
  v-model="searchText"
  @keydown.enter="search"
/>
<BaseButton @click="search">
  <BaseIcon name="search"/>
</BaseButton>


```

Afortunadamente si se esta usando Webpack (ó Vue CLI3+, la cual usa internamente Webpack), se puede usar `require.context` para el registro global. Aquí un ejemplo de código que podría usar componentes base importado globalmente dentro de la carpeta de nuestra app (ejemplo: src/main.js)


JS

```

import Vue from 'vue'
import upperFirts from 'lodash/upperFirst'
import camelCase from 'lodash/camelCase'



const requireComponent = require.context(
  // The relative path of the components folder
  './components',
  // Whether or not to look in subfolders
  false,
  // The regular expression used to match base component filenames
  /Base[A-Z]\w+\.(vue|js)$/
)

requireComponent.keys().forEach(fileName => {
  // Get component config
  const componentConfig = requireComponent(fileName)

  // Get PascalCase name of component
  const componentName = upperFirst(
    camelCase(
      // Gets the file name regardless of folder depth
      fileName
        .split('/')
        .pop()
        .replace(/\.\w+$/, '')
    )
  )


  // Register component globally
  Vue.component(
    componentName,
    // Look for the component options on `.default`, which will
    // exist if the component was exported with `export default`,
    // otherwise fall back to module's root.
    componentConfig.default || componentConfig
  )
})


```

***Recordar que el registro global debe tomar lugar antes de que la raíz de la instancia Vue sea creada (new Vue)***

## Caso Prop (camenCase vs kebab-case) ##

Los nombres de atributos HTML no distinguen entre mayúsculas y minúsculas, por lo que los navegadores interpretan los caracteres en mayúsculas como minúsculas. Esto significa que cuando usas plantillas en DOM, los nombres de prop `camelCased` necesitan usar sus equivalentes en `kebab-case` (delimitados por guiones): 


JS 


```

Vue.component('blog-post',{
//camelCase in Javascript 

props: ['postTitle'],
template:'<h3>{{ postTitle }}</h3>'


})

```

HTML

```
<blog-post post-title= "hello!"></blog-post>

```

Si se esta usando plantillas string, estas limitaciones no aplican

### Tipo de Prop

**Arreglos de strings:**


JS

```
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']

```

Se puede listar el props como un obejto, donde los nombre de las propiedades y valores, cuentan como nombres prop y tipos, respectivamente:

JS

```

props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}


```

## Pasando Props estáticos y dinámicos

**Pasando props a valores estáticos:**

HTML

```
<blog-post title="My journey with Vue"></blog-post>

```

**Props asignados dinamicamente**


HTML

```
<blog-post title="My journey with Vue"></blog-post>


<!-- Dynamically assign the value of a complex expression -->
<blog-post
  v-bind:title="post.title + ' by ' + post.author.name"
></blog-post>

```

 ### Pasando un número o number

HTML

 ```

<!-- Even though `42` is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.       -->
<blog-post v-bind:likes="42"></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post v-bind:likes="post.likes"></blog-post>

 ```

 ### Pasando un booleano o Boolean

HTML

 ```
<!-- Including the prop with no value will imply `true`. -->
<blog-post is-published></blog-post>

<!-- Even though `false` is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.          -->
<blog-post v-bind:is-published="false"></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post v-bind:is-published="post.isPublished"></blog-post>

 ```


### Pasando un Arreglo

 ```

<!-- Even though the array is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.            -->
<blog-post v-bind:comment-ids="[234, 266, 273]"></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post v-bind:comment-ids="post.commentIds"></blog-post>

 ```


### Pasando un Objeto

HTML

```
<!-- Even though the object is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.             -->
<blog-post
  v-bind:author="{
    name: 'Veronica',
    company: 'Veridian Dynamics'
  }"
></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post v-bind:author="post.author"></blog-post>

```

### Pasando las propiedadesde unn objeto

Si se desea pasar todas las propiedades de un objeto como props, se puede usar `v-bind` sin el argumento (v-bind en lugar de v-bind: prop-name). Por ejemplo, dado un objeto post:

JS


```
post: {
  id: 1,
  title: 'My Journey with Vue'
}


```

La siguiente plantilla sería:

HTML

```
<blog-post v-bind="post"></blog-post>

```

Será equivalente a: 

HTML

```

<blog-post
  v-bind:id="post.id"
  v-bind:title="post.title"
></blog-post>

```

### Flujo de datos unidireccional

Todos los accesorios forman un vínculo unidireccional entre la propiedad hijo y la propiedad padre: cuando la propiedad padre se actualiza, fluirá hacia el hijo pero no al revés. Esto evita que los componentes hijo muten accidentalmente a el estado padre, lo que puede hacer que el flujo de datos de su aplicación sea más difícil de entender.

Además, cada vez que el componente padre es actualizado todos los `props` en el componente hijo serán refrescado con el último valor. Esto significa que no intentaria mutar un prop dentro de un componente hijo. 

Para estos existen dos casos:

1. El prop es usado para inicializar valores; los componentes hijos quieren usar despues esto como una propiedad data local. 


JS

```

props: ['initialCounter'],
data: function () {
  return {
    counter: this.initialCounter
  }
}


```

2. El prop para ser transformado. En este caso es mejor definir una propiedad calculada usando los valores de props:

JS

```
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}

```

Nota: los objeto y arreglos en javacript son pasados por referencia, si el prop es un arreglo o objeto, la mutación del objeto o arreglo dentro del componente hijo afectará al estado padre. 


## Validación del Prop

Los componentes pueden requerir especificamente por su prop, como los que ya hemos visto. Si un requerimiento no es conocido, Vue lo mostrará en la consola del navegador.
Esto es útil cuando se desarrolla un componente que esta destinado a ser utilizado por otro.

Para especificar validaciones de `prop`, se puede proporcionar un objeto con requisitos de validación al valor de los `prop`, en lugar de un arreglo de cadenas. 

**Por ejemplo:** 

JS

```
Vue.component('my-component', {
  props: {
    // Basic type check (`null` and `undefined` values will pass any type validation)
    propA: Number,
    // Multiple possible types
    propB: [String, Number],
    // Required string
    propC: {
      type: String,
      required: true
    },
    // Number with a default value
    propD: {
      type: Number,
      default: 100
    },
    // Object with a default value
    propE: {
      type: Object,
      // Object or array defaults must be returned from
      // a factory function
      default: function () {
        return { message: 'hello' }
      }
    },
    // Custom validator function
    propF: {
      validator: function (value) {
        // The value must match one of these strings
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})

```

Cuando la validación del prop falla, Vue mostrará en la consola un error

**Nota: los prop seran validos antes de la creación de la instancia del componente, sino la propiedad de la instancia no estará disponible dentro de la funciones `default` ó `validator`.**  

### Tipos

Los tipos `type` pueden ser uno de los siguientes contructores nativos:


    String
    Number
    Boolean
    Array
    Object
    Date
    Function
    Symbol


Además, los tipos pueden tambien ser funciones de contructores personalizados y la inserción será realizada con `instanceof` . Por ejemplo, tenemos la siguiente función del constructor existente:

JS

```

function Person (firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}

```

se podría usar:

JS 

```

Vue.component('blog-post', {
  props: {
    author: Person
  }
})

```

para validar que el valor del objeto de `author` fue creada con `new Person`

## No-prop

Un atributo `non-prop` es un atributo que se pasa a un componente, pero no tiene un prop correspondiente definido.

Mientras un `prop` definido explícitamente pasan la información a un componete hijo, los autores de las librerías de los componentes no siempre se pueden proveer los contextos en los que se pueden utilizar sus componentes. Es por eso que los componentes pueden aceptar atributos arbitrarios, que se añaden al elemento raíz del componente.

Por ejemplo, imaginemos que estamos usando un componente de entrada `bootstrap-date-input` con un plugin de Bootstrap que requiere un atributo `data-date-picker` en la entrada. Podemos añadir este atributo a nuestra instancia de componente:

HTML

```
<bootstrap-date-input data-date-picker="activated"></bootstrap-date-input>

```

Y el atributo `data-date-picker="activated"` será agregado automáticamente a la raíz del elemento de `bootstrap-date-input`.

## Reemplazando/ fusionando con atributos existentes

Imagine que tenemos como plantilla `bootstrap-date-input`:


HTML
```
<input type="date" class="form-control">

```
Para especificar un tema para nuestro plugin de `date picker`, nosotros podríamos agregar una clase especifica, como este:

HTML

```

<bootstrap-date-input
  data-date-picker="activated"
  class="date-picker-theme-dark"
></bootstrap-date-input>


```
En este caso los dos diferentes valores para la clase estan definidos:

1. `form-control` :  envia estilos a los componentes.

2. `date-picker-theme-dark` : pasa a los componentes por sus padres.

La mayoría de los atributos, el valor proporcionado al componente reemplazará los valores establecido por los componente. Así que por ejemplo, pasando `type="text"`; reemplazará `type="date"`; y probablemente lo romperá! Afortunadamente, los atributos de clase y estilo (`class` y `style`) son un poco más hábil, por lo que ambos valores se fusionan, haciendo que el valor final: `form-control` `date-picker-theme-dark`.

### Desactivación de atributos de herencia
Si no se requiere que el elemento raíz de una componente de atributos sea heredado podemos enviar `inherintAttrs: false ` en las opciones del componente. Por ejemplo:

JS

```
Vue.component( 'my-component', {

    inheritAttrs: false,
})

```

Esto puede ser especialmente útil en combinanción con la propiedad de la instancia `$attrs` 

JS

```
{
  required :true,
  palceholder:'Enter your username'
}

```

JS

```
Vue.component('base-input', {
  inheritAttrs: false,
  props: ['label', 'value'],
  template: `
    <label>
      {{ label }}
      <input
        v-bind="$attrs"
        v-bind:value="value"
        v-on:input="$emit('input', $event.target.value)"
      >
    </label>
  `
})

```

**Nota: la opción de `inheritAttrs: false` no afectan los estilos y las clases vinculadas**


HTML

```
<base-input
  v-model="username"
  required
  placeholder="Enter your username"
></base-input>

```

Este patrón le permite utilizar componentes base más como elementos puros HTML, sin tener que preocuparse sobre cual elemento está actualmente en su raíz: 

HTML

```
<base-input
  v-model="username"
  required
  placeholder="Enter your username"
></base-input>

```

## Eventos personalizados ##

## Nombre de eventos


A diferencia de los componentes y `prop`, los nombres de los eventos no proporcionan ninguna transformación automática de casos. En su lugar, el nombre de un evento emitido debe coincidir exactamente con el nombre utilizado para escuchar ese evento. Por ejemplo, si se emite un nombre de evento `camelCased`: 
Por ejemplo, si enviamos un evento `camelCased`: 

JS

```
this.$emit('myEvent')

```

Escuchando a la versión de `kebab-cased` no tendrá efecto:

HTML

```
<!-- Won't work -->
<my-component v-on:my-event="doSomething"></my-component>

```

A diferencia de los componentes y props, los nombres de eventos nunca se utilizarán como nombres de variables o propiedades en JavaScript, por lo que no hay razón para usar `camelCase` ó `PascalCase`. Además, los oyentes de eventos `v-on` dentro de las plantillas DOM se transformarán automáticamente a minúsculas (debido a la insensibilidad a mayúsculas y minúsculas de HTML), de modo que `v-on:myEvent` se convertirá en `v-on:myevent` - haciendo que `myEvent` sea imposible de escuchar. 

Por esta razón se recomienda siempre usar `kebab-case ` para el nombre del evento.

## Personalización de componentes `vmodel` 2.2.0+

Por defecto, `v-model` como el `prop` y `input` como el evento, pero algunos tipos de entradas cada boton `checkboxes` y `radio` podría usar el valor del atributo `value` para un propósito diferente. Usando la opción `model` podemos evitar un conflicto en cada caso:

JS

```
Vue.component(base-checkbox,{

  model:{

    prop:'checked',
    event:'change'
  },
  props:{

    checked:Boolean

  },
  template:

    <input
      type="checkbox"
      v-bind:checked="checked"
      v-on:change="$emit('change', $event.target.checked)"
    >

})

```

Ahora cuando usamos `v-model` en este componente:

HTML 

```
<base-checkbox v-model="lovingVue"></base-checkbox>

```

el valor de `lovingVue` pasará a el prop `checked`. La propiedad `lovingVue` se actualizará cuando ` <base-checkbox>` emita un evento `change` con un nuevo valor. 

## Vinculación de eventos nativos a componentes 

Puede haber ocasiones en las que se desea escuchar directamente un evento nativo al elemento raíz de un componente. En estos casos, se puede utilizar el modificador `.native` para `v-on`:

HTML

```
<base-input v-on:focus.native="onFocus"></base-input>

```


Esto algunas veces puede ser de utilidad, sin embargo no es una buena idea cuando se trata de escuchar un elemento especifico, como un `<input>`. Por ejemplo, la componente `<base-input>` podría refactorizarse para que el elemento raíz sea realmente un elemento `<label>`

HTML 

```

<label>
  {{ label }}
  <input
    v-bind="$attrs"
    v-bind:value="value"
    v-on:input="$emit('input', $event.target.value)"
  >
</label>


```

En este caso, el oyente `.native` en el padre se romperìa silenciosamente. No habría errores, pero el `onFocus` no sería llamado cuando esperábamos.

Para resolver este problema, Vue proporciona una propiedad `$listeners` que contiene objetos de oyentes que suele usarse sobre el componente:

JS


```

{
  focus: function (event) { /* ... */ }
  input: function (value) { /* ... */ },
}


```

Usando la propiedad `$listeners` se puede enviar todos los eventos oyentes sobre los componentes a un elemento hijo especifico con `v-on= "$listener"`. Para los elementos como `<input>`, tambien se puede trabajar con `v-model` , esto a menudo es útil para crear una nueva propiedad calculada para oyentes, como `inputListeners`


JS

```


Vue.component('base-input', {
  inheritAttrs: false,
  props: ['label', 'value'],
  computed: {
    inputListeners: function () {
      var vm = this
      // `Object.assign` merges objects together to form a new object
      return Object.assign({},
        // We add all the listeners from the parent
        this.$listeners,
        // Then we can add custom listeners or override the
        // behavior of some listeners.
        {
          // This ensures that the component works with v-model
          input: function (event) {
            vm.$emit('input', event.target.value)
          }
        }
      )
    }
  },
  template: `
    <label>
      {{ label }}
      <input
        v-bind="$attrs"
        v-bind:value="value"
        v-on="inputListeners"
      >
    </label>
  `
})



```

Ahora el componente `<base-input>` es totalmente transparente, lo que significa que puede ser usado exactamente como un elemento `<input>` normal: todos los mismos atributos y oyentes funcionarán, sin el modificador `.native`. 

## .sync Modificador 2.3.0+

En algunos casos podríamos necesitar dos caminos de vinculación para un `prop`. Desafortunadamente, estos dos caminos de enlace crean problemas de mantenimiento, porque los componentes hijos pueden mutar al padre sin que la fuente de esa mutación este tanto en el padre como en el hijo. 

JS

```
this.$emit('update:title', newTitle)

```

Entonces el padre puede escuchar a ese evento y actualizar una propiedad de data local, por ejemplo:

HTML

```
<text-document
  v-bind:title="doc.title"
  v-on:update:title="doc.title = $event"
></text-document>

```

Atajos para los patrones con el modificador `.sync`:

HTML

```

<text-document v-bind:title.sync="doc.title"></text-document>

```

Nota: `v-bind` con el modificador `.sync` no trabaja con expresiones tal como: `v-bind:title.sync=”doc.title + ‘!’”`. En su lugar, solo debe proporcionar el nombre de la propiedad que se desea vincular, de forma similar a `v-model`.

El modificador `.sync` puede tambien ser usado con `v-bin` cuando usas un objetopara enviar `props` multiples :


HTML

```
<text-document v-bind.sync="doc"></text-document>

```

Éste pasa cada propiedad en el objeto `doc` (por ejemplo title) como un prop individual, luego agrega oyentes de `v-on` actualizados para cada uno. 

Usar `v-bind.sync` con un objeto literal, como en `v-bind.sync=";{ title: doc. title }`, no funcionará, porque hay demasiados casos para considerar al analizar una expresión compleja como ésta. 


## Contenido Slot

Vue implementa un contenido de distribución API inspirada por el 'Web components spec draft', usando el elemento `<slot>` para servir como puntos de distribución de contenido.

Este permite contener componentes como: 

HTML

```
<navigation-link url="/profile">
  Your Profile
</navigation-link>

```

Luego en la plantilla por `<navigation-link>` , podríamos tener:


```
<a
  v-bind:href="url"
  class="nav-link"
>
  <slot></slot>
</a>

```

Cuando el componente se muestre, `<slot></slot>` será reemplazado por "Your Profile". Los `slots` pueden contar con cualquier código de la plantilla, incluyendo HTML:

HTML

```
<navigation-link url="/profile">
  <!-- Add a Font Awesome icon -->
  <span class="fa fa-user"></span>
  Your Profile
</navigation-link>


```

ó inclusos otros componentes: 

HTML

```
<navigation-link url="/profile">
  <!-- Use a component to add an icon -->
  <font-awesome-icon name="user"></font-awesome-icon>
  Your Profile
</navigation-link>

```

Si la plantilla del `<navigation-link>` no cuenta con un elemento `<slot>` cualquier contenido proveniente entre sus etiquetas abiertas y cerradas serán descartadas.

## Alcance de la compilación

Cuando se requiera usar una data dentro de un slot:

HTML 

```
<navigation-link url="/profile">
  Logged in as {{ user.name }}
</navigation-link>

```

Ese slot tiene acceso a las mismas propiedades de la instancia (ejemplo el mismo scope) como el resto de la plantilla. El slot no tiene acceso al scope de  `<navigation-link>‘`. Por ejemplo, intentar acceder a la url no funcionará:

HTML

```

<navigation-link url="/profile">
  Clicking here will send you to: {{ url }}
  <!--
  The `url` will be undefined, because this content is passed
  _to_ <navigation-link>, rather than defined _inside_ the
  <navigation-link> component.
  -->
</navigation-link>



```

Toda plantilla padre es compilado dentro del scope padres, toda plantilla hijo es compilado dentro del scope hijo.

## Contenido Fallback

Hay casos en los que es útil especificar contenido alternativo (es decir, predeterminado) para una slot, que se renderizará sólo cuando no se proporcione contenido. Por ejemplo, en un componente `<submit-button>`: 

HTML

```
<button type="submit">
  <slot></slot>
</button>

```

Es posible que se requiera el contenido que este dentro un boton, Para hacer "Submit" del contenido alternativo o fallback , nosotros podemos colocarlo entre las etiquetas `<slot>` :

HTML

```
<button type="submit">
  <slot>Submit</slot>
</button>

```

Ahora cuando nosotros usamos `<submit-button>` en un componente padre, sin proporcionar contenido para el `slot`:

HTML 

```
<submit-button></submit-button>

```
mostrando contenido alternativo, "Submit":

HTML

```
<button type="submit">
  Submit
</button>

```

pero si nosotros preevemos el contenido:

HTML

```

<submit-button>
  Save
</submit-button>

```

Luego el contenido proveniente será mostrado en su lugar:

HTML

```
<button type="submit">
  Save
</button>

```

## Slots nombrados

Hay momentos en los que es útil tener multiples `slots`. Por ejemplo, en un componente `<base-layout>` con la siguiente plantilla: 

HTML

```
<div class="container">
  <header>
    <!-- We want header content here -->
  </header>
  <main>
    <!-- We want main content here -->
  </main>
  <footer>
    <!-- We want footer content here -->
  </footer>
</div>

```

Para este caso el elemento `<slot>` tiene un atributo especial, `name`, la cual puede ser usado para definir slots adicionales:

HTML

```
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>

```

Una salida `<slot>` sin `name` implicitamente tiene el nombre por defecto (default).

Para proporcionar contenido a `slots` con nombre, podemos usar la directiva `v-slot` en un `<template>`, proporcionando el nombre del slot `<slot>` como argumento de `v-slot`:

HTML

```
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>

```

Ahora cada cosa dentro del elemento `<element>` pasará a `slots` correspondiente. Cualquier contenido no envuelto en un `<template>` usando `v-slot` asume que será un slot por defecto.


HTML

```
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <template v-slot:default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>

```

Por otro lado el HTML mostrará:

HTML


```

<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>

```

Nota: `v-slot` puede solamente ser agregado a un `<template>` a diferencia del atributo `slot`


## Scoped Slots

Algunas veces, es útil que el contenido del `slot` tenga acceso a datos sólo disponibles en el componente hijo. Por ejemplo, imagine un componente `<current-user>` con la siguiente plantilla: 

HTML

```
<span>
  <slot>{{ user.lastName }}</slot>
</span>

```
Nosotros podriamos querer reeemplazar el contenido alternativo para mostrar el nombre del primer usuario, en lugar del último, como este:

HTML

```
<current-user>
  {{ user.firstName }}
</current-user>

```

Este no trabajará, porque solamente el componente `<current-user>` tiene acceso a el `user` y el contenido lo hemos mostrado en el padre.


Para hacer un `user` disponible para el contenido slot en el padre, nosotros podemos vincular `user` con un atributo del elemento `<slot>`

HTML

```
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>

```

Los atributos ligados a un elemento `<slot>` se denominan 'slop props'. Ahora, en el scope padre, podemos usar `v-slot` con un valor para definir un nombre para los slop props que nos han proporcionado: 

HTML

```
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>

```

En este ejemplo, hemos elegido el nombre del objeto que contiene todos nuestros `slot props` (slotProps), pero puedes usar el nombre que quieras.

### Sintaxis abreviada para los slots por defecto de Lone 

En el caso que solamente el `slot` por defecto provenga del contenido, las etiquetas de los componentes pueden ser usadas como las plantillas del slot. Esto nos permite usar directamente el `v-slot` en el componente:

HTML

```
<current-user v-slot:default="slotProps">
  {{ slotProps.user.firstName }}
</current-user>

```

Tambien se puede colocar sin el argumento ya que asume que esta por default:

HTML

```
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```

Nota: esta sintaxis del slot por defecto no puede estar mezclado con otros `slots`:

HTML 

```

<!-- INVALID, will result in warning -->
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
  <template v-slot:other=" ES2015 destructuringotherSlotProps">
    slotProps is NOT avail ES2015 destructuringable here
  </template> ES2015 destructuring
</current-user> ES2015 destructuring
 ES2015 destructuring
 ES2015 destructuring
 ES2015 destructuring
 ES2015 destructuring

 ``` 
Normalmente cuando hay var ES2015 destructuringios slots se usa la sintaxis basada en varios `<template>` para todos los `slots`:

HTML

```

<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>

  <template v-slot:other="otherSlotProps">
    ...
  </template>
</current-user>


```


### Destructuración del Slot Props


Internamente, el trabajo del slot se realizará dentro de una función que pasará a un argumento simple:

JS

```

function (slotProps){
// ... contenido del slot
}

```

Eso significa que el valor de `v-slot` puede actualmente aceptar cualquier valor de la expresion de Javascript esto puede aparecer en la posición del argumento de una función definida. Tambien puedes hacer uso de [ES2015 destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Object_destructuring) para especificar `slot props`:

HTML

```
<current-user v-slot="{ user }">
  {{ user.firstName }}
</current-user>

```

Esto puede ser mas limpio especialmente cuando proviene de muchos props. Esto tambien abre muchas posibilidades, como renombrar props, ejemplo `user` a `person`:


HTML

```
<current-user v-slot="{ user: person }">
  {{ person.firstName }}
</current-user>

```

Puedes incluso definir alternativas 'fallbacks' para ser usados en caso de que un slot prop este indefinido:

HTML

```

<current-user v-slot="{ user = { firstName: 'Guest' } }">
  {{ user.firstName }}
</current-user>

```

## Nombre de Slot dinamicos 2.6.0+

Los argumentos de directivas dinamicas tambien trabajan en `v-slot` permitiendo la definición de nombres de slot dinamicos:



HTML

```

<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>


```

## Atajos de Slots nombrados


Similar al `v-on` y `v-bind`, `v-slot` tambien tiene atajos, reemplazando cada cosa antes del argumento con el simbolo `#`. Por ejemplo, `v-slot:header` pueden reescribirse como `#header`:

HTML

```
<base-layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>

```

Sin embargo, hasta con otras directivas, el atajo esta siempre disponible cuando un argumento es proporcionado.
Por lo que la siguiente sintaxis es invalidad:

HTML

```
<!-- This will trigger a warning -->
<current-user #="{ user }">
  {{ user.firstName }}
</current-user>

```

En su lugar se debe siempre especificar el nombre de el slot si se desea usar el atajo:

HTML

```
<current-user #default="{ user }">
  {{ user.firstName }}
</current-user>

```

### Otros ejemplos

Los slot prop nos permite transformar el slots dentro de las plantillas reutilizables que puedan mostrar diferentes contenidos basados en entradas props. Esto mayormente es útil cuando se esta diseñando un componente reutilizable que encapsula la logica de la data mientras permite consumir el componente padre para personalizar parte de su layout.

Por ejemplo, la componente `<todo-list>` cuenta el layout y filtrando la lógica para una lista:

HTML

```
<ul>
  <li
    v-for="todo in filteredTodos"
    v-bind:key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>

```


En el lugar de un codigo complejo el contenido de cada `todo` podemos permitir que el componente padre tome el control por hacer cada `todo` un slot, luego vincular `todo` como un slot prop: 

HTML

```
<ul>
  <li
    v-for="todo in filteredTodos"
    v-bind:key="todo.id"
  >
    <!--
    We have a slot for each todo, passing it the
    `todo` object as a slot prop.
    -->
    <slot name="todo" v-bind:todo="todo">
      <!-- Fallback content -->
      {{ todo.text }}
    </slot>
  </li>
</ul>

```

Ahora cuando nosotros usamos el componente `<todo-list>`, nosotros podemos definir opcionalmente una alternativa `<template>` por el item `todo`, pero con el acceso para la data desde el niño:

HTML

```
<todo-list v-bind:todos="todos">
  <template v-slot:todo="{ todo }">
    <span v-if="todo.isComplete">✓</span>
    {{ todo.text }}
  </template>
</todo-list>

```

## Sintaxis no apropiado

La directiva `v-slot` se introdujo en Vue 2.6.0, ofreciendo de esta manera una API mejorada y alternativa a los atributos de soportados de `slot` . 

### Slots nombrados con el atributo Slot 2.6.0+

Para pasar contenido a slot nombrados desde el padre, se utiliza el atributo especial del slot `<template> (usando el componente descrito <base-layout>` 

HTML

```
<base-layout>
  <template slot="header">
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template slot="footer">
    <p>Here's some contact info</p>
  </template>
</base-layout>

```

ó los atributos slot pueden ser tambien usados directamente en un elemento normal:

HTML

```
<base-layout>
  <h1 slot="header">Here might be a page title</h1>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <p slot="footer">Here's some contact info</p>
</base-layout>
```

Puede haber un slot sin nombre, la cual es el slot por defecto que sirve como `catch-all` para cualquier contenido.
Entonce sería:

HTML

```
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>

```

### SLOTS de alcance con el atributo slot-scope

Para recibir props pasados a slot, el componente padre puede usar `<template>` con el atributo `slot-scope` (usando `<slot-example>`):

HTML


```
<slot-example>
  <template slot="default" slot-scope="slotProps">
    {{ slotProps.msg }}
  </template>
</slot-example>
```

Aquí el `slot-scope` declara el object props recibido como una variable de slotProps y lo hace disponible dentro del `<template>` Se puede nombrar el slotprop como se desee pero de forma similar a los argumentos de las funciones de los argumentos de javasacript.

`slot= "default"`

HTML

```
<slot-example>
  <template slot-scope="slotProps">
    {{ slotProps.msg }}
  </template>
</slot-example>

```

El atributo `slot-scope` puede ser usado directamente en un elemento non-`<template>` 

HTML

```
<slot-example>
  <span slot-scope="slotProps">
    {{ slotProps.msg }}
  </span>
</slot-example>


```

El valor de `slot-scope` puede aceptar cualquiera expresión de javascript esto puede aparecer en la posición del argumento de una función definida:

HTML

 ```
 <slot-example>
  <span slot-scope="{ msg }">
    {{ msg }}
  </span>
</slot-example>

```

Usando el `<todo-list>` descrito aquí, es equivalente a usar `slot-scope`:

HTML

```
<todo-list v-bind:todos="todos">
  <template slot="todo" slot-scope="{ todo }">
    <span v-if="todo.isComplete">✓</span>
    {{ todo.text }}
  </template>
</todo-list>

```

## Componentes dinamicos y asincronicos

`keep-alive` con componentes dinámicos

Anteriormente usabamos este atributo para cambiar entre componentes de una interfaz de pestañas:

HMTL

```
<component v-bind:is="currentTabComponent"></component>

```

Sin embargo, cuando entre estos cambien los componentes a veces se querra mantener su estado o evitar volver a rederizarlos por razones de rendimiento.


Notarás que si seleccionas un mensaje, cambias a la pestaña Archivar, luego vuelves a Cambiar a Mensajes, ya no se muestra el mensaje que seleccionaste. Esto se debe a que cada vez que se cambia a una nueva pestaña, Vue crea una nueva instancia del `currentTabComponente`.

Recrear componentes dinámicos es normalmente un comportamiento útil, pero en este caso, nos gustaría que esas instancias de componentes de pestañas se almacenaran en caché una vez que se hayan creado por primera vez. Para resolver este problema, podemos envolver nuestro componente dinámico con un elemento `<keep-alive>`: 

HTML

```
<!-- Inactive components will be cached! -->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>

```

Tenga en cuenta que `<keep-alive>` requiere que los componentes entre los que se cambia a todos tengan nombres, ya sea usando la opción de nombre en un componente, o a través del registro local/global. 

### Componentes Asincrónicos

En aplicaciones grandes, es posible que tengamos que dividir la aplicación en trozos más pequeños y sólo cargar un componente del servidor cuando sea necesario. Para facilitar esto, Vue le permite definir su componente como una función de fábrica que resuelve asincrónicamente la definición de su componente. Vue sólo activará la función de fábrica cuando el componente necesite ser renderizado y almacenará en caché el resultado para futuros re-envíos. Por ejemplo: 

JS

```

Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // Pass the component definition to the resolve callback
    resolve({
      template: '<div>I am async!</div>'
    })
  }, 1000)
})

```

Como puede ver, la función de fábrica recibe una llamada de resolve, que debería llamarse cuando haya recuperado la definición de su componente del servidor. También puede llamar a reject(reason) para indicar que la carga ha fallado. El setTimeout aquí es para demostración; cómo recuperar el componente depende de usted. Un enfoque recomendado es utilizar componentes de asincronía junto con la función de división de código de Webpack: 

JS
```
Vue.component('async-webpack-example', function (resolve) {
  // This special require syntax will instruct Webpack to
  // automatically split your built code into bundles which
  // are loaded over Ajax requests.
  require(['./my-async-component'], resolve)
})

```

Tambien puedes retornar a una promesa `Promise` 

JS
```
Vue.component(
  'async-webpack-example',
  // The `import` function returns a Promise.
  () => import('./my-async-component')
)
```

Cuando usamos registro local, se puede proveer directamente de una función esta returna a una promesa:

JS

```
new Vue({
  // ...
  components: {
    'my-component': () => import('./my-async-component')
  }
})

```

### Manipulación de estado de carga 2.3.0+

La fábrica de componentes asincrónicos tambien puede devolver un objeto del siguiente formato:

JS

```
const AsyncComponent = () => ({
  // The component to load (should be a Promise)
  component: import('./MyComponent.vue'),
  // A component to use while the async component is loading
  loading: LoadingComponent,
  // A component to use if the load fails
  error: ErrorComponent,
  // Delay before showing the loading component. Default: 200ms.
  delay: 200,
  // The error component will be displayed if a timeout is
  // provided and exceeded. Default: Infinity.
  timeout: 3000
})

```

## Manejos de casos de bordes

### Accesos de Elememntos y Componentes


La mayoría de los casos, es mejor evitar buscar en otras instancias de componentes o manipular manualmente los elementos DOM. Sin embargo, hay casos en los que puede ser apropiado:


### Acceso a la instancia raíz

En cada subcomponente de la nueva instancia new Vue, esta instancia raíz puede ser accedido con la propiedad `$root`: 
JS

```
// The root Vue instance
new Vue({
  data: {
    foo: 1
  },
  computed: {
    bar: function () { /* ... */ }
  },
  methods: {
    baz: function () { /* ... */ }
  }
})

```

Todos los subcomponentes ahora estaran disponibles para acceder a esta instancia y usar esto como una tienda global:

JS

```

// Get root data
this.$root.foo

// Set root data
this.$root.foo = 2

// Access root computed properties
this.$root.bar

// Call root methods
this.$root.baz()

```

### Acceso a la instancia del Componente Padre


Similar a `$root`, la propiedad padre puede ser usado para acceder a la instancia desde un hijo. Eso puede ser tentador para alcanzar como una alternativa a pasar datos con un prop.


Sin embargo, hay casos, en particular las bibliotecas de componentes compartidos, en los que esto podría ser apropiado. Por ejemplo, en componentes abstractos que interactúan con APIs de JavaScript en lugar de renderizar HTML, como estos hipotéticos componentes de Google Map:

HTML

```
<google-map>
  <google-map-markers v-bind:places="iceCreamShops"></google-map-markers>
</google-map>

```

El componente `<google-map>` podria definir una propiedad del map a las que todos los subcomponentes necesitan tener acceso. En este caso ` <google-map-markers> ` podría querer acceder a ese mapa con algo como `this.$parent.getMap` :

HTML
```
<google-map>
  <google-map-region v-bind:shape="cityBoundaries">
    <google-map-markers v-bind:places="iceCreamShops"></google-map-markers>
  </google-map-region>
</google-map>

```
En lugar de `<google-map-markers>` se podria encontrar buscando un hack como este:

JS 

```
var map = this.$parent.map || this.$parent.$parent.map

```

### Accediendo a la instancia de los componentes hijos y elementos hijos

A pesar de los props y eventos, algunas veces es posible que se necesite acceder directamente a un componente hijo en Javascript. Para ello, puede asignar un ID de referencia al componente hijo utilizando el atributo ref. Por ejemplo:


HTML

```
<base-input ref="usernameInput"></base-input>
```

Ahora en el componente donde has definido este `ref` se puede usar:

JS
```
this.$refs.usernameInput
```

para acceder a la instancia `<base-input>`. Este puede ser util cuando se requiera, por ejemplo enfocar  programaticamente esta entrada de un padre. En este caso, el componente `<base-input>` puede utilizar de forma similar a una ref para proporcionar acceso a elementos especificos dentro de él:

HTML
```
<input ref="input">
```

E incluso definir metodos para el uso de los padres

JS

 ```
methods: {
  // Used to focus the input from the parent
  focus: function () {
    this.$refs.input.focus()
  }
}
 ```

 Esto permite al componente padre enfocar la entrada dentro de `<base-input>` 

 JS

  ```
    this.$refs.usernameInput.focus()
  ```

Cuando el `ref` es usado junto con `v-for` la referencia que obtendra sera una matriz que contiene los componentes hijo que reflejan la fuente de los datos.

### Inyección de dependencia

Cuando describimos un acceso de la instancia de los componentes padres, mostramos un ejemplo como este:

HTML

```
<google-map>
  <google-map-region v-bind:shape="cityBoundaries">
    <google-map-markers v-bind:places="iceCreamShops"></google-map-markers>
  </google-map-region>
</google-map>
```
En este componente, todos los descendientes de `<google-map>` necesitan acceder a un metodo getMap para saber con que mapa interactuar. Desafortunadamente, el uso de la propiedad $parent no se adaptó bien a componentes anidados más profundos. Ahí es donde la inyección de dependencia puede ser útil, utilizando dos nuevas opciones de instancia: proporcionar e inyectar.

Las opciones de proporcionar nos permiten especificar los datos/métodos que queremos proporcionar a los componentes descendentes. Metodo `getMap` en lugar de `<google-map>`

JS

```
provide: function () {
  return {
    getMap: this.getMap
  }
}

```
Luego de cualquier decendencia nosotros podemos usar la opción `inject` para recibir especificamente propiedades que se agrego en la instancia:

JS

```
inject: ['getMap']

```

 La ventaja sobre el uso de `$parent` es que podemos acceder a `getMap` en cualquier componente descendente, sin exponer toda la instancia de `<google-map>`. Esto nos permite seguir desarrollando ese componente de forma más segura, sin temor a que podamos cambiar o eliminar algo en lo que confía un componente hijo. La interfaz entre estos componentes permanece claramente definida, al igual que en el caso de los `props`. 

 ### Listeners de oventos programáticos

 Hasta ahora hemos usado `$emit`, escuchado con `v-on`, pero la instancia Vue tambien ofrece otros metodos en su interfaz de eventos:

 1. Escuchar un evento con `$on(eventName, eventHandler)` 
 2. Escuchar un evento solamente una vez con `$once(eventName, eventHandler)`
 3. Deja de escuchar un evento con `$off(eventName, eventHandler)`

 Normalmente no tendrá que usarlos, pero están disponibles para los casos en los que necesite escuchar manualmente los eventos de una instancia de un componente. También pueden ser útiles como una herramienta de organización de código. Por ejemplo, es posible que a menudo vea este patrón para integrar una biblioteca de terceros: 

 JS 

 ```
// Attach the datepicker to an input once
// it's mounted to the DOM.
mounted: function () {
  // Pikaday is a 3rd-party datepicker library
  this.picker = new Pikaday({
    field: this.$refs.input,
    format: 'YYYY-MM-DD'
  })
},
// Right before the component is destroyed,
// also destroy the datepicker.
beforeDestroy: function () {
  this.picker.destroy()
}


 ```


Esto tiene dos potenciales:

1. Requiere guardar el `picker` en la instancia del componente, cuando es posible que sólo los ganchos del ciclo de vida necesiten acceder a él. Esto no es terrible, pero podría considerarse un desorden.
2. Nuestro código de configuración se mantiene separado de nuestro código de limpieza, lo que dificulta la limpieza programática de todo lo que configuramos.

Puede resolver ambos problemas con un oyente programático: 

JS

 ```
mounted: function () {
  var picker = new Pikaday({
    field: this.$refs.input,
    format: 'YYYY-MM-DD'
  })

  this.$once('hook:beforeDestroy', function () {
    picker.destroy()
  })
}

  ```

  Usando esta estrategia, podríamos incluso usar `Pikaday` con varios elementos de entrada, con cada nueva instancia limpiándose automáticamente después de sí misma: 

JS

  ```
mounted: function () {
  this.attachDatepicker('startDateInput')
  this.attachDatepicker('endDateInput')
},
methods: {
  attachDatepicker: function (refName) {
    var picker = new Pikaday({
      field: this.$refs[refName],
      format: 'YYYY-MM-DD'
    })

    this.$once('hook:beforeDestroy', function () {
      picker.destroy()
    })
  }
}
  ```

## Referencia circular

### Componentes recursivos

Los componentes pueden invocarse recursivamente en su propio modelo. Sin embargo solo puede hacerlo con la opción `name`:

JS

  ```
name: 'unique-name-of-my-component'


  ```

  Cuando se registra un componente globalmente usando `Vue.component` , el ID global es automaticamente enviado como la opcion de componeonte `name`.

  JS

  ```
Vue.component('unique-name-of-my-component', {
  // ...
})

  ```

  Si no se tiene cuidado los componentes recursivos pueden tambien conducir a lazos infinitos:
   
   JS

    
```
name: 'stack-overflow',
template: '<div><stack-overflow></stack-overflow></div>'
```

Un componente como el anterior resultará en un error de "tamaño máximo de pila excedido", así que asegúrese de que la invocación recursiva es condicional (es decir, usa una `v-if` esa eventualmente es falsa). 

### Referencia circular entre componentes

Supongamos que está construyendo un árbol de directorios de archivos, como en el Finder o el Explorador de archivos. Es posible que tenga un componente de carpeta de árbol con esta plantilla: 

HTML

```
<p>
  <span>{{ folder.name }}</span>
  <tree-folder-contents :children="folder.children"/>
</p>

```

Luego un `tree-folder-contents` componentes con esta plantilla:

HTML 
```
<ul>
  <li v-for="child in children">
    <tree-folder v-if="child.children" :folder="child"/>
    <span v-else>{{ child.name }}</span>
  </li>
</ul>

```


Cuando mires de cerca, verás que estos componentes serán en realidad los descendientes y antepasados del otro en el árbol de renderizado - ¡una paradoja! Al registrar componentes globalmente con Vue.component, esta paradoja se resuelve automáticamente. Si necesita/importa componentes utilizando un sistema de módulos, por ejemplo, a través de Webpack o Browserify, obtendrá un error:

```

Failed to mount component: template or render function not defined.

```

Para explicar lo que está sucediendo, llamemos a nuestros componentes A y B. El sistema de módulos ve que necesita A, pero primero A necesita a B, pero B necesita a A, pero A necesita a B, etc. Está atascado en un bucle, sin saber cómo resolver completamente ninguno de los dos componentes sin resolver primero el otro. Para arreglar esto, necesitamos darle al sistema modular un punto en el que pueda decir: "A necesita a B eventualmente, pero no hay necesidad de resolver a B primero".

En nuestro caso, hagamos de este punto el componente de `tree-folder`. Sabemos que el hijo que crea la paradoja es el componente `tree-folder-contents`, así que esperaremos hasta el gancho del ciclo de vida `beforeCreate para registrarlo`:

JS

```
beforeCreate: function () {
  this.$options.components.TreeFolderContents = require('./tree-folder-contents.vue').default
}

```

Ó alternativamente podemos usar webpacks asincrono `ìmport` cuando registre la componente localmente:

JS

```
components: {
  TreeFolderContents: () => import('./tree-folder-contents.vue')
}

```

## Definiciones de plantillas alternativas

### Plantillas en lineas

Cuando el atributo especial `inline-template` es presentado en componentes hijos, la componente se usará su contenido interno como plantilla, en lugar de tratarlo como contenido distribuido. Esto permite una creación de plantillas más flexibles.

HTML

```
<my-component inline-template>
  <div>
    <p>These are compiled as the component's own template.</p>
    <p>Not parent's transclusion content.</p>
  </div>
</my-component>
```

Sin embargo, ` inline-template` hace que el alcance de sus plantillas sea más difícil de razonar. Como mejor práctica, prefiera definir plantillas dentro del componente utilizando la opción `template` o en un elemento `<template>` en un archivo `.vue`. 

### X-Template

Otra forma de definir plantillas es dentro de un elemento de script con el tipo text/x-template, luego haciendo referencia a la plantilla mediante un id. Por ejemplo: 

HTML

```
<script type="text/x-template" id="hello-world-template">
  <p>Hello hello hello</p>
</script>
```

JS

```
Vue.component('hello-world', {
  template: '#hello-world-template'
})
```

## Controlando Actualizaciones


Gracias al sistema Reactivity de Vue, siempre sabe cuándo actualizar (si lo utiliza correctamente). Sin embargo, hay casos extremos en los que puede querer forzar una actualización, a pesar del hecho de que no ha cambiado ningún dato reactivo. Además, hay otros casos en los que podría querer evitar actualizaciones innecesarias. 

### Forzar una actualización 

Si usted necesita forzar una actualización en Vue, en el 99. 99% de los casos, ha cometido un error en alguna parte. 

Es posible que no haya tenido en cuenta las advertencias de detección de cambios con matrices u objetos, o que esté confiando en un estado que no es seguido por el sistema de reactividad de Vue, por ejemplo, con `date`.

Sin embargo, si ha descartado lo anterior y se encuentra en esta situación extremadamente rara de tener que forzar manualmente una actualización, puede hacerlo con `$forceUpdate`. 

### Componentes estáticos baratos con v-once

Renderizar elementos HTML simples es muy rápido en Vue, pero a veces puede que tenga un componente que contenga mucho contenido estático. En estos casos, puede asegurarse de que sólo se evalúa una vez y luego se almacena en caché añadiendo la directiva `v-once` al elemento raíz, de esta forma: 

JS

```
Vue.component('terms-of-service', {
  template: `
    <div v-once>
      <h1>Terms of Service</h1>
      ... a lot of static content ...
    </div>
  `
})

```

## Introducir/Dejar y Transiciones de Lista ##

### Transición de elementos individuales/componentes 

Vue proporciona un componente de envoltura `transition`, lo que le permite añadir transiciones de entrada/salida para cualquier elemento o componente en los siguientes contextos: 

1. Renderización condicional (usando `v-if`)
2. Visualización condicional (usando `v-show`)
3. Componentes dinamicos
4. Nodos raíz de componentes


HTML 

```
<div id="demo">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
</div>
```

JS

```
new Vue({
  el: '#demo',
  data: {
    show: true
  }
})


```

CSS 

```
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}


```

Cuando se inserta o se retira un elemento envuelto en un componente de transición `transition`, esto es lo que sucede:

1. Vue detectará automáticamente si el elemento de destino tiene transiciones CSS o animaciones aplicadas. Si lo hace, las clases de transición de CSS se agregarán/quitarán en los tiempos apropiados.

2. Si el componente de transición proporcionó hooks JavaScript, estos hooks se llamarán en el momento adecuado.

3. Si no se detectan transiciones/animaciones CSS y no se proporcionan hooks JavaScript, las operaciones DOM para inserción y/o eliminación se ejecutarán inmediatamente en el siguiente cuadro (Nota: se trata de un cuadro de animación del navegador, diferente del concepto de Vue de `nextTick`).

### Clases de transición

Hay seis clases que se aplican para las transiciones de entrada/salida. 

1. `v-enter`: Estado inicial para la entrada. Añadido antes de insertar el elemento, se retira un marco después de insertar el elemento.

2. `v-enter-active`: Estado activo para entrar. Se aplica durante toda la fase de entrada. Se agrega antes de insertar el elemento y se retira cuando termina la transición/animación. Esta clase se puede utilizar para definir la duración, el retardo y la curva de relajación para la transición de entrada.

3. `v-enter-to`: Sólo disponible en las versiones 2. 1. 8+. Estado final para la entrada. Se agregó un marco después de insertar el elemento (al mismo tiempo que se quita v-enter), se quita cuando termina la transición/animación.

4. `v-leave`: Estado de partida de la licencia. Se añade inmediatamente cuando se activa una transición de salida, que se elimina después de un fotograma.

5. `v-leave-active`: Estado activo para la licencia. Se aplica durante toda la fase de salida. Se añade inmediatamente cuando se activa la transición de salida y se elimina cuando finaliza la transición/animación. Esta clase se puede utilizar para definir la duración, el retardo y la curva de relajación para la transición de salida.

6. `v-leave-to`: Sólo disponible en las versiones 2. 1. 8+. Estado final de la licencia. Se agrega un fotograma después de que se activa una transición de salida (al mismo tiempo que se elimina el `v-leave`), que se elimina cuando finaliza la transición/animación.


Cada una de estas clases irá precedida del nombre de la transición. Aquí el prefijo v- es el predeterminado cuando se utiliza un elemento `<transition>` sin nombre. Si utiliza `<transition name="my-transición">`; por ejemplo, entonces la clase v-enter será en su lugar my-transition-enter.

`v-enter-active y v-leave-active` le ofrecen la posibilidad de especificar diferentes curvas de relajación para las transiciones de entrada/salida, de las que verá un ejemplo en la siguiente sección. 


### CSS Transitions


Uno de los mas usados tipos de transiciones css:

HTML

```
<div id="example-1">
  <button @click="show = !show">
    Toggle render
  </button>
  <transition name="slide-fade">
    <p v-if="show">hello</p>
  </transition>
</div>
```

JS

```

new Vue({
  el: '#example-1',
  data: {
    show: true
  }
})
```

CSS

```
/* Enter and leave animations can use different */
/* durations and timing functions.              */
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}

```

# Animaciones CSS

Las animaciones CSS se aplican de la misma manera que las transiciones CSS, con la diferencia de que `v-enter` no se elimina inmediatamente después de insertar el elemento, sino en un evento `animationend`.

He aquí un ejemplo, omitiendo las reglas CSS prefijadas en aras de la brevedad: 

HTML

```
<div id="example-2">
  <button @click="show = !show">Toggle show</button>
  <transition name="bounce">
    <p v-if="show">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris facilisis enim libero, at lacinia diam fermentum id. Pellentesque habitant morbi tristique senectus et netus.</p>
  </transition>
</div>
```

JS

```
new Vue({
  el: '#example-2',
  data: {
    show: true
  }
})

```

CSS
```
.bounce-enter-active {
  animation: bounce-in .5s;
}
.bounce-leave-active {
  animation: bounce-in .5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
```

### Clases de transición personalizada

También puede especificar clases de transición personalizadas proporcionando los siguientes atributos: 


    1. enter-class
    2. enter-active-class
    3. enter-to-class (2.1.8+)
    4. leave-class
    5. leave-active-class
    6. leave-to-class (2.1.8+)


Esto invalidará los nombres de clase convencionales. Esto es especialmente útil cuando se desea combinar el sistema de transición de Vue con una biblioteca de animación CSS existente, como Animate. css. 

HTML

```
<link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">

<div id="example-3">
  <button @click="show = !show">
    Toggle render
  </button>
  <transition
    name="custom-classes-transition"
    enter-active-class="animated tada"
    leave-active-class="animated bounceOutRight"
  >
    <p v-if="show">hello</p>
  </transition>
</div>

```

JS

```
new Vue({
  el: '#example-3',
  data: {
    show: true
  }
})

```

### Usando Transiciones y Animaciones Juntas

Vue necesita conectar a los oyentes del evento para saber cuándo ha terminado una transición. Puede ser `transitionend` o `animationend`, dependiendo del tipo de reglas CSS aplicadas. Si sólo utiliza uno u otro, Vue puede detectar automáticamente el tipo correcto.

Sin embargo, en algunos casos es posible que desee tener ambos en el mismo elemento, por ejemplo, tener una animación CSS activada por Vue, junto con un efecto de transición CSS en el flotador. En estos casos, deberá declarar explícitamente el tipo que desea que Vue tenga en un atributo de `type`, con un valor de `animation` o de `transition`. 

### Duraciones de transición explícitas 2.2.0+

En la mayoría de los casos, Vue puede determinar automáticamente cuándo ha terminado la transición. Por defecto, Vue espera el primer evento `transitionend` o `animationend` en el elemento de transición raíz. Sin embargo, esto no siempre puede ser deseado - por ejemplo, podemos tener una secuencia de transición coreografiada donde algunos elementos internos anidados tienen una transición retardada o una duración de transición más larga que el elemento de transición raíz.

En tales casos, puede especificar una duración de transición explícita (en milisegundos) utilizando la duración `duration` prop en el componente `<transición>`: 

HTML

```
<transition :duration="1000">...</transition>
```

HTML

```

<transition :duration="{ enter: 500, leave: 800 }">...</transition>

```

Javascript hooks

HTML
```

<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"

  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
  <!-- ... -->
</transition>


```

JS

```
// ...
methods: {
  // --------
  // ENTERING
  // --------

  beforeEnter: function (el) {
    // ...
  },
  // the done callback is optional when
  // used in combination with CSS
  enter: function (el, done) {
    // ...
    done()
  },
  afterEnter: function (el) {
    // ...
  },
  enterCancelled: function (el) {
    // ...
  },

  // --------
  // LEAVING
  // --------

  beforeLeave: function (el) {
    // ...
  },
  // the done callback is optional when
  // used in combination with CSS
  leave: function (el, done) {
    // ...
    done()
  },
  afterLeave: function (el) {
    // ...
  },
  // leaveCancelled only available with v-show
  leaveCancelled: function (el) {
    // ...
  }
}
```

Estos hooks  se pueden utilizar en combinación con transiciones/animaciones CSS o por sí solos. 


HTML

```
<!--
Velocity works very much like jQuery.animate and is
a great option for JavaScript animations
-->
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>

<div id="example-4">
  <button @click="show = !show">
    Toggle
  </button>
  <transition
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
    v-bind:css="false"
  >
    <p v-if="show">
      Demo
    </p>
  </transition>
</div>

```

JS


```
new Vue({
  el: '#example-4',
  data: {
    show: false
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
    },
    enter: function (el, done) {
      Velocity(el, { opacity: 1, fontSize: '1.4em' }, { duration: 300 })
      Velocity(el, { fontSize: '1em' }, { complete: done })
    },
    leave: function (el, done) {
      Velocity(el, { translateX: '15px', rotateZ: '50deg' }, { duration: 600 })
      Velocity(el, { rotateZ: '100deg' }, { loop: 2 })
      Velocity(el, {
        rotateZ: '45deg',
        translateY: '30px',
        translateX: '30px',
        opacity: 0
      }, { complete: done })
    }
  }
})


```

### Transiciones en el renderizado inicial 

Si tambien se requiere aplicar una transición en el nodo de un render inicial, se puede agregar el atributo `appear`:

HTML

```
<transition appear>
  <!-- ... -->
</transition>
```

Por defecto, utilizará las transiciones especificadas para entrar y salir. Sin embargo, si lo desea, también puede especificar clases CSS personalizadas: 

HTML
```
<transition
  appear
  appear-class="custom-appear-class"
  appear-to-class="custom-appear-to-class" (2.1.8+)
  appear-active-class="custom-appear-active-class"
>
  <!-- ... -->
</transition>
```

Y LOS HOOKS PERSONALIZADOS DE JAVASCRIPT:

HTML

```

<transition
  appear
  v-on:before-appear="customBeforeAppearHook"
  v-on:appear="customAppearHook"
  v-on:after-appear="customAfterAppearHook"
  v-on:appear-cancelled="customAppearCancelledHook"
>
  <!-- ... -->
</transition>

```

En el ejemplo anterior, el atributo appear o el HOOK `v-on:appear` harán que aparezca una transición.

## Transición entre elementos 

Discutimos la transición entre componentes más adelante, pero también se puede hacer la transición entre elementos crudos usando `v-if/v-else`. Una de las transiciones más comunes de dos elementos es entre un contenedor de listas y un mensaje que describe una lista vacía: 

HTML

```
<transition>
  <table v-if="items.length > 0">
    <!-- ... -->
  </table>
  <p v-else>Sorry, no items found.</p>
</transition>
```

Al alternar entre elementos que tienen el mismo nombre de etiqueta, debe decirle a Vue que son elementos distintos dándoles atributos `key` únicos. De lo contrario, el compilador de Vue sólo reemplazará el contenido del elemento por eficiencia. Incluso cuando técnicamente es innecesario, se considera una buena práctica el teclear siempre múltiples ítems dentro de un componente `<transición>`

Por ejemplo:

HTML

```
<transition>
  <button v-if="isEditing" key="save">
    Save
  </button>
  <button v-else key="edit">
    Edit
  </button>
</transition>

```

En estos casos, también puede utilizar el atributo `key` para la transición entre diferentes estados del mismo elemento. En lugar de usar `v-if y v-else`, el ejemplo anterior podría reescribirse como: 

HTML

```
<transition>
  <button v-bind:key="isEditing">
    {{ isEditing ? 'Save' : 'Edit' }}
  </button>
</transition>
```

En realidad, es posible realizar la transición entre cualquier número de elementos, ya sea utilizando múltiples `v-if` o vinculando un único elemento a una propiedad dinámica. Por ejemplo: 

HTML

```
<transition>
  <button v-if="docState === 'saved'" key="saved">
    Edit
  </button>
  <button v-if="docState === 'edited'" key="edited">
    Save
  </button>
  <button v-if="docState === 'editing'" key="editing">
    Cancel
  </button>
</transition>

```

Tambien podemos escribirlo como:

HTML

```
<transition>
  <button v-bind:key="docState">
    {{ buttonMessage }}
  </button>
</transition>


```

JS

```

// ...
computed: {
  buttonMessage: function () {
    switch (this.docState) {
      case 'saved': return 'Edit'
      case 'edited': return 'Save'
      case 'editing': return 'Cancel'
    }
  }
}


```

### Modos de transición


Pero todavía hay un problema. ver el siguiente link de la sección "transition Modes" : [ver ejemplo](https://vuejs.org/v2/guide/transitions.html)

A medida que se realiza la transición entre el botón "on" y el botón "off", se renderizan ambos botones, uno de ellos con transición de salida y el otro con transición de entrada. Este es el comportamiento por defecto de `<transición>` - la entrada y la salida ocurren simultáneamente. 

A veces esto funciona muy bien, como cuando los elementos en transición están absolutamente colocados uno encima del otro o en transición. (ver el enlace anterior)
 

Sin embargo, las transiciones simultáneas de entrada y salida no siempre son deseables, por lo que Vue ofrece algunos modos de transición alternativos:

1. in-out: El nuevo elemento entra primero, luego cuando se completa, el elemento de corriente sale.

2. out-in: El elemento de corriente se transiciona primero hacia afuera, luego cuando se completa, el nuevo elemento se transiciona hacia adentro.

Ahora vamos a actualizar la transición para nuestros botones on/off con out-in: 

HTML

```
<transition name="fade" mode="out-in">
  <!-- ... the buttons ... -->
</transition>

```

## Transición entre Componentes

La transición entre componentes es aún más sencilla: ni siquiera necesitamos el atributo `key`. En su lugar, envolvemos un componente dinámico: 

HTML

```
<transition name="component-fade" mode="out-in">
  <component v-bind:is="view"></component>
</transition>

```

JS

```
new Vue({
  el: '#transition-components-demo',
  data: {
    view: 'v-a'
  },
  components: {
    'v-a': {
      template: '<div>Component A</div>'
    },
    'v-b': {
      template: '<div>Component B</div>'
    }
  }
})

```

CSS

```

.component-fade-enter-active, .component-fade-leave-active {
  transition: opacity .3s ease;
}
.component-fade-enter, .component-fade-leave-to
/* .component-fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

```

## Transiciones List

Hasta ahora hemos manejado las transiciones:

1. Nodos Individuales
2. Multiples nodos donde solamente 1 es rederizado a la vez


Entonces, ¿qué pasa cuando tenemos una lista completa de elementos que queremos renderizar simultáneamente, por ejemplo con v-for? En este caso, usaremos el componente `<transition-group>`. Sin embargo, antes de que nos sumerjamos en un ejemplo, hay algunas cosas que es importante saber acerca de este componente:

A diferencia de `<transición>`, muestra un elemento real: un `<span>` por defecto. Puede cambiar el elemento que se renderiza con el atributo `tag`.
Los modos de transición no están disponibles, porque ya no estamos alternando entre elementos mutuamente excluyentes.
Los elementos del interior siempre deben tener un atributo `key` único. 

## Entrada/salida de la lista de transiciones 

Ahora vamos a sumergirnos en un ejemplo, haciendo la transición entrada y salida usando las mismas clases CSS que hemos usado anteriormente: 

HTML

```
<div id="list-demo">
  <button v-on:click="add">Add</button>
  <button v-on:click="remove">Remove</button>
  <transition-group name="list" tag="p">
    <span v-for="item in items" v-bind:key="item" class="list-item">
      {{ item }}
    </span>
  </transition-group>
</div>

```

JS

```
new Vue({
  el: '#list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9],
    nextNum: 10
  },
  methods: {
    randomIndex: function () {
      return Math.floor(Math.random() * this.items.length)
    },
    add: function () {
      this.items.splice(this.randomIndex(), 0, this.nextNum++)
    },
    remove: function () {
      this.items.splice(this.randomIndex(), 1)
    },
  }
})

```

CSS

```
.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}

```

[ver ejemplo: List Entering/Leaving Transitions](https://vuejs.org/v2/guide/transitions.html)


Hay un problema con este ejemplo, cuando queremos agregar o remover un item, los que lo rodean instantáneamente entran en su nuevo lugar en lugar de hacer una transición sin problemas. Lo arreglaremos más tarde. 

### Transiciones de movimiento de lista 


El componente `<transition-group>` tiene otro truco bajo la manga. No sólo puede animar la entrada y la salida, sino también los cambios de posición. El único concepto nuevo que necesita conocer para utilizar esta función es la adición de la clase `v-move`, que se añade cuando los elementos cambian de posición. Al igual que las otras clases, su prefijo coincidirá con el valor de un atributo `name` proporcionado y también puede especificar manualmente una clase con el atributo de `move-class`.

Esta clase es mayormente útil para especificar el tiempo de transición y la curva de relajación, como se verá a continuación:

HTML

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>

<div id="flip-list-demo" class="demo">
  <button v-on:click="shuffle">Shuffle</button>
  <transition-group name="flip-list" tag="ul">
    <li v-for="item in items" v-bind:key="item">
      {{ item }}
    </li>
  </transition-group>
</div>


```

JS

```
new Vue({
  el: '#flip-list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9]
  },
  methods: {
    shuffle: function () {
      this.items = _.shuffle(this.items)
    }
  }
})

```

CSS


```

.flip-list-move {
  transition: transform 1s;
}

```


VER EJEMPLO: [ver ejemplo: List Move Transitions](https://vuejs.org/v2/guide/transitions.html)

Tambien tenemos el siguiente:

HTML

```

<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>

<div id="list-complete-demo" class="demo">
  <button v-on:click="shuffle">Shuffle</button>
  <button v-on:click="add">Add</button>
  <button v-on:click="remove">Remove</button>
  <transition-group name="list-complete" tag="p">
    <span
      v-for="item in items"
      v-bind:key="item"
      class="list-complete-item"
    >
      {{ item }}
    </span>
  </transition-group>
</div>


```

JS 


```

new Vue({
  el: '#list-complete-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9],
    nextNum: 10
  },
  methods: {
    randomIndex: function () {
      return Math.floor(Math.random() * this.items.length)
    },
    add: function () {
      this.items.splice(this.randomIndex(), 0, this.nextNum++)
    },
    remove: function () {
      this.items.splice(this.randomIndex(), 1)
    },
    shuffle: function () {
      this.items = _.shuffle(this.items)
    }
  }
})


```

CSS


```

.list-complete-item {
  transition: all 1s;
  display: inline-block;
  margin-right: 10px;
}
.list-complete-enter, .list-complete-leave-to
/* .list-complete-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
.list-complete-leave-active {
  position: absolute;
}


```
(VER EN EL LINK ANTERIOR EL EJEMPLO 2)



Una nota importante es que estas transiciones FLIP no funcionan con elementos configurados para `display: inline`. Como alternativa, puede utilizar `display: inline-block` o colocar elementos en un contexto flexible. 



 [Ejemplo: sudoku ](https://vuejs.org/v2/guide/transitions.html)


 ### transiciones de lista escalonada


Al comunicarse con transiciones JavaScript a través de atributos de datos, también es posible escalonar las transiciones en una lista: 

 HTML

 ```
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>

<div id="staggered-list-demo">
  <input v-model="query">
  <transition-group
    name="staggered-fade"
    tag="ul"
    v-bind:css="false"
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <li
      v-for="(item, index) in computedList"
      v-bind:key="item.msg"
      v-bind:data-index="index"
    >{{ item.msg }}</li>
  </transition-group>
</div>

 ```


 JS

  ```

new Vue({
  el: '#staggered-list-demo',
  data: {
    query: '',
    list: [
      { msg: 'Bruce Lee' },
      { msg: 'Jackie Chan' },
      { msg: 'Chuck Norris' },
      { msg: 'Jet Li' },
      { msg: 'Kung Fury' }
    ]
  },
  computed: {
    computedList: function () {
      var vm = this
      return this.list.filter(function (item) {
        return item.msg.toLowerCase().indexOf(vm.query.toLowerCase()) !== -1
      })
    }
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
      el.style.height = 0
    },
    enter: function (el, done) {
      var delay = el.dataset.index * 150
      setTimeout(function () {
        Velocity(
          el,
          { opacity: 1, height: '1.6em' },
          { complete: done }
        )
      }, delay)
    },
    leave: function (el, done) {
      var delay = el.dataset.index * 150
      setTimeout(function () {
        Velocity(
          el,
          { opacity: 0, height: 0 },
          { complete: done }
        )
      }, delay)
    }
  }
})

   ```

  VER EJEMPLO: BUSCADOR [Staggering List Transitions](https://vuejs.org/v2/guide/transitions.html)


  ## Transiciones reusables

  Las transiciones pueden ser reutilizadas a través del sistema de componentes de Vue. Para crear una transición reutilizable, todo lo que tiene que hacer es colocar un componente `<transition>` o `transition-group>` en la raíz, y luego pasar los hijos al componente de transición. 

  Aqui un ejemplo:

  JS
```
Vue.component('my-special-transition', {
  template: '\
    <transition\
      name="very-special-transition"\
      mode="out-in"\
      v-on:before-enter="beforeEnter"\
      v-on:after-enter="afterEnter"\
    >\
      <slot></slot>\
    </transition>\
  ',
  methods: {
    beforeEnter: function (el) {
      // ...
    },
    afterEnter: function (el) {
      // 
    }
  }
})
 ```

Y componentes funcionables son especialmente adecuados para esta tarea: 

JS

 ```


Vue.component('my-special-transition', {
  functional: true,
  render: function (createElement, context) {
    var data = {
      props: {
        name: 'very-special-transition',
        mode: 'out-in'
      },
      on: {
        beforeEnter: function (el) {
          // ...
        },
        afterEnter: function (el) {
          // ...
        }
      }
    }
    return createElement('transition', data, context.children)
  }
})

 ```

 ## Transiciones DINAMICAS

El ejemplo más básico de una transición dinámica vincula el atributo `name` a una propiedad dinámica. 

HTML
 ```
<transition v-bind:name="transitionName">
  <!-- ... -->
</transition>
 ```


 Esto puede ser útil cuando ha definido transiciones/animaciones CSS utilizando las convenciones de clase de transición de Vue y desea cambiar entre ellas.

Sin embargo, en realidad, cualquier atributo de transición puede ser ligado dinámicamente. Y no son sólo atributos. Dado que los HOOK de eventos son métodos, tienen acceso a cualquier dato en el contexto. Esto significa que dependiendo del estado de su componente, sus transiciones JavaScript pueden comportarse de forma diferente. 

HTML

 ```
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>

<div id="dynamic-fade-demo" class="demo">
  Fade In: <input type="range" v-model="fadeInDuration" min="0" v-bind:max="maxFadeDuration">
  Fade Out: <input type="range" v-model="fadeOutDuration" min="0" v-bind:max="maxFadeDuration">
  <transition
    v-bind:css="false"
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <p v-if="show">hello</p>
  </transition>
  <button
    v-if="stop"
    v-on:click="stop = false; show = false"
  >Start animating</button>
  <button
    v-else
    v-on:click="stop = true"
  >Stop it!</button>
</div>

  ```

JS


  ```

new Vue({
  el: '#dynamic-fade-demo',
  data: {
    show: true,
    fadeInDuration: 1000,
    fadeOutDuration: 1000,
    maxFadeDuration: 1500,
    stop: true
  },
  mounted: function () {
    this.show = false
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
    },
    enter: function (el, done) {
      var vm = this
      Velocity(el,
        { opacity: 1 },
        {
          duration: this.fadeInDuration,
          complete: function () {
            done()
            if (!vm.stop) vm.show = false
          }
        }
      )
    },
    leave: function (el, done) {
      var vm = this
      Velocity(el,
        { opacity: 0 },
        {
          duration: this.fadeOutDuration,
          complete: function () {
            done()
            vm.show = true
          }
        }
      )
    }
  }
})
  
  ```

  [VER EJEMPLO Dynamic Transitions](https://vuejs.org/v2/guide/transitions.html)

  ## REUSABILIDAD Y COMPOSICIÓN ## 

  ## MEZCLAS

  ## Básicos

  Las mezclas son una forma flexible de distribuir funcionalidades reutilizables para los componentes Vue. Un objeto mixin puede contener cualquier opción de componente. Cuando un componente utiliza una mezcla, todas las opciones del mixin se "mezclan" en las propias opciones del componente. 

  JS

  ```
  // define a mixin object
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}

// define a component that uses this mixin
var Component = Vue.extend({
  mixins: [myMixin]
})

var component = new Component() // => "hello from mixin!"


  ```

  ## Fusión de opciones(Option Merging)

  JS


```

var mixin = {
  data: function () {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created: function () {
    console.log(this.$data)
    // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})

  ```

  Las funciones de HOOK con el mismo nombre se fusionan en una matriz para que se puedan llamar a todas ellas. Los HOOK de mezcla se llamarán antes que los HOOK propios del componente. 


JS


  ```
var mixin = {
  created: function () {
    console.log('mixin hook called')
  }
}

new Vue({
  mixins: [mixin],
  created: function () {
    console.log('component hook called')
  }
})

// => "mixin hook called"
// => "component hook called"
 
  ```

Las opciones que esperan valores de objeto, por ejemplo métodos, componentes y directivas, se fusionarán en el mismo objeto. Las opciones del componente tendrán prioridad cuando haya claves en conflicto en estos objetos: 

JS

 ```
var mixin = {
  methods: {
    foo: function () {
      console.log('foo')
    },
    conflicting: function () {
      console.log('from mixin')
    }
  }
}

var vm = new Vue({
  mixins: [mixin],
  methods: {
    bar: function () {
      console.log('bar')
    },
    conflicting: function () {
      console.log('from self')
    }
  }
})

vm.foo() // => "foo"
vm.bar() // => "bar"
vm.conflicting() // => "from self"

 ```

Tenga en cuenta que las mismas estrategias de fusión se utilizan en `Vue.extend()`. 

### Global Mixin (Mezclas globales)


También puede aplicar una mezcla globalmente. Usar con precaución! Una vez que aplique un mixin globalmente, afectará a cada instancia de Vue creada posteriormente. Cuando se utiliza correctamente, se puede utilizar para inyectar lógica de procesamiento para opciones personalizadas. 

JS 

```

// inject a handler for `myOption` custom option
Vue.mixin({
  created: function () {
    var myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

new Vue({
  myOption: 'hello!'
})
// => "hello!"

```

## Estrategias de fusión de opciones personalizadas 

Cuando las opciones personalizadas se fusionan, utilizan la estrategia predeterminada que sobrescribe el valor existente. Si desea que una opción personalizada se fusione usando lógica personalizada, necesita adjuntar una función a `Vue. config.optionMergeStrategies`: 

JS

```
Vue.config.optionMergeStrategies.myOption = function (toVal, fromVal) {
  // return mergedVal
}
```

Para la mayoría de las opciones basadas en objetos, puede utilizar la misma estrategia que utilizan los métodos: 

JS

```
var strategies = Vue.config.optionMergeStrategies
strategies.myOption = strategies.methods

```

Un ejemplo más avanzado se puede encontrar en la estrategia de fusión 1.x de Vuex: 

JS

```
const merge = Vue.config.optionMergeStrategies.computed
Vue.config.optionMergeStrategies.vuex = function (toVal, fromVal) {
  if (!toVal) return fromVal
  if (!fromVal) return toVal
  return {
    getters: merge(toVal.getters, fromVal.getters),
    state: merge(toVal.state, fromVal.state),
    actions: merge(toVal.actions, fromVal.actions)
  }
}

```

## Directivas personalizadas

Cuando se carga la página, ese elemento gana enfoque (nota: `autofocus` no funciona en Safari móvil). De hecho, si no ha hecho clic en nada más desde que visitó esta página, la entrada de arriba debe ser enfocada ahora. Ahora construyamos la directiva que logre esto: 

JS

```
// Register a global custom directive called `v-focus`
Vue.directive('focus', {
  // When the bound element is inserted into the DOM...
  inserted: function (el) {
    // Focus the element
    el.focus()
  }
})

```

Si desea registrar una directiva localmente, los componentes también aceptan la opción de `directives`:

JS

```
directives: {
  focus: {
    // directive definition
    inserted: function (el) {
      el.focus()
    }
  }
}

```

Luego, en una plantilla, puede utilizar el nuevo atributo v-focus en cualquier elemento, como este: 

HTML

```
<input v-focus>

```

## FUNCIONES HOOK


Un objeto de definición de directiva puede proporcionar varias funciones de HOOK (todas opcionales):

1. bind: se llama sólo una vez, cuando la directiva está ligada por primera vez al elemento. Aquí es donde se puede realizar el trabajo de preparación de una sola vez.

2. inserted: llamado cuando el elemento encuadernado ha sido insertado en su nodo padre (esto sólo garantiza la presencia del nodo padre, no necesariamente en el documento).

3. update: llamado después de que el VNode del componente que contiene se haya actualizado, pero posiblemente antes de que sus hijos lo hayan hecho. El valor de la directiva puede o no haber cambiado, pero puede omitir actualizaciones innecesarias comparando los valores actuales y antiguos de la encuadernación (ver más adelante sobre los argumentos hook). 


4. ComponentUpdated: llamado después de que el VNode del componente que contiene y los VNodes de sus hijos se hayan actualizado.

5. unbind: se llama sólo una vez, cuando la directiva no está ligada al elemento.

Exploraremos los argumentos pasados a estos hook (es decir, el, binding, vnode y oldVnode) en la siguiente sección. 

## Argumentos sobre la directiva hook

Los hook de la directiva se pasan estos argumentos:

1. el: El elemento al que está obligada la directiva. Esto se puede utilizar para manipular directamente el DOM.
2. binding: Un objeto que contiene las siguientes propiedades.
- name: El nombre de la directiva, sin el prefijo v-.
- valor: El valor pasado a la directiva. Por ejemplo, en - v-my-directive="1 + 1";, el valor sería 2.
- oldValue: El valor anterior, sólo disponible en `update` y `componentUpdated`. Está disponible independientemente de si el valor ha cambiado o no.
- expression: La expresión del binding como un string. Por ejemplo, en `v-my-directive="1 + 1"`, la expresión sería "1 + 1".
- arg: El argumento pasó a la directiva, si la hubiera. Por ejemplo, en `v-mi-directiva:foo`, el arg sería "foo".
- modificadores: Un objeto que contiene modificadores, si los hay. Por ejemplo, en `v-my-directive.foo.bar`, el objeto modificador sería `{ foo: true, bar: true }`.
3. vnode: El nodo virtual producido por el compilador de Vue. Consulte la API de VNode para obtener más detalles.
4. oldVnode: El nodo virtual anterior, sólo disponible en los hook `update` y `componentUpdated`.

Aparte de `el`, debe tratar estos argumentos como de sólo lectura y nunca modificarlos. Si necesita compartir información entre hook, se recomienda hacerlo a través del conjunto de datos del elemento. 

Ejemplo:

HTML
```
<div id="hook-arguments-example" v-demo:foo.a.b="message"></div>
```
JS

```
Vue.directive('demo', {
  bind: function (el, binding, vnode) {
    var s = JSON.stringify
    el.innerHTML =
      'name: '       + s(binding.name) + '<br>' +
      'value: '      + s(binding.value) + '<br>' +
      'expression: ' + s(binding.expression) + '<br>' +
      'argument: '   + s(binding.arg) + '<br>' +
      'modifiers: '  + s(binding.modifiers) + '<br>' +
      'vnode keys: ' + Object.keys(vnode).join(', ')
  }
})

new Vue({
  el: '#hook-arguments-example',
  data: {
    message: 'hello!'
  }
})
```
Un ejemplo de una directiva personalizada que utiliza un argumento dinámico: 

HTML
```
<div id="app">
  <p>Scroll down the page</p>
  <p v-tack:left="[dynamicleft]">I’ll now be offset from the left instead of the top</p>
</div>
```

JS
```

Vue.directive('tack', {
  bind(el, binding, vnode) {
    el.style.position = 'fixed';
    const s = (binding.arg == 'left' ? 'left' : 'top');
    el.style[s] = binding.value + 'px';
  }
})

// start app
new Vue({
  el: '#app',
  data() {
    return {
      dynamicleft: 500
    }
  }
})
```

## Atajos de funciones

En muchos casos, es posible que desee el mismo comportamiento en `bind` y `update`, pero no se preocupe por los otros hook. Por ejemplo: 

JS
```
Vue.directive('color-swatch', function (el, binding) {
  el.style.backgroundColor = binding.value
})

```

## Objetos literales

Si su directiva necesita múltiples valores, también puede pasar un literal de objeto JavaScript. Recuerde, las directivas pueden tomar cualquier expresión JavaScript válida. 

HTML

```
<div v-demo="{ color: 'white', text: 'hello!' }"></div>

```

JS

```
Vue.directive('demo', function (el, binding) {
  console.log(binding.value.color) // => "white"
  console.log(binding.value.text)  // => "hello!"
})

```

## Funciones renderizados y JSX

### Basic


Vue recomienda usar plantillas para construir su HTML en la gran mayoría de los casos. Sin embargo, hay situaciones en las que realmente se necesita toda la potencia programática de JavaScript. Ahí es donde puede utilizar la función de `render`, una alternativa más cercana al compilador a las plantillas.

Veamos un ejemplo sencillo en el que una función de render sería práctica. Supongamos que desea generar encabezados anclados:

HTML
```

<h1>
  <a name="hello-world" href="#hello-world">
    Hello world!
  </a>
</h1>
```

Para el código HTML anterior, decide que desea esta interfaz de componente:

HTML

```
<anchored-heading :level="1">Hello world!</anchored-heading

```

Cuando comienzas con un componente que solo genera un encabezado basado en la prop de `level`, rápidamente llegas a esto:

HTML
```
<script type="text/x-template" id="anchored-heading-template">
  <h1 v-if="level === 1">
    <slot></slot>
  </h1>
  <h2 v-else-if="level === 2">
    <slot></slot>
  </h2>
  <h3 v-else-if="level === 3">
    <slot></slot>
  </h3>
  <h4 v-else-if="level === 4">
    <slot></slot>
  </h4>
  <h5 v-else-if="level === 5">
    <slot></slot>
  </h5>
  <h6 v-else-if="level === 6">
    <slot></slot>
  </h6>
</script>

```

JS

```

Vue.component('anchored-heading', {
  template: '#anchored-heading-template',
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})


```

Esa plantilla no estaria bien. No sólo es verbose, sino que estamos duplicando `<slot></slot>` para cada nivel de encabezado y tendremos que hacer lo mismo cuando añadamos el elemento ancla.

Aunque las plantillas funcionan bien para la mayoría de los componentes, está claro que ésta no es una de ellas. Así que vamos a intentar reescribirlo con una función de `render`: 

JS
```

Vue.component('anchored-heading', {
  render: function (createElement) {
    return createElement(
      'h' + this.level,   // tag name
      this.$slots.default // array of children
    )
  },
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})

```



Mucho más sencillo! Más o menos. El código es más corto, pero también requiere una mayor familiaridad con las propiedades de la instancia de Vue. En este caso, tienes que saber que cuando pasas hijos sin una directiva de v-slot a un componente, como en el caso de Hello world! dentro de `anchored-heading`, esos hijos se almacenan en la instancia del componente en `$slots.default`. 


## Nodos, Árboles y el DOM Virtual 

Antes de sumergirnos en las funciones de render, es importante saber un poco sobre el funcionamiento de los navegadores. Tomemos como ejemplo este HTML: 

HTML
```

<div>
  <h1>My title</h1>
  Some text content
  <!-- TODO: Add tagline  -->
</div>


```


Cuando un navegador lea este código, construye un árbol de "nodos DOM" para ayudarlo a llevar un registro de todo, de la misma manera que usted podría construir un árbol genealógico para llevar un registro de su familia extendida.

El árbol de nodos DOM para el HTML anterior tiene el siguiente aspecto:

[ver nodes,trees, and the virtual DOM](https://vuejs.org/v2/guide/render-function.html)


Cada elemento es un nodo. Cada trozo de texto es un nodo. Incluso los comentarios son nodos! Un nodo es sólo una parte de la página. Y como en un árbol genealógico, cada nodo puede tener hijos (es decir, cada pieza puede contener otras piezas).

Actualizar todos estos nodos eficientemente puede ser difícil, pero afortunadamente, nunca tienes que hacerlo manualmente. En su lugar, usted le dice a Vue qué HTML quiere en la página, en una plantilla:

HTML 

```
<h1>{{ blogTitle }}</h1>
```

O una función render:

JS

```

render: function (createElement) {
  return createElement('h1', this.blogTitle)
}

```

Y en ambos casos, Vue mantiene automáticamente la página actualizada, incluso cuando `blogTitle` cambia. 

### El virtual DOM

Vue logra esto construyendo un DOM virtual para hacer un seguimiento de los cambios que necesita hacer en el DOM real. Echando un vistazo más de cerca a esta línea: 

JS

```
return createElement('h1', this.blogTitle)

```

¿Qué es lo que está devolviendo `createElement`? No es exactamente un elemento DOM real. Tal vez podría llamarse `createNodeDescription`, ya que contiene información que describe a Vue qué tipo de nodo debe representar en la página, incluyendo descripciones de cualquier nodo hijo. Llamamos a esta descripción de nodo un "nodo virtual";, normalmente abreviado como VNode. "Virtual DOM"; es lo que llamamos el árbol entero de VNodes, construido por un árbol de componentes Vue. 


## Argumentos createElement


Lo siguiente con lo que tendrá que familiarizarse es con el uso de las funciones de plantilla en la función createElement. Aquí están los argumentos que createElement acepta:  

JS

```

// @returns {VNode}
createElement(
  // {String | Object | Function}
  // An HTML tag name, component options, or async
  // function resolving to one of these. Required.
  'div',

  // {Object}
  // A data object corresponding to the attributes
  // you would use in a template. Optional.
  {
    // (see details in the next section below)
  },

  // {String | Array}
  // Children VNodes, built using `createElement()`,
  // or using strings to get 'text VNodes'. Optional.
  [
    'Some text comes first.',
    createElement('h1', 'A headline'),
    createElement(MyComponent, {
      props: {
        someProp: 'foobar'
      }
    })
  ]
)

```

### El objeto de datos en profundidad 

Una cosa a tener en cuenta: al igual que `v-bind:class` y `v-bind:style` tienen un tratamiento especial en las plantillas, tienen sus propios campos de nivel superior en los objetos de datos de VNode. Este objeto también le permite enlazar atributos HTML normales así como propiedades DOM como innerHTML (esto reemplazaría a la directiva v-html): 

JS

```
{
  // Same API as `v-bind:class`, accepting either
  // a string, object, or array of strings and objects.
  class: {
    foo: true,
    bar: false
  },
  // Same API as `v-bind:style`, accepting either
  // a string, object, or array of objects.
  style: {
    color: 'red',
    fontSize: '14px'
  },
  // Normal HTML attributes
  attrs: {
    id: 'foo'
  },
  // Component props
  props: {
    myProp: 'bar'
  },
  // DOM properties
  domProps: {
    innerHTML: 'baz'
  },
  // Event handlers are nested under `on`, though
  // modifiers such as in `v-on:keyup.enter` are not
  // supported. You'll have to manually check the
  // keyCode in the handler instead.
  on: {
    click: this.clickHandler
  },
  // For components only. Allows you to listen to
  // native events, rather than events emitted from
  // the component using `vm.$emit`.
  nativeOn: {
    click: this.nativeClickHandler
  },
  // Custom directives. Note that the `binding`'s
  // `oldValue` cannot be set, as Vue keeps track
  // of it for you.
  directives: [
    {
      name: 'my-custom-directive',
      value: '2',
      expression: '1 + 1',
      arg: 'foo',
      modifiers: {
        bar: true
      }
    }
  ],
  // Scoped slots in the form of
  // { name: props => VNode | Array<VNode> }
  scopedSlots: {
    default: props => createElement('span', props.text)
  },
  // The name of the slot, if this component is the
  // child of another component
  slot: 'name-of-slot',
  // Other special top-level properties
  key: 'myKey',
  ref: 'myRef',
  // If you are applying the same ref name to multiple
  // elements in the render function. This will make `$refs.myRef` become an
  // array
  refInFor: true
}

```

### Ejemplos completados

Con este conocimiento, ahora podemos terminar el componente que empezamos:

JS
```

var getChildrenTextContent = function (children) {
  return children.map(function (node) {
    return node.children
      ? getChildrenTextContent(node.children)
      : node.text
  }).join('')
}

Vue.component('anchored-heading', {
  render: function (createElement) {
    // create kebab-case id
    var headingId = getChildrenTextContent(this.$slots.default)
      .toLowerCase()
      .replace(/\W+/g, '-')
      .replace(/(^-|-$)/g, '')

    return createElement(
      'h' + this.level,
      [
        createElement('a', {
          attrs: {
            name: headingId,
            href: '#' + headingId
          }
        }, this.$slots.default)
      ]
    )
  },
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})

```

### Limitaciones

Los VNode deben ser únicos:

JS

```
render: function (createElement) {
  var myParagraphVNode = createElement('p', 'hi')
  return createElement('div', [
    // Yikes - duplicate VNodes!
    myParagraphVNode, myParagraphVNode
  ])
}
```

Si realmente desea duplicar el mismo elemento/componente muchas veces, puede hacerlo con una función de fábrica. Por ejemplo, la siguiente función de render es una forma perfectamente válida de renderizar 20 párrafos idénticos: 

JS

```
render: function (createElement) {
  return createElement('div',
    Array.apply(null, { length: 20 }).map(function () {
      return createElement('p', 'hi')
    })
  )
}

```

## Sustitución de las características de la plantilla por JavaScript simple 

### v-if and v-for

Donde quiera que algo se pueda lograr fácilmente en JavaScript simple, las funciones de renderizado de Vue no proporcionan una alternativa patentada. Por ejemplo, en una plantilla que utiliza `v-if` y `v-for`: 

HTML

```
<ul v-if="items.length">
  <li v-for="item in items">{{ item.name }}</li>
</ul>
<p v-else>No items found.</p>

```
Esto puede ser reescrito con JavaScript's `if/else` y `map` en una función de render:

JS

```
props: ['items'],
render: function (createElement) {
  if (this.items.length) {
    return createElement('ul', this.items.map(function (item) {
      return createElement('li', item.name)
    }))
  } else {
    return createElement('p', 'No items found.')
  }
}

```

### v-model

No hay una contraparte directa del `v-model` en las funciones de renderizado - tendrá que implementar la lógica usted mismo: 

JS

```

props: ['value'],
render: function (createElement) {
  var self = this
  return createElement('input', {
    domProps: {
      value: self.value
    },
    on: {
      input: function (event) {
        self.$emit('input', event.target.value)
      }
    }
  })
}
```


### Eventos y modificadores Key 


Para el modificador de eventos `.passive`, `.capture` y `.once`. Vue ofrece prefijos que suelen ser usados con `on`:

```
.passive       -> &
.capture       -> !
.once          -> ~
.capture.once or 
.once.capture  -> ~!

```

Por ejemplo:

```
on: {
  '!click': this.doThisInCapturingMode,
  '~keyup': this.doThisOnce,
  '~!mouseover': this.doThisOnceInCapturingMode
}

```

Para todos los demás eventos y modificadores key, no se necesita ningún prefijo propietario, ya que puede utilizar métodos de eventos en el handler:

```
.stop     -> 	event.stopPropagation()
.prevent 	-> event.preventDefault()
.self 	  -> if (event.target !== event.currentTarget) return
Keys:
.enter, .13 -> if (event.keyCode !== 13) return (change 13 to another key code for other key modifiers)
Modifiers Keys:
.ctrl, .alt, .shift, .meta 	-> if (!event.ctrlKey) return (change ctrlKey to altKey, shiftKey, or metaKey, respectively)

```

Ejemplo:


```
on: {
  keyup: function (event) {
    // Abort if the element emitting the event is not
    // the element the event is bound to
    if (event.target !== event.currentTarget) return
    // Abort if the key that went up is not the enter
    // key (13) and the shift key was not held down
    // at the same time
    if (!event.shiftKey || event.keyCode !== 13) return
    // Stop event propagation
    event.stopPropagation()
    // Prevent the default keyup handler for this element
    event.preventDefault()
    // ...
  }
}

```

# Slots

Puede acceder al contenido estático de slot como Arrays of VNodes desde `this.$slots`: 

JS
```
render: function (createElement) {
  // `<div><slot></slot></div>`
  return createElement('div', this.$slots.default)
}


```

Y acceder a SLOTS de alcance como funciones que retornan a  VNodes desde `this.$scopedSlots`:

JS

```
props: ['message'],
render: function (createElement) {
  // `<div><slot :text="message"></slot></div>`
  return createElement('div', [
    this.$scopedSlots.default({
      text: this.message
    })
  ])
}

```

Para pasar slots scoped a un componente hijo usando funciones de render, utilice el campo scopedSlots en los datos de VNode: 

JS

```

render: function (createElement) {
  return createElement('div', [
    createElement('child', {
      // pass `scopedSlots` in the data object
      // in the form of { name: props => VNode | Array<VNode> }
      scopedSlots: {
        default: function (props) {
          return createElement('span', props.text)
        }
      }
    })
  ])
}
```

## JSX

Si estás escribiendo muchas funciones de render, puede resultar doloroso escribir algo como esto: 

JS
```

createElement(
  'anchored-heading', {
    props: {
      level: 1
    }
  }, [
    createElement('span', 'Hello'),
    ' world!'
  ]
)

```

Especialmente cuando la versión de la plantilla es tan simple en comparación:

HTML

```

<anchored-heading :level="1">
  <span>Hello</span> world!
</anchored-heading>
```

Por eso hay un plugin de Babel para usar JSX con Vue, que nos devuelve a una sintaxis más cercana a las plantillas: 

JS
```
import AnchoredHeading from './AnchoredHeading.vue'

new Vue({
  el: '#demo',
  render: function (h) {
    return (
      <AnchoredHeading level={1}>
        <span>Hello</span> world!
      </AnchoredHeading>
    )
  }
})


```

## Componentes funcionales

El componente de encabezado anclado que creamos anteriormente es relativamente simple. No maneja ningún estado, mira cualquier estado que se le haya pasado, y no tiene métodos de ciclo de vida. En realidad, es sólo una función con algunos accesorios.

En casos como este, podemos marcar los componentes como `functional`, lo que significa que son sin estado (sin datos reactivos) y sin instinto (sin `this` contexto). Un componente funcional tiene este aspecto: 

JS

```

Vue.component('my-component', {
  functional: true,
  // Props are optional
  props: {
    // ...
  },
  // To compensate for the lack of an instance,
  // we are now provided a 2nd context argument.
  render: function (createElement, context) {
    // ...
  }
})

```

En 2. 5. 0+, si está utilizando componentes de un solo archivo, los componentes funcionales basados en plantillas se pueden declarar con: 

HTML

```
<template functional>
</template>
```

Todo lo que el componente necesita se pasa a través del `context`, que es un objeto que contiene:

1. props: Un objeto de los props proporcionados
2. children: Una selección de los children VNode
3. slots: Una función que devuelve un objeto de slots
4. scopedSlots: (2. 6. 0+) Un objeto que expone slot de alcance pasado. También expone los slots normales como funciones.
5. data: El objeto de datos completo, pasado al componente como segundo argumento de createElement
6. parent: Una referencia al componente padre
7. listeners: (2. 3. 0+) Un objeto que contiene oyentes de eventos registrados por los padres. Este es un alias de data.on
8. inyecciones: (2. 3. 0+) si utiliza la opción de inject, ésta contendrá inyecciones resueltas.

Después de añadir `funcional: true`, actualizar la función de render de nuestro componente de encabezado anclado requeriría añadir el argumento `context`, actualizar `this.$slots.default` a `context.children`, y luego actualizar `this.nivel` a `context.props.level`.

Dado que los componentes funcionales son sólo funciones, es mucho más barato renderizarlos.

También son muy útiles como componentes de envoltura. Por ejemplo, cuando sea necesario:

1. Elegir programáticamente uno de los otros componentes en los que delegar
2. Manipular hijos, objetos de utilería o datos antes de pasarlos a un componente hijo.

He aquí un ejemplo de un componente de `smart-list` que se delega a componentes más específicos, dependiendo de los accesorios que se le pasen: 

JS

```
var EmptyList = { /* ... */ }
var TableList = { /* ... */ }
var OrderedList = { /* ... */ }
var UnorderedList = { /* ... */ }

Vue.component('smart-list', {
  functional: true,
  props: {
    items: {
      type: Array,
      required: true
    },
    isOrdered: Boolean
  },
  render: function (createElement, context) {
    function appropriateListComponent () {
      var items = context.props.items

      if (items.length === 0)           return EmptyList
      if (typeof items[0] === 'object') return TableList
      if (context.props.isOrdered)      return OrderedList

      return UnorderedList
    }

    return createElement(
      appropriateListComponent(),
      context.data,
      context.children
    )
  }
})


```

### Transmisión de atributos y eventos a los elementos/componentes Hijos 

En componentes normales, los atributos no definidos como props se añaden automáticamente al elemento raíz del componente, reemplazando o fusionándose inteligentemente con cualquier atributo existente del mismo nombre.

Sin embargo, los componentes funcionales requieren que defina explícitamente este comportamiento: 

JS

```
Vue.component('my-functional-button', {
  functional: true,
  render: function (createElement, context) {
    // Transparently pass any attributes, event listeners, children, etc.
    return createElement('button', context.data, context.children)
  }
})

```

Al pasar `context.data` como segundo argumento para `createElement`, estamos pasando todos los atributos o eventos de los oyentes utilizados en el `my-functional-button`. Es tan transparente, de hecho, que los eventos ni siquiera requieren el modificador `.native`.

Si está utilizando componentes funcionales basados en plantillas, también tendrá que añadir manualmente atributos y oyentes. Como tenemos acceso a los contenidos de contexto individuales, podemos usar `data.attrs` para pasar cualquier atributo HTML y los `listeners` (el alias de `data.on`) para pasar a los oyentes de cualquier evento. 

HTML

```

<template functional>
  <button
    class="btn btn-primary"
    v-bind="data.attrs"
    v-on="listeners"
  >
    <slot/>
  </button>
</template>

```

### slots() vs children


Usted se preguntará por qué necesitamos tanto slots() como children. ¿No sería `slots().default` lo mismo que `children`? En algunos casos, sí, pero ¿qué pasa si tiene un componente funcional con los siguientes children?

HTML

```
<my-functional-component>
  <p v-slot:foo>
    first
  </p>
  <p>second</p>
</my-functional-component>

```

Para este componente, los hijos le darán ambos párrafos, `slot().default` le dará sólo el segundo, y `slots().foo` le dará sólo el primero. Por lo tanto, tener tanto hijos como slots() le permite elegir si este componente sabe de un sistema de slots o quizás delegar esa responsabilidad a otro componente pasando a los hijos. 

## Plantillas de Compilación


Puede que le interese saber que las plantillas de Vue se compilan para renderizar funciones. Este es un detalle de implementación que normalmente no necesita conocer, pero si desea ver cómo se compilan las características específicas de las plantillas, puede que le resulte interesante. Abajo hay una pequeña demostración usando `Vue.compile` para compilar una cadena de plantillas en vivo: 

[ver ejemplo en TEMPLATE COMPILATION](https://vuejs.org/v2/guide/render-function.html)

## Plugins

Los plugins usualmente agregan niveles globales a "vue". 
No hay un alcance estrictamente definido para un plugin - típicamente hay varios tipos de plugins: 

1. Agregar algunos métodos o propiedades globales. Por ejemplo: `.vue-custom-element`

2. Añadir uno o más activos globales: directivas/filtros/transiciones, etc. Por ejemplo: `.vue-touch`

3. Agregue algunas opciones de componentes por mezcla global. Por ejemplo, `vue-router`

4. Añada algunos métodos de instancia de Vue adjuntándolos a Vue.prototype.

5. Una librería que proporciona una API propia, mientras que al mismo tiempo inyecta alguna combinación de las anteriores, por ejemplo: `vue-router` 

## Usando un plugin

Usando por la llamada del metodo global `Vue.use()`. 

Eso tiene que hacerse antes de iniciar su aplicación llamado `new Vue()` :

JS

```
// calls `MyPlugin.install(Vue)`
Vue.use(MyPlugin)

new Vue({
  //... options
})

```

Se puede opcionalmente pasar algunas opciones: 

JS

```
Vue.use(MyPlugin, { someOption: true })

```

`Vue.use` automáticamente le impide usar el mismo plugin más de una vez, por lo que si lo llama varias veces en el mismo plugin, éste se instalará sólo una vez.

Algunos plugins proporcionados por los plugins oficiales de `Vue.js`, como `vue-router`, llaman automáticamente a `Vue.use()` si `Vue` está disponible como una variable global. Sin embargo, en un entorno de módulo como CommonJS, siempre es necesario llamar a `Vue.use()` explícitamente: 

JS

```
// When using CommonJS via Browserify or Webpack
var Vue = require('vue')
var VueRouter = require('vue-router')

// Don't forget to call this
Vue.use(VueRouter)

```




## Escribiendo un plugin

Un plugin `Vue.js` debería exponer un método de `install`. El método se llamará con el constructor de `Vue` como primer argumento, junto con las opciones posibles: 

JS

```

MyPlugin.install = function (Vue, options) {
  // 1. add global method or property
  Vue.myGlobalMethod = function () {
    // some logic ...
  }

  // 2. add a global asset
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // some logic ...
    }
    ...
  })

  // 3. inject some component options
  Vue.mixin({
    created: function () {
      // some logic ...
    }
    ...
  })

  // 4. add an instance method
  Vue.prototype.$myMethod = function (methodOptions) {
    // some logic ...
  }
}

```

### Filtros


`Vue.js` le permite definir filtros que se pueden utilizar para aplicar el formato de texto común. Los filtros se pueden usar en dos lugares: interpolaciones de bigote y expresiones `v-bind` (estas últimas soportadas en 2.1.0+). Los filtros deben añadirse al final de la expresión JavaScript, señalados con el símbolo "pipe"

```
<!-- in mustaches -->
{{ message | capitalize }}

<!-- in v-bind -->
<div v-bind:id="rawId | formatId"></div>

```

Puede definir filtros locales en las opciones de un componente: 

JS

```

filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}

```

O definir globalmente un filtro antes de la creación de la instancia del Vue:


JS

```
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})

new Vue({
  // ...
})


```

Este ejemplo es un filtro de nuestro `capitalize` siendo usado:

[ver ejemplo filters](https://vuejs.org/v2/guide/filters.html)



La función de filtro siempre recibe el valor de la expresión (el resultado de la cadena anterior) como su primer argumento. En el ejemplo anterior, la función de filtro `capitalizer` recibirá el valor de mensaje como argumento. 

Los filtros se pueden encadenar

HTML

```
{{ message | filterA | filterB }}

```

En este caso, `filterA`, definido con un argumento simple, recibirá el valor de `message`, y luego la función del `filterB` será llamada con el resultado de paso de `filterA` dentro del argumento del `filterB` 


Los filtros son funciones de javascript, ademas ellos pueden tomar argumentos:

HTML

```
{{ message | filterA('arg1', arg2) }}

```

Aquí el `filtroA` se define como una función que toma tres argumentos. El valor del `message` se pasará al primer argumento. El string 'arg1'; pasará al `filtroA` como segundo argumento, y el valor de la expresión `arg2` será evaluado y transmitido como tercer argumento. 

## Herramientas ##

### Componentes de archivos simples



En muchos proyectos de Vue, los componentes globales se definirán utilizando `Vue.component`, seguido de un `new Vue ({ el: '#container' })` para dirigirse a un elemento contenedor en el cuerpo de cada página.

Esto puede funcionar muy bien para proyectos pequeños y medianos, donde JavaScript sólo se utiliza para mejorar ciertas vistas. Sin embargo, en proyectos más complejos, o cuando su interfaz está totalmente impulsado por JavaScript, estas desventajas se hacen evidentes:

1. Las definiciones globales fuerzan la creación de nombres únicos para cada componente
2. Las plantillas de string carecen de resaltado de sintaxis y requieren barras oblicuas para HTML multilínea.
3. La ausencia de soporte para CSS significa que mientras que HTML y JavaScript están modulados en componentes, CSS se omite visiblemente.
4. Ningún paso de compilación nos restringe a HTML y ES5 JavaScript, en lugar de preprocesadores como Pug (antes Jade) y Babel.

Todo esto se resuelve con componentes de un solo archivo con extensión `.vue`, que son posibles gracias a herramientas de construcción como Webpack o Browserify.

[He aquí un ejemplo de un archivo que llamaremos Hello. vue:](https://vuejs.org/v2/guide/single-file-components.html)





HTML

```
<!-- my-component.vue -->
<template>
  <div>This will be pre-compiled</div>
</template>
<script src="./my-component.js"></script>
<style src="./my-component.css"></style>

```


### ¿Qué pasa con la separación de asuntos?

Una cosa importante a tener en cuenta es que la separación de asuntos no es igual a la separación de tipos de archivos. En el desarrollo moderno de la interfaz de usuario, hemos encontrado que en lugar de dividir la base de código en tres grandes capas que se entrelazan entre sí, tiene mucho más sentido dividirlas en componentes sueltos y componerlas. Dentro de un componente, su plantilla, lógica y estilos están intrínsecamente acoplados, y la colocación de los mismos hace que el componente sea más cohesivo y mantenible.

Incluso si no le gusta la idea de los componentes de un solo archivo, puede aprovechar sus funciones de recarga en caliente y precompilación separando su JavaScript y CSS en archivos separados:

HTML

```
<!-- my-component.vue -->
<template>
  <div>This will be pre-compiled</div>
</template>
<script src="./my-component.js"></script>
<style src="./my-component.css"></style>

```

## Pruebas de unidad


### Simples afirmaciones

No se tiene que hacer nada especial en sus componentes para hacerlos comprobables:

HTML

```
<template>
  <span>{{ message }}</span>
</template>

<script>
  export default {
    data () {
      return {
        message: 'hello!'
      }
    },
    created () {
      this.message = 'bye!'
    }
  }
</script>

```


JS 


```
// Import Vue and the component being tested
import Vue from 'vue'
import MyComponent from 'path/to/MyComponent.vue'

// Here are some Jasmine 2.0 tests, though you can
// use any test runner / assertion library combo you prefer
describe('MyComponent', () => {
  // Inspect the raw component options
  it('has a created hook', () => {
    expect(typeof MyComponent.created).toBe('function')
  })

  // Evaluate the results of functions in
  // the raw component options
  it('sets the correct default data', () => {
    expect(typeof MyComponent.data).toBe('function')
    const defaultData = MyComponent.data()
    expect(defaultData.message).toBe('hello!')
  })

  // Inspect the component instance on mount
  it('correctly sets the message when created', () => {
    const vm = new Vue(MyComponent).$mount()
    expect(vm.message).toBe('bye!')
  })

  // Mount an instance and inspect the render output
  it('renders the correct message', () => {
    const Constructor = Vue.extend(MyComponent)
    const vm = new Constructor().$mount()
    expect(vm.$el.textContent).toBe('bye!')
  })
})

```



### Escribiendo componentes de prueba

La salida de render de un componente está determinada principalmente por los props que recibe. Si la salida de render de un componente depende únicamente de sus props, se convierte en algo sencillo de probar, similar a afirmar el valor de retorno de una función con argumentos diferentes. Tomemos un ejemplo simplificado: 

HTML

```
<template>
  <p>{{ msg }}</p>
</template>

<script>
  export default {
    props: ['msg']
  }
</script>


```

Con diferentes props usando la opción `propsData`:

JS

```

import Vue from 'vue'
import MyComponent from './MyComponent.vue'

// helper function that mounts and returns the rendered text
function getRenderedText (Component, propsData) {
  const Constructor = Vue.extend(Component)
  const vm = new Constructor({ propsData: propsData }).$mount()
  return vm.$el.textContent
}

describe('MyComponent', () => {
  it('renders correctly with different props', () => {
    expect(getRenderedText(MyComponent, {
      msg: 'Hello'
    })).toBe('Hello')

    expect(getRenderedText(MyComponent, {
      msg: 'Bye'
    })).toBe('Bye')
  })
})

```

### Actualizaciones asincronas afirmadas

Las actualizaciones en el DOM debera realizarse dentro de un callback `Vue.nextTick`

JS
```

// Inspect the generated HTML after a state update
it('updates the rendered message when vm.message updates', done => {
  const vm = new Vue(MyComponent).$mount()
  vm.message = 'foo'

  // wait a "tick" after state change before asserting DOM updates
  Vue.nextTick(() => {
    expect(vm.$el.textContent).toBe('foo')
    done()
  })
})

```

## Soporte TypeScript

### Declaraciones oficial en paquetes NPM

Un sistema tipo estatico puede ayudar a prevenir muchos errores de corridas, especialmente como aplicaciones en crecimiento.

### Configuraciones recomendadas

JS

```
// tsconfig.json
{
  "compilerOptions": {
    // this aligns with Vue's browser support
    "target": "es5",
    // this enables stricter inference for data properties on `this`
    "strict": true,
    // if using webpack 2+ or rollup, to leverage tree shaking:
    "module": "es2015",
    "moduleResolution": "node"
  }
}


```


Se debe incluir `strict: true `(ó al menos `noImplicitThis: true` la cual es una parte de la bandera `strict`) para aprovechar la comprobación del `this` en los métodos de componentes, de lo contrario siempre se tratará como de cualquier tipo. 

### Herramientas de desarrollo

### Creación del proyecto

Podemos generar nuevos proyectos con el uso de TypeScript:


Shell
```
# 1. Install Vue CLI, if it's not already installed
npm install --global @vue/cli

# 2. Create a new project, then choose the "Manually select features" option
vue create my-project-name
```


### Editor de soportes

Para desarrolar aplicaciones Vue con typescript, se recomienda usar Visual Studio Code, la cual provee grandes suportes para Type Script.

### Usos básicos

Se necesita definir componentes con `Vue.component` ó `Vue.extend`:

JS

```
import Vue from 'vue'

const Component = Vue.extend({
  // type inference enabled
})

const Component = {
  // this will NOT have type inference,
  // because TypeScript can't tell this is options for a Vue component.
}

```

### Clases y estilos de Vue Components

Si se requiere que la API este basada en una clase, se puede realizar el siguiente decorador de vue-class-component

JS

```
import Vue from 'vue'
import Component from 'vue-class-component'

// The @Component decorator indicates the class is a Vue component
@Component({
  // All component options are allowed in here
  template: '<button @click="onClick">Click!</button>'
})
export default class MyComponent extends Vue {
  // Initial data can be declared as instance properties
  message: string = 'Hello!'

  // Component methods can be declared as instance methods
  onClick (): void {
    window.alert(this.message)
  }
}

```

## Tipos de ampliación para el uso con plugins

Los plugins pueden agregar propiedades a la instancia global de Vue y opciones de componentes. En este caso la declaración son necesarias para compilar el plugins. En typeScript tenemos module augmentation o modulo de ampliación:


Para declarar la propiedad de la instancia `$myProperty` de tipo string:


JS

```
// 1. Make sure to import 'vue' before declaring augmented types
import Vue from 'vue'

// 2. Specify a file with the types you want to augment
//    Vue has the constructor type in types/vue.d.ts
declare module 'vue/types/vue' {
  // 3. Declare augmentation for Vue
  interface Vue {
    $myProperty: string
  }
}

```

Despues se incluirá el código como una declaración de archivos en su proyecto `my-property.s.ts` , se puede usar  `$myProperty` en una instancia de Vue.

JS

```
var vm = new Vue()
console.log(vm.$myProperty) // This should compile successfully

```

Tambien se puede declarar propiedades globales y opciones de componentes:

JS
```
import Vue from 'vue'

declare module 'vue/types/vue' {
  // Global properties can be declared
  // on the `VueConstructor` interface
  interface VueConstructor {
    $myGlobal: string
  }
}

// ComponentOptions is declared in types/options.d.ts
declare module 'vue/types/options' {
  interface ComponentOptions<V extends Vue> {
    myOption?: string
  }
}

```
Esto permite compilar el siguiente código:

JS
```


// Global property
console.log(Vue.$myGlobal)

// Additional component option
var vm = new Vue({
  myOption: 'Hello'
})

```

### Tipos de anotación retorno

Typescript puede tener dificultades para inferir en los tipos de metodos acertados.Por esta razon, se necesita anotar el tipo de retorno en los metodos como `render` y estos en `computed`.

JS

```

import Vue, { VNode } from 'vue'

const Component = Vue.extend({
  data () {
    return {
      msg: 'Hello'
    }
  },
  methods: {
    // need annotation due to `this` in return type
    greet (): string {
      return this.msg + ' world'
    }
  },
  computed: {
    // need annotation
    greeting(): string {
      return this.greet() + '!'
    }
  },
  // `createElement` is inferred, but `render` needs return type
  render (createElement): VNode {
    return createElement('div', this.greeting)
  }
})
```
## Despliegue de producción ##

## Activar el modo de producción

Durante el desarrollo, Vue proporciona una gran cantidad de advertencias para ayudarle con los errores y peligros más comunes. Sin embargo, estas cadenas o strings de advertencia se vuelven inútiles en la producción e incrementan el tamaño de la carga útil de su aplicación. Además, algunas de estas comprobaciones de advertencia tienen costes de tiempo de ejecución reducidos que pueden evitarse en el modo de producción.


### Sin las herramientas de construcción


Si estas usando el constructor completo, por ejemplo directamente incluyendo VUe una etiqueta script sin incluir una herramienta de contructor, se hace uso de la version minimizada de vue.min.js para la producción. Ambas versiones pueden ser encontradas en al [guia de instalación](https://vuejs.org/v2/guide/installation.html#Direct-lt-script-gt-Include).


### Con las herramientas de construcción

Cuando usamos una herramienta de compilación como Webpack ó Browserfy, el modo de producción será determinado por el `process.env.NODE_ENV` dentro del código fuente de Vue, y esto estará en modo de desarrollo por defecto. Ambas herramientas de compilación proporcionan formas de sobrescribir esta variable para habilitar el modo de producción de Vue, y las advertencias serán eliminadas por los minitransmisores durante la compilación. Todas las plantillas `vue-cli ` tienen estas pre-configuradas:

*Webpack 4+*

Se puede usar la opción `mode`:

JS
```
module.exports = {
  mode: 'production'
}

```

Se necesitará para usar en Webpack3 `DefinePlugin`:

JS
```
var webpack = require('webpack')

module.exports = {
  // ...
  plugins: [
    // ...
    new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify('production')
    })
  ]
}
```

### Browserfy

1. Ejecute el comando bundling con la variable de entorno `NODE_ENV` actual establecida en "producción". Esto le dice a `vueify` que evite incluir código relacionado con la recarga (hot-reload) y el desarrollo.

2. Aplique una transformación `envify` global a su paquete. Esto permite al minificador eliminar todas las advertencias del código fuente de Vue envueltas en bloques condicionales env variables. Por ejemplo: 

SHELL

```
NODE_ENV=production browserify -g envify -e main.js | uglifyjs -c -m > build.js
```

ó usando `envify` con Gulp:

JS

```
// Use the envify custom module to specify environment variables
var envify = require('envify/custom')

browserify(browserifyOptions)
  .transform(vueify)
  .transform(
    // Required in order to process node_modules files
    { global: true },
    envify({ NODE_ENV: 'production' })
  )
  .bundle()

```

Ó usando `envify` con Grunt y grunt-browserif:

JS

```

// Use the envify custom module to specify environment variables
var envify = require('envify/custom')

browserify: {
  dist: {
    options: {
      // Function to deviate from grunt-browserify's default order
      configure: b => b
        .transform('vueify')
        .transform(
          // Required in order to process node_modules files
          { global: true },
          envify({ NODE_ENV: 'production' })
        )
        .bundle()
    }
  }
}

```

### Rollup

Usando (rollup-plugin-replace)[https://github.com/rollup/rollup-plugin-replace]

JS

```

const replace = require('rollup-plugin-replace')

rollup({
  // ...
  plugins: [
    replace({
      'process.env.NODE_ENV': JSON.stringify( 'production' )
    })
  ]
}).then(...)

```

## Ampliación ##

## Router

## Router simple desde SCRATCH

Si solamente necesitas routeo simples y no deseas envolver características completas de la libreria del router, se puede renderizar dinamicamente componentes de page-level como:

JS 

```

const NotFound = { template: '<p>Page not found</p>' }
const Home = { template: '<p>home page</p>' }
const About = { template: '<p>about page</p>' }

const routes = {
  '/': Home,
  '/about': About
}

new Vue({
  el: '#app',
  data: {
    currentRoute: window.location.pathname
  },
  computed: {
    ViewComponent () {
      return routes[this.currentRoute] || NotFound
    }
  },
  render (h) { return h(this.ViewComponent) }
})

```

## Gestionar Estados ##

### Administración de estados simples desde Scratch

A menudo se pasa por alto que la fuente de la verdad en las aplicaciones Vue es el objeto de la data sin tratar  - una instancia de Vue sólo da acceso a los proxys. Por lo tanto, si tienes un trozo de estado que debería ser compartido por múltiples instancias, puedes compartirlo por identidad: 

JS

```
const sourceOfTruth = {}

const vmA = new Vue({
  data: sourceOfTruth
})

const vmB = new Vue({
  data: sourceOfTruth
})

```

Ahora siempre que `sourceOfTruth` es mutado, ambos `vmA` y `vmB` actualiza sus vistas automaticamente. Los subcomponentes dentro de cada instancia deberían tambien tener acceso via `this.$root.$data`. Tenemos una fuente unica de verdad, pero la depuración deberia ser tedioso.

Ayudar a solventar este problema, nosotros podemos adoptar unos patrones de STORE:

JS
```

var store = {
  debug: true,
  state: {
    message: 'Hello!'
  },
  setMessageAction (newValue) {
    if (this.debug) console.log('setMessageAction triggered with', newValue)
    this.state.message = newValue
  },
  clearMessageAction () {
    if (this.debug) console.log('clearMessageAction triggered')
    this.state.message = ''
  }
}

```

Note que todas las acciones que mutan el estado del store son puestas dentro del mismo store. Este tipo de gestión centralizada del estado facilita la comprensión de qué tipo de mutaciones pueden ocurrir y cómo se desencadenan. Ahora, cuando algo va mal, también tendremos un registro de lo que ocurrió antes de que se produjera el error. 

Además, cada instancia / componente puede poseer y gestionar su propio estado privado:


JS

```
var vmA = new Vue({
  data: {
    privateState: {},
    sharedState: store.state
  }
})

var vmB = new Vue({
  data: {
    privateState: {},
    sharedState: store.state
  }
})
```

## Server-Side Renderizado ##

## Nuxt.js

Configurar correctamente todos los aspectos discutidos de una aplicación lista para producción renderizada por el servidor puede ser una tarea desalentadora. Afortunadamente, hay un excelente proyecto comunitario que pretende hacer todo esto más fácil: [Nuxt.js](https://nuxtjs.org/). Nuxt.js es un framework de alto nivel construido sobre el ecosistema Vue que proporciona una experiencia de desarrollo extremadamente racionalizada para escribir aplicaciones universales Vue. Mejor aún, usted puede incluso usarlo como un generador estático de sitios (con páginas creadas como componentes Vue de un solo archivo)!



## Framework Quasar SSR + PWA

[Quasar Framework](https://quasar.dev/) generará una aplicación SSR (con entrega opcional de PWA) que aprovecha su sistema de construcción, configuración sensible y extensibilidad del desarrollador para hacer que el diseño y la construcción de su idea sea un juego de niños. Con más de cien componentes específicos compatibles con "Material Design 2.0", puede decidir cuáles desea ejecutar en el servidor, cuáles están disponibles en el navegador e incluso gestionar las etiquetas `<meta>` de su sitio. Quasar es un entorno de desarrollo basado en nodo.js y webpack que sobrecarga y agiliza el rápido desarrollo de aplicaciones SPA, PWA, SSR, Electron y Cordova, todo desde una misma base de código.


## Reactividad en profundidad ##

### Seguimiento de los cambios

Cuando usted pasa un objeto JavaScript simple a una instancia de Vue como su opción de `data`, Vue marchará a través de todas sus propiedades y las convertirá en `getter/setters` usando `Object.defineProperty`. Esta es una característica exclusiva de ES5, por lo que Vue no es compatible con IE8 y versiones inferiores.

Los `getter/setters` son invisibles para el usuario, pero bajo el hood permiten a Vue realizar el seguimiento de dependencias y la notificación de cambios cuando se accede a las propiedades o se modifican. Una advertencia es que las consolas de navegación formatean al `getter/setters` de forma diferente cuando se registran los objetos de datos convertidos, por lo que es posible que desee instalar `vue-devtools` para una interfaz más fácil de inspeccionar.

Cada instancia de componente tiene una instancia de observador (watcher) correspondiente, que registra cualquier propiedad "touched" durante el renderizado del componente como dependencias. Más tarde, cuando se activa el `setter`, notifica al watcher, lo que hace a su vez que el componente vuelva a ejecutarse. 

### Advertencia de detecciòn de cambios

Debido a las limitaciones del JavaScript moderno (y al abandono de `Object. observe`), Vue no puede detectar la adición o eliminación de propiedades. Como Vue realiza el proceso de conversión `getter/setter` durante la inicialización de la instancia, una propiedad debe estar presente en el objeto de datos para que Vue lo convierta y lo haga reactivo. Por ejemplo: 


JS
```
var vm = new Vue({
  data: {
    a: 1
  }
})
// `vm.a` is now reactive

vm.b = 2
// `vm.b` is NOT reactive

``` 

Vue no permite añadir dinámicamente nuevas propiedades reactivas a nivel raíz a una instancia ya creada. Sin embargo, es posible añadir propiedades reactivas a un objeto anidado utilizando el método `Vue.set(object, propertyName, value)`: 

JS
```
Vue.set(vm.someObject, 'b', 2)
```

Tambien puede usarse la instancia del metodo `vm.$set`, la cual es un alias global de `Vue.set`:

JS
```
this.$set(this.someObject, 'b', 2)
```

A veces puede querer asignar un número de propiedades a un objeto existente, por ejemplo usando `Object.assign()` o `_. extend()`. Sin embargo, las nuevas propiedades añadidas al objeto no provocarán cambios. En tales casos, cree un objeto nuevo con propiedades tanto del objeto original como del objeto mixto: 

JS

```
// instead of `Object.assign(this.someObject, { a: 1, b: 2 })`
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
```

### Declaración de propiedades reactivas

Dado que Vue no permite añadir dinámicamente propiedades reactivas a nivel de raíz, debe inicializar las instancias de Vue declarando todas las propiedades de datos reactivos a nivel de raíz por adelantado, incluso con un valor vacío: 



JS
```
var vm = new Vue({
  data: {
    // declare message with an empty value
    message: ''
  },
  template: '<div>{{ message }}</div>'
})
// set `message` later
vm.message = 'Hello!'

```



Si no declara el `message` en la opción de datos, Vue le advertirá que la función de render está intentando acceder a una propiedad que no existe.

Hay razones técnicas detrás de esta restricción - elimina una clase de casos extremos en el sistema de seguimiento de dependencias, y también hace que las instancias de Vue sean más agradables con los sistemas de comprobación de tipos. Pero también hay una consideración importante en términos de mantenibilidad del código: el objeto de la `data` es como el esquema para el estado de su componente. Declarar todas las propiedades reactivas por adelantado hace que el código del componente sea más fácil de entender cuando se vuelve a visitar más tarde o es leído por otro desarrollador. 

### Tráfico actualizado Asincrónico

Vue realiza actualizaciones DOM de forma asíncrona. Siempre que se observe un cambio de datos, se abrirá una cola y se almacenarán en un búfer todos los cambios de datos que se produzcan en el mismo bucle de eventos. Si el mismo observador se activa varias veces, será empujado a la cola sólo una vez. Esta desduplicación tamponada es importante para evitar cálculos innecesarios y manipulaciones DOM. Luego, en el siguiente bucle de eventos "tic", Vue nivela o limpia la cola y realiza el trabajo actual (ya de-duplicado). Internamente Vue prueba `Promise.then`, `MutationObserver`, y `setImmediate` para la cola asíncrona y vuelve a `setTimeout(fn,0)`.

Por ejemplo, cuando se define `vm.someData = new value'`, el componente no se volverá a procesar inmediatamente. Se actualizará en el siguiente "tic", cuando la cola se limpie. La mayoría de las veces no necesitamos preocuparnos por esto, pero puede ser difícil cuando quieres hacer algo que depende del estado de DOM posterior a la actualización. Aunque `Vue.js` generalmente anima a los desarrolladores a pensar de una manera "basada en datos" y evitar tocar el DOM directamente, a veces puede ser necesario ensuciarse las manos. Para esperar hasta que Vue.js haya terminado de actualizar el DOM después de un cambio de datos, puede utilizar `Vue.nextTick(callback)` inmediatamente después de cambiar los datos. La llamada de retorno se llamará después de que el DOM haya sido actualizado. Por ejemplo: 


HTML

```
<div id="example">{{ message }}</div>

```


JS


```
var vm = new Vue({
  el: '#example',
  data: {
    message: '123'
  }
})
vm.message = 'new message' // change data
vm.$el.textContent === 'new message' // false
Vue.nextTick(function () {
  vm.$el.textContent === 'new message' // true
})


```

También está el método de instancia `vm.$nextTick()`, que es especialmente útil dentro de los componentes, ya que no necesita Vue global y su contexto del `this ` del callback's estará automáticamente vinculada a la instancia de Vue actual: 


JS

```

Vue.component('example', {
  template: '<span>{{ message }}</span>',
  data: function () {
    return {
      message: 'not updated'
    }
  },
  methods: {
    updateMessage: function () {
      this.message = 'updated'
      console.log(this.$el.textContent) // => 'not updated'
      this.$nextTick(function () {
        console.log(this.$el.textContent) // => 'updated'
      })
    }
  }
})

```

Como `$nextTick()` devuelve una promesa, puede lograr lo mismo que lo anterior usando la nueva sintaxis async/wait de ES2016: 

JS

```
methods: {
  updateMessage: async function () {
    this.message = 'updated'
    console.log(this.$el.textContent) // => 'not updated'
    await this.$nextTick()
    console.log(this.$el.textContent) // => 'updated'
  }
}

```

## Meta ##

## Comparación con otros Frameworks ##

Esta es la página más compleja de escribir pero sinceramente es de gran importancia. Probablemente has tenido problemas que has intentado resolver y has usado otra libreria para resolverlos. LLegaste aquí por que quieres saber si puedes resolver tu problema en particular de una mejor manera. Es lo que aquí deseamos responderte.

También intentamos totalmente evitar prejuicios. Como el corazón del equipo, nos encanta demasiado Vué. Existen algunos problemas en los que pensamos que se resuelven de una mejor manera. Si no lo creemos de esa manera, no podriamos estar trabajando en ello. Sin embargo queremos ser justos y precisos. En donde otras librerías ofrecen avances significativos, como el vasto ecosistema de alternativas para el renderizado de React, de todas maneras  trataremos de enlistarlas.

## React

React y Vue comparten muchas similaridades, entre ellas mencionamos:

1. Utiliza un virtual DOM
2. Provee componentes de vista reactivos y compuestos.
3. Se enfoca en el nucleo de la librería, atendiendo aspectos como el enrutamiento y la gestión global de estado dirigidas por librerías guías.

Siendo tán similares a la vista, hicimos mayor énfasis en esta comparación que cualquier otra. Queremos garantizar no solo la presición técnica, sino también el balance. Señalamos donde React supera a Vue, por ejemplo en lo rico de sus ecosistemas y la variedad en la personalización de sus renderizados.

Al decir esto, es inevitable que la comparación pudiera parecer en favor de los usuarios de Vue, pero muchos de los puntos que aquí se tocan son muy subjetivos. Sabemos de la existencia de varios aspectos tecnicos y en primer lugar, en esta comparación intenta resaltar las razones del por que Vue pudiera convenir más si tus preferencias coinciden con las nuestras.

Algunas de las próximas secciones pudieran ser un poco anticuadas debido a las recientes actualizaciones en React 16+ y nuestra intensión es trabajar con la comunidad de React para actualizar esta sección en el futuro cercano.

----------------------

Migrando desde Vue 1.x

### ¿A partir de donde deberiamos migrar?

1. Inicia usando el ayudante para la migración en un proyecto actual. Nosotros cuidadosamente hemos reducido y simplificado desarrollos complejos en una línea de comandos simple. En el momento en el que se reconozca una característica obsoleta, se te notificará ofreciendo sugerencias y dando links para mayor información.

2. En caso de que la ayuda para la migración no sea precisa, puedes buscar  a través de la tabla de contenidos de esta página.

3. Si tienes algunas versiones no oficiales, correlas y observa si las fallas continuan. En caso de no tenerlas, simplemente abre la aplicación en tu navegador y visualiza alertas o errores al navegar a través de ella.

4. En este punto, tu aplicación debería haber migrado completamente. Sin embargo si se desea mayor información, puedes continuar leyendo esta página o puedes explorar la nueva y mejorada guia desde el inicio. Muchas de las partes se podrían saltar si ya estas familiarizado con los conceptos básicos.

###  ¿Cuanto tomaría la migración de una aplicación de Vue 1.x a 2.x?

Dependiendo de algunos factores:

1. El tamaño de tu aplicación

2. Cuantas veces te distraes jugando con las nuevas características interesantes. Sin juzgar, también nos sucede mientras construimos la 2.0!

3. Cual de las viejas caraterísticas estas usando. La mayoría puede ser actualizada con una simple "busqueda y remplazo" pero otras podrían tomar algunos cuantos minutos. Si actualmente no estas siguiendo las buenas prácticas, Vue 2.0 intentará obligarte incanzablemente a ello. Esto pudiera ser bueno pero también se convertiría en un factor influyente.

### Si actualizo a Vue 2, tendré también que actualizar Vuex y Vue Router?

Vue Router 2 solamente es compatible con Vue 2, por lo que tendrás que seguir los pasos de migración para Vue Router. Afortunadamente la mayoría de las aplicaciones no tienen una gran cantidad de codigo router, por lo tanto no nos tomará más de una hora.

Con respecto a Vuex, incluso la versión 0.8 es compatible con Vue 2.0, por lo que no te verás obligado a actualizar. La unica razón por la que querras actualizar automaticamente is para dar paso a los avances de las nuevas características in Vuex 2, como los modulos.

## Plantillas

### Instancias fragmentas Removed

Cada componente debe tener exactamente un elemento raíz. Las instancias fragmentas ya no son permitidas. Si se tiene una plantilla como acontinuación:

HTML

```
<p>foo</p>
<p>bar</p>
``` 

Es recomendable encerrar todo el contenido en un nuevo elemento, como acontinuación:

HTML

```
<div>
  <p>foo</p>
  <p>bar</p>
</div>
```

Ruta Mejorada
Corre de principio a fin tu prototipo o aplicación después realizar mejoras y observa las alertas de la consola sobre los multiples elementos raices en una plantilla.

## Ciclo de vida del Hooks

### `beforeCompile` removed

Usa el hook `created` en su lugar. 

Mejor Ruta: Consulta la ayuda de migración sobre tu código base para encontrar todos los ejemplos de este hook.

### `Compiled` replaced

En su lugar usa el nuevo hook `mounted`.

Mejor Ruta: Consulta al ayudante de migración sobre tu código base para encontrar todos los ejemplos sobre este hook.

### `attached` removed

Usa un in-DOM check particular en otros hook. Por ejemplo, para reemplazar:

JS
```
attached: function () {
  doSomething()
}
```

Podrías usar:

JS
```
mounted: function () {
  this.$nextTick(function () {
    doSomething()
  })
}
```

Mejor Ruta: Consulta al ayudante de migración sobre tu código base para encontrar todos los ejemplos sobre este hook.

### `detached` removed

Usa un in-DOM check particular en otros hook. Por ejemplo, para reemplazar:

JS
```
detached: function () {
  doSomething()
}
```

Podrías usar:
JS
```
destroyed: function () {
  this.$nextTick(function () {
    doSomething()
  })
}
```

Mejor Ruta: Consulta el ayudante de migración sobre tu código base para encontrar todos los ejemplos de este hook.

### `init` renamed

En su lugar usa el nuevo hook `beforeCreate`, el cual es esencialmente  la misma cosa. Fue renombrado por consistencia con otros ciclos vítales de métodos.

Ruta Mejorada: Consulta el ayudante de migración sobre tu código base para encontrar todos los ejemplos sobre este hook.

### `ready` replaced

En su lugar usa el nuevo hook `mounted`. Sin embargo debe notarse que con `mounted`, no hay garantia de ser documentado. Por ello, incluye también `Vue.nextTick` / `vm.$nextTick`. Por ejemplo:

JS
```
mounted: function () {
  this.$nextTick(function () {
    // code that assumes this.$el is in-document
  })
}
```

Ruta Mejorada: Consulta el ayudante de migración sobre tu codigo base para encontrar todos los ejemplos de este hook.

## `v-for`

### `v-for` Argument Order for Arrays changed

Al incluir `index`, el orden usado del argumento para el array será `(index,value)`. Es ahora `(value,index)` para ser más consistente con los métodos de arreglo nativo de JavaScript como `forEach` y `map`.

Ruta Mejorada: Consulta el ayudante de migración sobre tu código base para encontrar ejemplos del orden de los argumentos obsoletos. Observa que si nombras tu argumentos principales de una manera inusual como `position` o `num`, el ayudante no lo resaltará.

### `v-for` Argument Order for Objects changed

Cuando se incluye un nombre propio o clave 
propia, el orden de los argumentos usados para los objetos será `(name, value)`. Es ahora `(value, name)` para ser más consistente con la iteración de los objetos comunes como lodash's.

Ruta Mejorada: Consulta el ayudante de migración sobre tu código base para encontrar ejemplos del orden de argumentos obsoletos. Observa que si nombras tus argumentos claves como `name` o `property`, el ayudante no lo resaltará.

### `$index` and `$key` removed

Las variables implicitas asignadas `$index` y `$key` han sido removidas en favor de definirlas explicitamente en `v-for`. Esto simplifica el código para los desarrolladores menos experimentados con Vue y también resulta en un comportamiento más claro al tratar con lazos anidados.

Ruta mejorada: Consulta con el ayudante de migración sobre tu código base para encontrar ejemplos de estas variables removidas. Si ignoras cualquiera de ellas, debes también consultar errores de consola como: `Uncaught ReferenceError: $index is not defined`

### `track-by` replaced

`track-by` ha sido reemplazado por `key`, el cual trabaja como cualquier otro atributo: sin el `v-bind:` or el prefijo `:`, es tratado como un string literal. En la mayoría de casos, querras usar un vinculo dinámico, el cual espera una expresión completa en vez de una clave. Por ejemplo, en lugar de: 

HTML
```
<div v-for="item in items" track-by="id">
```  

Escribiremos ahora:

HTML
```
<div v-for="item in items" v-bind:key="item.id">
```

Ruta Mejorada: Consulta el ayudante de migración en tu código base para encontrar ejemplos de `track-by`

### `v-for` Range Values changed

Anteriormente, `v-for="number in 10"` tendría `number` iniciando en 0 y finalizando en 9. Ahora inicia en 1 y finaliza en 10.

Ruta Mejorada: Busca en tu código base la expresión regular `/\w+ in \d+/`. Donde sea que aparezca en un `v-for`, confirma si pudiera verse afectado.

## Props

### `coerce` Prop Option removed

Si quieres coaccionar un prop, establece un valor de computo local basado en él en su lugar. Por ejemplo, en su lugar:

JS
```
props: {
  username: {
    type: String,
    coerce: function (value) {
      return value
        .toLowerCase()
        .replace(/\s+/, '-')
    }
  }
}
```

Podrías escribir:

JS
```
props: {
  username: String,
},
computed: {
  normalizedUsername: function () {
    return this.username
      .toLowerCase()
      .replace(/\s+/, '-')
  }
}
```

Existen algunas ventajas:

1. Continuas teniendo acceso al valor original de el prop.

2. Estas obligado a ser más explicito, al dar un nombre a tu valor coaccionado que lo diferencie de el valor pasado en el prop.

Ruta Mejorada: Consulta el ayudante de migración sobre tu código base para encontrar ejemplos de la opción `coerce`.

### `twoWay` Prop Option removed

Los Props están ahora siempre en una sola dirección. Para producir efectos colaterales en el patrón de alcance, un componente necesita para emitir explicitamente un evento en vez de confiar en vinculaciones implicitas.

### `.once` and `.sync` Modifiers on `v-bin` removed

Los Props están ahora siempre en una sola dirección. Para producir efectos colaterales en el patrón de alcance, un componente necesita para emitir explicitamente un evento en vez de confiar en vinculaciones implicitas.

### Prop Mutation deprecated

La mutación del prop localmente esta ahora considerado un anti-patron, por ejemplo, declarando un prop y luego estableciendo `this.myProp = 'someOtherValue'` en el componente. Debido al nuevo mecanismo de renderizado, 

