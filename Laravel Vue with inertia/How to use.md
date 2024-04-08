## Tips and trick
bagaimana jika v-model nya banyak?

Jika Anda memiliki banyak `v-model` yang ingin Anda gunakan dalam sebuah komponen Vue 3, Anda dapat mengelompokkannya dalam sebuah objek di dalam komponen parent dan mengirimkan objek tersebut sebagai prop ke komponen child. Di komponen child, Anda dapat mengakses nilai-nilai tersebut secara individu dan mengirimkan perubahan menggunakan event. Berikut adalah contoh bagaimana Anda dapat melakukannya:

<!-- ChildComponent.vue -->
```
<template>
  <div>
    <input :value="formData.name" @input="updateForm('name', $event.target.value)">
    <input :value="formData.email" @input="updateForm('email', $event.target.value)">
  </div>
</template>

<script>
export default {
  props: ['formData'],
  methods: {
    updateForm(key, value) {
      this.$emit('updateFormData', { ...this.formData, [key]: value });
    }
  }
};
</script>
```

<!-- ParentComponent.vue -->
```
<template>
  <div>
    <ChildComponent :formData="formData" @updateFormData="updateFormData" />
  </div>
</template>

<script>
import { ref } from 'vue';
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  setup() {
    const formData = ref({
      name: '',
      email: ''
      // tambahkan properti lain sesuai kebutuhan
    });

    const updateFormData = (data) => {
      formData.value = data;
    };

    return {
      formData,
      updateFormData
    };
  }
};
</script>
```

Dengan cara ini, Anda dapat mengelola banyak nilai `v-model` dalam sebuah objek di dalam komponen parent dan mengirimkan objek tersebut sebagai prop ke komponen child. Di komponen child, Anda kemudian dapat mengakses dan memperbarui nilai-nilai tersebut secara individu dan mengirimkan perubahan kembali ke komponen parent melalui event.