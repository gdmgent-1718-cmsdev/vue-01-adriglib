<!--Dit is de template van de selectiecomponent en wordt zo via app.vue gerendered op het scherm.-->
<template>
  <div class="sourceselection">
    <div>
      <div class="jumbotron">
        <h2><span class="glyphicon glyphicon-list-alt"></span>&nbsp;Populaire nieuwsbronnen.</h2>
        <h3>Selecteer een nieuwsbron, deze haallt de beschrijving uit de <a href="https://newsapi.org/v1/sources?language=en">News Api</a> en geeft een korte beschrijving en een link naar hun website.</h3>
        <select class="form-control" v-on:change="sourceChanged">
          <!--Omdat de eerste optie bij de select tag wordt getoond -->
          <option value="">Kies een nieuwsbron uit de lijst ...</option>
          <!--Schrijf alle nieuwsbronnen in de sources array als optie. We binden ook het id eraan voor het sourceChanged event listener.-->
          <option v-for="source in sources" v-bind:value="source.id">{{source.name}}</option>
        </select>
        <!--Is 'source' true (niet leeg.) toon dan deze div met de beschrijving, naam en link.-->
        <div v-if="source">
          <h4>{{source.description}}</h4>
          <a v-bind:href="source.url" class="btn btn-primary" target="_blank">Go To {{source.name}} Website</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'sourceselection',
    data () {
      return {
        // We hebben een lege array 'sources'. Hierin zitten al de nieuwsbronnen. Deze array wordt gevuld door de created functie.
        sources: [],
        // Dit is de huidig gekozen nieuwsbron Dit zal een object worden met naam, beschrijving etc..
        source: ''
      }
    },
    methods: {
      // Deze functie wordt opgeroepen als je een nieuwe bron selecteert.
      // We gebruiken een loop om uit al de nieuwsbronnen van de API de geselecteerde eruit te halen.
      sourceChanged: function (e) {
        // Een loop om alles te controleren.
        for (let i = 0; i < this.sources.length; i++) {
          // Als de id van de door de user geselecteerde optie gelijk is aan die van de sourceArray dan...
          if (this.sources[i].id === e.target.value) {
            // ...initialiseer je het source object met het gekozen object uit de array.
            this.source = this.sources[i]
          }
        }
        // We geven de geselecteerde source mee als parameter om in App.vue daar het juiste sourceobject te ontvangen.
        this.$emit('sourceChanged', e.target.value)
      }
    },
    // We gebruiken een 'lifecycle hook', hier kan je al data ontvangen voordat de Tamplates zijn gerendered.
    created: function () {
      this.$http.get('https://newsapi.org/v1/sources?language=en')
        .then(response => {
          // Vul de sources array met de info van de http request
          this.sources = response.data.sources
        })
    }
  }
</script>
