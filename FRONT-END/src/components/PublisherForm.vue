<template>
  <div class="publisher-form-container">
    <h2>{{ isEditMode ? 'Chỉnh Sửa NXB' : 'Thêm NXB' }}</h2>
    <form class="publisher-form" @submit.prevent="handleSubmit">
      <div class="form-group">
        <label class="form-label">Mã NXB:</label>
        <input class="form-control" v-model="publisher.MaNXB" required />
      </div>
      <div class="form-group">
        <label class="form-label">Tên NXB:</label>
        <input class="form-control" v-model="publisher.TenNXB" required />
      </div>
      <div class="form-group">
        <label class="form-label">Địa Chỉ:</label>
        <input class="form-control" v-model="publisher.DiaChi" required />
      </div>
      <button class="btn btn-primary" type="submit">{{ isEditMode ? 'Cập Nhật NXB' : 'Thêm NXB' }}</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: {
    initialPublisher: {
      type: Object,
      default: () => ({
        MaNXB: '',
        TenNXB: '',
        DiaChi: '',
      }),
    },
  },
  data() {
    return {
      publisher: { ...this.initialPublisher },
      isEditMode: false,
    };
  },
  watch: {
    initialPublisher(newVal) {
      this.publisher = { ...newVal };
      this.isEditMode = !!newVal.MaNXB;
    },
  },
  methods: {
    async handleSubmit() {
      if (this.isEditMode) {
        await this.updatePublisher();
      } else {
        await this.addPublisher();
      }
    },
    async addPublisher() {
      try {
        await axios.post('http://localhost:3000/publishers', this.publisher);
        alert('Thêm Nhà Xuất Bản Thành Công');
        this.$emit('publisherAdded');
      } catch (error) {
        console.error(error);
        alert('Thêm Nhà Xuất Bản Không Thành Công');
      }
    },
    async updatePublisher() {
      try {
        await axios.put(`http://localhost:3000/publishers/${this.publisher.MaNXB}`, this.publisher);
        alert('Cập Nhật Nhà Xuất Bản Thành Công');
        this.$emit('publisherUpdated');
      } catch (error) {
        console.error(error);
        alert('Cập Nhật Nhà Xuất Bản Không Thành Công');
      }
    },
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@500&display=swap');
.publisher-form-container {
  font-family: 'Quicksand', sans-serif;
  max-width: 600px;
  margin: 0 auto;
  padding: 2rem;
  background-color: #f5f5f5;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.publisher-form {
  display: grid;
  grid-template-columns: 1fr;
  grid-gap: 1rem;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-label {
  font-weight: bold;
  margin-bottom: 0.5rem;
}

.form-control {
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.btn {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}

.btn-primary {
  background-color: #007bff;
  color: #fff;
}

.btn-primary:hover {
  background-color: #0056b3;
}
</style>
