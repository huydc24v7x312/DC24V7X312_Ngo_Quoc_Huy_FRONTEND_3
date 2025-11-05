<script>
export default {
    name: "ContactCard",
    props: {
        contact: { type: Object, required: true },
    },
    data() {
        return {
            hobbies: ["Đọc sách", "Du lịch", "Nghe nhạc", "Thể thao"],
            familyStatuses: ["Độc thân", "Đã kết hôn", "Ly hôn"],
            selectedHobbies: [],
            selectedStatus: "",
        };
    },
};
</script>

<template>
<div>
    <div class="p-1">
        <strong>Tên:</strong>
        {{ contact.name }}
    </div>
    <div class="p-1">
        <strong>E-mail:</strong>
        {{ contact.email }}
    </div>
    <div class="p-1">
        <strong>Địa chỉ:</strong>
        {{ contact.address }}
    </div>
    <div class="p-1">
        <strong>Điện thoại:</strong>
        {{ contact.phone }}
    </div>
    <div class="p-1">
        <strong>Liên hệ yêu thích:&nbsp;</strong>
        <i v-if="contact.favorite" class="fas fa-check-circle"></i>
        <i v-else class="fas fa-times-circle"></i>
    </div>
      <!-- Sở thích (checkbox, chỉ xem) -->
    <div class="p-1">
      <strong>Sở thích:</strong><br />
      <div class="form-check form-check-inline" v-for="(hobby, index) in hobbies" :key="index">
        <input
          class="form-check-input"
          type="checkbox"
          :id="'hobby' + index"
          :checked="contact.hobbies && contact.hobbies.includes(hobby)"
          disabled
        />
        <label class="form-check-label" :for="'hobby' + index">{{ hobby }}</label>
      </div>
    </div>

    <!-- Tình trạng gia đình (radio, chỉ xem) -->
    <div class="p-1">
      <strong>Tình trạng gia đình:</strong><br />
      <div class="form-check form-check-inline" v-for="(status, index) in familyStatuses" :key="index">
        <input
          class="form-check-input"
          type="radio"
          name="familyStatus"
          :id="'status' + index"
          :checked="contact.familyStatus === status"
          disabled
        />
        <label class="form-check-label" :for="'status' + index">{{ status }}</label>
      </div>
    </div>
</div>
</template>