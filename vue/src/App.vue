<!--Dit is de maintemplate, deze zal gerendered worden op de pagina. De componenten hierin zullen ook gerendered worden.-->
<template>
  <div class="container" id="app">
<!--De pagina bestaat uit twee componenten. SourceSelection heeft een custom event, elke keer je een nieuwsbron
selecteert. De event handler wordt toegevoegd aan het methodsobject. Die ontvangt de nieuwe selectie.-->
    <SourceSelection v-on:sourceChanged="sourceChanged"></SourceSelection>

  </div>
</template>

<script>
// We importeren de component zodat we hun tags kunnen gebruiken in de maintemplate.
  import SourceSelection from './components/SourceSelection'

  export default {
    name: 'app',
    components: {
      SourceSelection
    },
    data () {
      return {
        // Deze 'variabele' declareren we al zodat selectie leeg is in begin.
        source: ''
      }
    },
    methods: {
// Deze methode wordt gecalled bij het sourceChanged event. Dat in feite 'change' event is maar wordt in component file gemaakt.
      sourceChanged: function (source) {
        // Verander de source variabele naar de gekozen nieuwsbron.
        this.source = source
      }
    }
  }
</script>
