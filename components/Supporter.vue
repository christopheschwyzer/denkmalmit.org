<template>
  <section id="supporter">
    <h3>Unterstützer</h3>
    <p>Wir danken folgenden Personen (und auch allen, die anonym bleiben wollen) herzlichst für die ideelle und/oder materielle Unterstützung. Ohne euch würde es Denkmal.org nicht geben. (Ohne bestimmte Reihenfolge)</p>

    <ul v-if="supporter" class="supporter-list">
      <li v-for="(item, index) in supporter" :key="index">
        {{ item.name }} {{ item.heart }}
      </li>
    </ul>
    <div v-else>
      <spinner/>
    </div>
    <div class="error" v-if="error">
      Supporter können nicht geladen werden.
    </div>

    <div class="supporter-official">
      <figure>
        <img src="/images/supporter/kultur-bs.svg" alt="">
        <figcaption>Unterstützt durch die Abteilung Kultur Basel-Stadt</figcaption>
      </figure>
    </div>
  </section>
</template>

<script>
import sheetsu from 'sheetsu-node';
import Spinner from '~/components/Spinner'

var config = {
  address: 'aaf997d1065a',
};

export default {
  components: {
    Spinner
  },
  data: () => ({
    supporter: null,
    error: null
  }),
  methods: {
    sheetsuSuccess: function (data) {
      this.supporter = JSON.parse(data);
    },
    sheetsuError: function (data) {
      this.error = true;
    }
  },
  mounted() {
    var client = sheetsu(config);
    client.read({}).then(this.sheetsuSuccess, this.sheetsuError);
  }
}
</script>

<style lang="scss" scoped>
.supporter-list {
  padding: 0;
  font-size: 0.8em;

  @media screen and (min-width: 500px) {
    text-align: justify;
  }

  > li {
    display: inline;
    font-style: italic;
    max-width: 100%;
    word-wrap: break-word;

    &:not(:last-child)::after {
      content: '•';
      white-space: pre;
    }
  }
}

.supporter-official {
  figure {
    margin: 2em 0.2em;
    width: 150px;
  }
  img {
    width: 90%;
  }
  figcaption {
    font-size: 0.7em;
  }
}
</style>
