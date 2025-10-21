<template>
  <input
    ref="inputRef"
    v-model="internalValue"
    type="text"
    class="px-2 py-1 border rounded focus:outline-none focus:ring-2 focus:ring-blue-500 transition-all duration-200 number-input"
    :style="{ width: inputWidth }"
    @input="handleInput"
    @focus="handleFocus"
    @blur="handleBlur"
  />
</template>

<script setup lang="ts">
import { ref, watch, onMounted } from 'vue';

const props = defineProps({
  modelValue: {
    type: [String, Number],
    default: '',
  },
  minWidth: {
    type: Number,
    default: 72,
  },
});

const emit = defineEmits([
  'update:modelValue',
  'focus',
  'blur'
]);

const inputRef = ref<HTMLInputElement | null>(null);
const internalValue = ref<string>('');
const inputWidth = ref<string>(`${props.minWidth}px`);

// Обработка события blur
const handleBlur = (event: FocusEvent) => {
  formatValue();
  emit('blur', event);
};

// Обработка события focus
const handleFocus = (event: FocusEvent) => {
  emit('focus', event);
};

// Форматирование значения с пробелами каждые 3 цифры
const formatValue = () => {
  const rawValue = internalValue.value.replace(/\D/g, '');
  const formattedValue = rawValue.replace(/\B(?=(\d{3})+(?!\d))/g, ' ');
  internalValue.value = formattedValue;
  updateInputWidth();
};

// Обработка ввода: разрешаем только цифры
const handleInput = (event: Event) => {
  const target = event.target as HTMLInputElement;
  const value = target.value.replace(/\D/g, ''); // Удаляем всё, кроме цифр
  internalValue.value = value;
  emit('update:modelValue', value);
  updateInputWidth();
};

// Обновление ширины инпута
const updateInputWidth = () => {
  if (!inputRef.value) return;
  // Создаём временный элемент для расчёта ширины
  const span = document.createElement('span');
  span.style.visibility = 'hidden';
  span.style.whiteSpace = 'nowrap';
  span.style.position = 'absolute';
  span.style.fontSize = window.getComputedStyle(inputRef.value).fontSize;
  span.style.fontFamily = window.getComputedStyle(inputRef.value).fontFamily;
  span.textContent = internalValue.value || '0';
  document.body.appendChild(span);
  const width = Math.max(72, span.offsetWidth + 18); // 18px для padding
  document.body.removeChild(span);
  inputWidth.value = `${width}px`;
};

// Обновление значения при изменении props
watch(
  () => props.modelValue,
  (newValue) => {
    internalValue.value = String(newValue).replace(/\D/g, '');
    formatValue();
  }
);

// Инициализация
onMounted(() => {
  internalValue.value = String(props.modelValue).replace(/\D/g, '');
  formatValue();
});
</script>

<style scoped>
.number-input {
  border-color: #CFCADF;
  color: #CFCADF;
}

.number-input:focus {
  color: initial;
}
</style>
