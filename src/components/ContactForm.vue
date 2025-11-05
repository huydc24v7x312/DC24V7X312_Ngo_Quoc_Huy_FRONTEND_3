<template>
  <Form @submit="submitContact" :validation-schema="contactFormSchema">
    <div class="form-group">
      <label for="name">Tên</label>
      <Field name="name" type="text" class="form-control" v-model="contactLocal.name" />
      <ErrorMessage name="name" class="error-feedback" />
    </div>

    <div class="form-group">
      <label for="email">E-mail</label>
      <Field name="email" type="email" class="form-control" v-model="contactLocal.email" />
      <ErrorMessage name="email" class="error-feedback" />
    </div>

    <div class="form-group">
      <label for="address">Địa chỉ</label>
      <Field name="address" type="text" class="form-control" v-model="contactLocal.address" />
      <ErrorMessage name="address" class="error-feedback" />
    </div>

    <div class="form-group">
      <label for="phone">Điện thoại</label>
      <Field name="phone" type="tel" class="form-control" v-model="contactLocal.phone" />
      <ErrorMessage name="phone" class="error-feedback" />
    </div>

    <div class="form-group form-check">
      <input name="favorite" type="checkbox" class="form-check-input" v-model="contactLocal.favorite" />
      <label for="favorite" class="form-check-label"><strong>Liên hệ yêu thích</strong></label>
    </div>

     <!-- Checkbox: Sở thích -->
      <div class="form-group mb-3">
        <label><strong>Sở thích:</strong></label><br />
        <div
          class="form-check form-check-inline"
          v-for="(hobby, index) in hobbies"
          :key="index"
        >
          <input
            class="form-check-input"
            type="checkbox"
            :id="'hobby' + index"
            v-model="contactLocal.hobbies"
            :value="hobby"
          />
          <label class="form-check-label" :for="'hobby' + index">{{ hobby }}</label>
        </div>
      </div>

      <!-- Radio: Tình trạng gia đình -->
      <div class="form-group mb-4">
        <label><strong>Tình trạng gia đình:</strong></label><br />
        <div
          class="form-check form-check-inline"
          v-for="(status, index) in familyStatuses"
          :key="index"
        >
          <input
            class="form-check-input"
            type="radio"
            name="familyStatus"
            :id="'status' + index"
            v-model="contactLocal.familyStatus"
            :value="status"
          />
          <label class="form-check-label" :for="'status' + index">{{ status }}</label>
        </div>
      </div>

    <div class="form-group">
      <button class="btn btn-primary">Lưu</button>
      <button v-if="contactLocal._id" type="button" class="ml-2 btn btn-danger" @click="deleteContact">Xóa</button>
      <button type="button" class="ml-2 btn btn-danger" @click="Cancel">Thoát</button>
    </div>
  </Form>
</template>

<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";

export default {
  components: { Form, Field, ErrorMessage },
  emits: ["submit:contact", "delete:contact"],
  props: { contact: { type: Object, required: true } },
  data() {
    const contactFormSchema = yup.object().shape({
      name: yup.string().required("Tên phải có giá trị.").min(2).max(50),
      email: yup.string().email("E-mail không đúng.").max(50),
      address: yup.string().max(100),
      phone: yup.string().matches(/((09|03|07|08|05)+([0-9]{8})\b)/g, "Số điện thoại không hợp lệ."),
    });
    return { 
        // Chúng ta sẽ không muốn hiệu chỉnh props, nên tạo biến cục bộ
        // contactLocal để liên kết với các input trên form
        contactLocal: {
          ...this.contact,
          hobbies: this.contact.hobbies || [],
          familyStatus: this.contact.familyStatus || "",
        },
        contactFormSchema,
        hobbies: ["Đọc sách", "Du lịch", "Nghe nhạc", "Thể thao"],
        familyStatuses: ["Độc thân", "Đã kết hôn", "Ly hôn"],
      };
  },
  methods: {
    submitContact() {
      this.$emit("submit:contact", this.contactLocal);
    },
    deleteContact() {
      this.$emit("delete:contact", this.contactLocal.id);
    },
    Cancel() {
      const reply = window.confirm("You have unsaved changes! Do you want to leave?");
      if (!reply) {
        // stay on the page if
        // user clicks 'Cancel'
        return false
        }
        else this.$router.push({ name: "contactbook" });
    },
  },
};
</script>

<style scoped>
@import "@/assets/form.css";
</style>
