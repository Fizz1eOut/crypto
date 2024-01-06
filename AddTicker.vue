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
                    @focus="$emit('focus')"
                    @input="searchAndMatch"
                    class="cryptonomicon__input"
                    placeholder="Например BTC"
                />
            </div>
            <ul v-if="ticker.trim() !== ''">
                <li v-for="symbol in matchedSymbols" :key="symbol" @click="addCoin(symbol)">{{ symbol }}</li>
            </ul>
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

  emits: [ "add-ticker", "focus", ],

  data() {
    return {
        // Введенный текст в инпут
        ticker: "",

        // Массив для хранения найденных символов
        matchedSymbols: [],

        // Идентификатор таймера для задержки запросов
        searchTimeout: null,
    };
  },

  methods: {
    add() {
      if (!this.ticker.trim()) {
        return;
      }
      this.$emit("add-ticker", this.ticker);
      this.ticker = "";
    },

    searchAndMatch() {
        // Очищаем предыдущий тайм-аут (если есть)
        clearTimeout(this.searchTimeout);

        // Устанавливаем тайм-аут для задержки запроса
        this.searchTimeout = setTimeout(() => {

          // Выполняем сетевой запрос
          fetch('https://min-api.cryptocompare.com/data/all/coinlist?summary=true')
            .then(response => response.json())
            .then(data => {

              // Получаем объект ответа
              const coins = data.Data;

              // Хранение найденных символов
              const matchedSymbols = [];

              // Ищем совпадение с введенным текстом в инпуте
              for (const coinKey in coins) {
                if (coinKey.toUpperCase().includes(this.ticker.toUpperCase())) {
                    // console.log(coinKey.toUpperCase().includes(this.ticker.toUpperCase()));

                    // Добавляем символ монеты в массив
                  matchedSymbols.push(coins[coinKey].Symbol);
                  // console.log(coins[coinKey].Symbol)

                  // Если найдено 4 совпадения, завершаем поиск
                  if (matchedSymbols.length === 4) {
                    break;
                  }
                }
              }

              // Обновляем массив совпадений в данных
              this.matchedSymbols = matchedSymbols;
            })
        }, 300); // Задержка в 300 миллисекунд (можно настроить под себя)
      }
  },
  addCoin(coinSymbol) {
    this.tickers.push(coinSymbol);
    console.log(this.tickers);
  },
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