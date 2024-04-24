<template>
  <div class="return-form-container">
    <h2>Đăng Ký Trả Sách</h2>
    <form class="return-form" @submit.prevent="returnBook">
      <div class="form-group">
        <label class="form-label">Mã Độc giả:</label>
        <input class="form-control" v-model="borrowing.MaDocGia" required />
      </div>
      <div class="form-group">
        <label class="form-label">Mã Sách:</label>
        <input class="form-control" v-model="borrowing.MaSach" required />
      </div>
      <button class="btn btn-primary" type="submit">Đăng Ký</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      borrowing: {
        MaDocGia: '',
        MaSach: '',
      },
    };
  },
  methods: {
    async returnBook() {
      try {
        await axios.delete(`http://localhost:3000/borrowings/${this.borrowing.MaDocGia}/${this.borrowing.MaSach}`);
        alert('Trả Sách Thành Công');
        this.$emit('returned');
      } catch (error) {
        console.error(error);
        alert('Trả Sách Không Thành Công');
      }
    },
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@500&display=swap');
.return-form-container {
  font-family: 'Quicksand', sans-serif;
  max-width: 600px;
  margin: 0 auto;
  margin-top: 30px;
  padding: 2rem;
  background-color: #f5f5f5;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.return-form {
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
