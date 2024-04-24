<template>
  <div class="reader-form-container">
    <h2>{{ isEditMode ? 'Chỉnh Sửa Độc Giả' : 'Thêm Độc Giả' }}</h2>
    <form class="reader-form" @submit.prevent="handleSubmit">
      <div class="form-group">
        <label class="form-label">Mã Độc Giả:</label>
        <input class="form-control" v-model="reader.MaDocGia" required />
      </div>
      <div class="form-group">
        <label class="form-label">Họ Lót:</label>
        <input class="form-control" v-model="reader.HoLot" required />
      </div>
      <div class="form-group">
        <label class="form-label">Tên:</label>
        <input class="form-control" v-model="reader.Ten" required />
      </div>
      <div class="form-group">
        <label class="form-label">Ngày Sinh:</label>
        <input class="form-control" type="date" v-model="reader.NgaySinh" required />
      </div>
      <div class="form-group">
        <label class="form-label">Phái:</label>
        <select class="form-control" v-model="reader.Phai" required>
          <option value="Nam">Nam</option>
          <option value="Nu">Nữ</option>
        </select>
      </div>
      <div class="form-group">
        <label class="form-label">Địa Chỉ:</label>
        <input class="form-control" v-model="reader.DiaChi" required />
      </div>
      <div class="form-group">
        <label class="form-label">Điện Thoại:</label>
        <input class="form-control" v-model="reader.DienThoai" required />
      </div>
      <button class="btn btn-primary" type="submit">{{ isEditMode ? 'Cập Nhật' : 'Thêm Độc Giả' }}</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: {
    initialReader: {
      type: Object,
      default: () => ({
        MaDocGia: '',
        HoLot: '',
        Ten: '',
        NgaySinh: '',
        Phai: '',
        DiaChi: '',
        DienThoai: '',
      }),
    },
  },
  data() {
    return {
      reader: { ...this.initialReader },
      isEditMode: false,
    };
  },
  watch: {
    initialReader(newVal) {
      this.reader = { ...newVal };
      this.isEditMode = !!newVal.MaDocGia;
    },
  },
  methods: {
    async handleSubmit() {
      if (this.isEditMode) {
        await this.updateReader();
      } else {
        await this.addReader();
      }
    },
    async addReader() {
      try {
        await axios.post('http://localhost:3000/readers', this.reader);
        alert('Thêm Độc Giả Thành Công');
        this.$emit('readerAdded');
      } catch (error) {
        console.error(error);
        alert('Lỗi Thêm Độc Giả');
      }
    },
    async updateReader() {
      try {
        await axios.put(`http://localhost:3000/readers/${this.reader.MaDocGia}`, this.reader);
        alert('Cập Nhật Độc Giả Thành Công');
        this.$emit('readerUpdated');
      } catch (error) {
        console.error(error);
        alert('Cập Nhật Độc Giả Thất Bại');
      }
    },
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@500&display=swap');
.reader-form-container {
  font-family: 'Quicksand', sans-serif;
  max-width: 600px;
  margin: 0 auto;
  padding: 2rem;
  background-color: #f5f5f5;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.reader-form {
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
