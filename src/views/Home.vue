<template>
  <div class="home">
    <h3>Ik ga weer rekenen</h3>
    <form @submit.prevent="controleer">
      <p>
        {{ eersteGetal }}
        <span v-if="isPlusSom">+</span>
        <span v-if="isMinSom">-</span>
        {{ tweedeGetal }}
        =
        <input v-model="antwoord" type="number" />

        <button type="submit">Controleer</button>
      </p>
    </form>
    <img v-if="antwoordGoed" src="@/assets/ballonnen-en-slingers.jpg" />
    <img v-if="antwoordFout" src="@/assets/rood-kruis.jpg" />

    <button v-if="isErEenNieuweSom" @click="nieuweSom">
      Volgende som
    </button>
  </div>
</template>

<script>
const sommetjes = [
  "100+100=",
  "30+20=",
  "100-30=",
  "10+30=",
  "100-10=",
  "20+30=",
  "30-10=",
  "20+30=",
  "10+10="
];
export default {
  name: "Home",
  data() {
    return {
      som: 0,
      antwoord: null,
      hoogsteGetal: 100,
      resultaat: null
    };
  },
  computed: {
    antwoordGoed() {
      return this.resultaat == true;
    },
    antwoordFout() {
      return this.resultaat == false;
    },
    huidigeSom() {
      return sommetjes[this.som];
    },
    isErEenNieuweSom() {
      return this.som + 1 < sommetjes.length;
    },
    isPlusSom() {
      return this.huidigeSom.includes("+");
    },
    isMinSom() {
      return this.huidigeSom.includes("-");
    },
    tekenPositie() {
      if (this.isPlusSom) {
        return this.huidigeSom.indexOf("+");
      }
      if (this.isMinSom) {
        return this.huidigeSom.indexOf("-");
      }

      return -1;
    },
    isTekenPositie() {
      return this.huidigeSom.indexOf("=");
    },
    eersteGetal() {
      return parseInt(this.huidigeSom.substring(0, this.tekenPositie));
    },
    tweedeGetal() {
      return parseInt(
        this.huidigeSom.substring(this.tekenPositie + 1, this.isTekenPositie)
      );
    },
    oplossing() {
      if (this.isPlusSom) {
        return this.eersteGetal + this.tweedeGetal;
      }
      if (this.isMinSom) {
        return this.eersteGetal - this.tweedeGetal;
      }

      return 0;
    }
  },
  methods: {
    controleer() {
      this.resultaat = this.oplossing == this.antwoord;
    },

    nieuweSom: function() {
      this.som += 1;
      this.antwoord = null;
      this.resultaat = null;

      if (this.oplossing > this.hoogsteGetal) {
        this.nieuweSom();
      }
    },
    volgendeSom: function() {
      this.nieuweSom();
    }
  }
};
</script>
