<template>
            <div
              v-for="t in paginatedTickers"
              :key="t.name"
              @click="select(t)"
              :class="{
                'cryptonomicon__item--target': selectedTicker === t
              }"
              class="cryptonomicon__item"
            >
              <div class="cryptonomicon__text">
                <dt class="cryptonomicon__name">
                  {{ t.name }} - USD
                </dt>
                <dd class="cryptonomicon__price">
                  {{ formatPrice(t.price) }}
                </dd>
              </div>
              <!-- <div class="w-full border-t border-gray-200"></div> -->
              <button
                @click.stop="handleDelete(t)"
                class="cryptonomicon-button__delete"
              >
                <svg
                  class="cryptonomicon-button__delete--svg"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                  fill="#718096"
                  aria-hidden="true"
                >
                  <path
                    fill-rule="evenodd"
                    d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
                    clip-rule="evenodd"
                  ></path>
                  </svg>
                  Удалить
              </button>
            </div>
</template>

<script>

</script>

<style scoped>
.cryptonomicon__item {
    max-width: 230px;
    width: 100%;
    text-align: center;
    padding: 20px;
    border-radius: 10px;
    cursor: pointer;
}
.cryptonomicon__item--target {
    border: 3px solid blue;
}
.cryptonomicon__text {
    text-align: center;
}
.cryptonomicon__name {
    font-size: 16px;
    color: gray;
    margin: 0;
}
.cryptonomicon__price {
    font-size: 20px;
    color: black;
    padding: 10px 0;
    margin: 0;
}
.cryptonomicon-button__delete {
    padding: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    width: 100%;
    border-radius: 10px;
    border: none;
    cursor: pointer;
    background-color: transparent;
    transition: all 0.3s ease-in-out;
}
.cryptonomicon-button__delete:hover {
    color: blue;
}
.cryptonomicon-button__delete:hover .cryptonomicon-button__delete--svg {
    fill: blue;
}
.cryptonomicon-button__delete--svg {
    width: 20px;
    height: 20px;
    transition: all 0.3s ease-in-out;
}
</style>