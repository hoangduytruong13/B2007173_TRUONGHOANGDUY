<template>
  <div class="borrow-form-container">
    <h2> Đăng Ký Mượn Sách</h2>
    <form class="borrow-form" @submit.prevent="borrowBook">
      <div class="form-group">
        <label class="form-label">Mã Độc Giả:</label>
        <input class="form-control" v-model="borrowing.MaDocGia" required />
      </div>
      <div class="form-group">
        <label class="form-label">Mã Sách:</label>
        <input class="form-control" v-model="borrowing.MaSach" required />
      </div>
      <div class="form-group">
        <label class="form-label">Ngày Mượn:</label>
        <input class="form-control" type="date" v-model="borrowing.NgayMuon" required />
      </div>
      <div class="form-group">
        <label class="form-label">Ngày Trả:</label>
        <input class="form-control" type="date" v-model="borrowing.NgayTra" required />
      </div>
      <button class="btn btn-primary" type="submit">Đăng ký</button>
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
        NgayMuon: '',
        NgayTra: '',
      },
    };
  },
  methods: {
    async borrowBook() {
      try {
        await axios.post('http://localhost:3000/borrowings', this.borrowing);
        alert('Đăng Ký Mượn Sách Thành Công');
        this.$emit('bookBorrowed');
      } catch (error) {
        console.error(error);
        alert('Đăng Ký Mượn Sách Không Thành Công');
      }
    },
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@500&display=swap');
.borrow-form-container {
  font-family: 'Quicksand', sans-serif;
  max-width: 600px;
  margin: 0 auto;
  padding: 2rem;
  background-color: #f5f5f5;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.borrow-form {
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
