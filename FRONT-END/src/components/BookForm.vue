<template>
  <div class="book-form-container">
    <h2>{{ isEditMode ? 'Edit Book' : 'Thêm Sách Mới' }}</h2>
    <form class="book-form" @submit.prevent="handleSubmit">
      <div class="form-group">
        <label class="form-label">Mã Sách:</label>
        <input class="form-control" v-model="book.MaSach" required />
      </div>
      <div class="form-group">
        <label class="form-label">Tên Sách:</label>
        <input class="form-control" v-model="book.TenSach" required />
      </div>
      <div class="form-group">
        <label class="form-label">Đơn Giá:</label>
        <input class="form-control" type="number" v-model="book.DonGia" required />
      </div>
      <div class="form-group">
        <label class="form-label">Số Quyển:</label>
        <input class="form-control" type="number" v-model="book.SoQuyen" required />
      </div>
      <div class="form-group">
        <label class="form-label">Năm Xuất Bản:</label>
        <input class="form-control" type="number" v-model="book.NamXuatBan" required />
      </div>
      <div class="form-group">
        <label class="form-label">Mã NXB:</label>
        <input class="form-control" v-model="book.MaNXB" required />
      </div>
      <div class="form-group">
        <label class="form-label">Nguồn Gốc:</label>
        <input class="form-control" v-model="book.NguonGoc" required />
      </div>
      <div class="form-group">
        <label class="form-label">Tác Giả:</label>
        <input class="form-control" v-model="book.TacGia" required />
      </div>
      <button class="btn btn-primary" type="submit">{{ isEditMode ? 'Cập Nhật Sách' : 'Thêm Sách' }}</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: {
    initialBook: {
      type: Object,
      default: () => ({
        MaSach: '',
        TenSach: '',
        DonGia: 0,
        SoQuyen: 0,
        NamXuatBan: 0,
        MaNXB: '',
        NguonGoc: '',
        TacGia: '',
      }),
    },
  },
  data() {
    return {
      book: { ...this.initialBook },
      isEditMode: false,
    };
  },
  watch: {
    initialBook(newVal) {
      this.book = { ...newVal };
      this.isEditMode = !!newVal.MaSach;
    },
  },
  methods: {
    async handleSubmit() {
      if (this.isEditMode) {
        await this.updateBook();
      } else {
        await this.addBook();
      }
    },
    async addBook() {
      try {
        await axios.post('http://localhost:3000/books', this.book);
        alert('Thêm Sách Thành Công');
        this.$emit('bookAdded');
      } catch (error) {
        console.error(error);
        alert('Thêm Sách Không Thành Công');
      }
    },
    async updateBook() {
      try {
        await axios.put(`http://localhost:3000/books/${this.book.MaSach}`, this.book);
        alert('Cập Nhật Sách Thành Công');
        this.$emit('bookUpdated');
      } catch (error) {
        console.error(error);
        alert('Cập Nhật Sách Không Thành Công');
      }
    },
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@500&display=swap');

.book-form-container {
  font-family: 'Quicksand', sans-serif;
  max-width: 600px;
  margin: 0 auto;
  padding: 2rem;
  background-color: #f5f5f5;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.book-form {
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