<template>
    <section>
        <div class="cryptonomicon__group">
            <div class="cryptonomicon__row">
                <label 
                    for="wallet" 
                    class="cryptonomicon__label"
                >
                    Тикер
                </label>

                <input
                    v-model="ticker"
                    @keydown.enter="add"
                    class="cryptonomicon__input"
                    placeholder="Например BTC"
                />
            </div>
        </div>

        <p 
            v-if="hasTicker"
            class="cryptonomicon__message"
        >
            Такой тикер уже существует
        </p>

        <add-button
            @click="add"
            :disabled="disabled"
        />
    </section>
</template>

<script>
import AddButton from "./AddButton.vue";

export default {
  components: {
    AddButton
  },

  props: {
    disabled: {
      type: Boolean,
      required: false,
      default: false
    },
    hasTicker: {
        type: Boolean,
        required: false,
        default: false
    }
  },

  emits: [ "add-ticker" ],

  data() {
    return { 
        ticker: "",
    };
  },

  methods: {
    add() {
      if (!this.ticker.trim()) {
        return;
      }
      this.$emit("add-ticker", this.ticker);
      this.ticker = "";
    }
  }
};
</script>

<style scoped>
.cryptonomicon__message {
    font-style: 16px;
    color: red;
}
.cryptonomicon__row {
    width: 100%;
}
.cryptonomicon__label {
    font-size: 30px;
    color: black;
}
.cryptonomicon__input {
    margin-top: 10px;
    display: flex; 
    align-items: center; 
    justify-content: center;
    max-width: 300px;
    width: 100%;
    padding: 10px 20px;
    border: 1px solid black;
    outline-color: blue;
    border-radius: 10px;
    font-size: 20px;
    color: black;
}
</style>