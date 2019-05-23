
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
