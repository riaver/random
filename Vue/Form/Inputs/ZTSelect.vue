<script setup>
import { computed, onMounted, ref } from 'vue'

const props = defineProps({
  options: {
    type: Array,
    required: true,
  },
  width: {
    type: String,
    required: false,
    default: () => '30rem'
  },
  clearable: {
    type: Boolean,
    required: false,
    default: () => true
  },
})
onMounted(() => {
  /**
   * This is to detect when the user clicks somewhere else
   */
  window.addEventListener('click', () => {
    inputActive.value = false
    filterInput.value = ''
    return false
  })
})
// The input used for searching
const filterInput = ref('')

// The final input value
const inputValue = ref('')

// Is the user interacting with the select
const inputActive = ref(false)

// Is the list of options open
const listOpen = computed(() => {
  return inputActive.value
})
// The list of filtered options
const filteredOptions = computed(() => {
  if (filterInput.value === '') {
    return props.options
  }
  return props.options.filter((option) => {
    return option.label.includes(filterInput.value)
  })
})

// The text to display in the input.
const inputTextDisplay = computed(() => {
  if(inputActive.value && filterInput.value.length === 0) {
    return 'Search...'
  }
  if(inputValue.value.length > 0) {
    return inputValue.value;
  }
  return 'Select...'
})

// Click events are captured by the parent
const clickedEvent = ($event) => {
  // Clicking on the input activates the Select
  if ($event.target.nodeName === 'INPUT') {
    inputActive.value = true
    return false
  }
  // Clicking on a list option selects the option
  if ($event.target.nodeName === 'LI') {
    inputValue.value = $event.target.attributes['data-value'].value
    inputActive.value = false
    filterInput.value = ''
    return false
  }
}
</script>

<template>
  <div
    :class="{ active: inputActive }"
    class="zt-component-select"
    @click.stop.capture.prevent="clickedEvent"
  >
    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
      <path d="m19.5 8.25-7.5 7.5-7.5-7.5" />
    </svg>
    <span
      :class="{
        hidden: filterInput.length > 0
      }"
    >
      {{ inputTextDisplay }}
    </span>
    <input v-model="filterInput" type="text" />
    <div  :class="{ open: listOpen }">
      <template v-if="filteredOptions.length > 0">
        <ul
          v-if="listOpen"
          :class="{ open: listOpen }"
        >
          <li v-for="option in filteredOptions" :key="option.value" :data-value="option.value">
            {{ option.label }}
          </li>
        </ul>
      </template>
      <template v-else>
        <span>
          No Results
        </span>
      </template>
    </div>
  </div>
</template>

<style scoped>
.zt-component-select {
  position: relative;
  line-height: 1rem;
  width:v-bind(width);
  border-radius: 0.25rem;
  border: 1px solid #c5c5c5;
  cursor: pointer;
  svg {
    position: absolute;
    right:0.25rem;
    height: 1.5rem;
    top:0.25rem;
    transition: all 0.25s;
  }
  input {
    padding: 0.25rem 2rem 0.25rem 0.25rem;
    margin: 0;
    border: none;
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    background: transparent;
    outline: none;
  }
  span {
    visibility: visible;
    padding: 0.5rem;
    display: block;
  }
  &.active {
    svg {
      transform: rotateZ(180deg);
    }
    span {
      &.hidden {
        color: transparent;
      }
    }
  }
  div {
    opacity: 0;
    position: absolute;
    top: calc(100% + 0.25rem);
    transition: all 0.25s;
    width: 100%;
    box-sizing: border-box;
    max-height: 7rem;
    overflow-y: auto;
    border-radius: 0.25rem;
    background-color: white;
    box-shadow: 0 0 15px -5px rgba(0, 0, 0, 0.68);
    &.open {
      opacity: 1;
    }
    span {
      padding: 1rem;
      text-align: center;
    }
  }
  ul {
    padding: 0;
    margin: 0;
    list-style: none;
    li {
      line-height: 1.25rem;
      padding: 0.25rem 0.5rem;
      cursor: pointer;

      &:hover {
        background-color: rgba(0, 0, 0, 0.25);
      }
    }
  }
}
</style>
