<template>
  <div class="main-body">
    <div class="container">
      <div class="head-content">
        <img src="../assets/img/user.png" alt="" />
        <h2>รายชื่อนักเรียน</h2>
      </div>
      <div class="adddata-content">
        <div class="input-data-content-main">
          <div class="head-input-data">
            <h3>แก้ไขข้อมูลนักเรียน</h3>
          </div>

          <div class="row">
            <div class="col-md-12">
              <label for="room">ห้อง </label>
            </div>
            <div class="col-md-12">
              <select
                v-model="memberData.room"
                type="number"
                id="room"
                placeholder="กรุณาใส่เลขห้อง"
                :disabled="isLeader"
              >
                <option value="" disabled>กรุณาเลือกห้อง</option>
                <option v-for="(item, i) in 10" :key="i" :value="item">
                  {{ item }}
                </option>
              </select>
            </div>
            
              <!-- <form id="app" @submit="checkForm" action="/something" method="post"><p v-if="errors.length">
    <b>Please correct the following error(s):</b>
    <ul>
      <li v-for="error in errors" :key="error">{{ error }}</li>
    </ul>
  </p>
  </form> -->
              
            
            <div class="col-md-12"><label for="number">เลขที่</label></div>
            <div class="col-md-12">
              <input
                v-model="memberData.rank"
                type="number"
                id="number"
                placeholder="กรุณาใส่เลขที่"
              />
            </div>
            <div class="col-md-12"><label for="fristname">ชื่อ </label></div>
            <div class="col-md-12">
              <input
                v-model="memberData.first_name"
                type="text"
                id="fristname"
                placeholder="กรุณาใส่ชื่อ"
                @keypress="isLetter"
              />
            </div>
            <div class="col-md-12"><label for="lastname">นามสกุล</label></div>
            <div class="col-md-12">
              <input
                v-model="memberData.last_name"
                type="text"
                id="lastname"
                placeholder="กรุณาใส่นามสกุล"
                @keypress="isLetter"
              />
            </div>
            <div class="col-md-12">
              <label for="rank">ตำเเหน่งในห้อง</label>
            </div>
            <div class="col-md-12">
              
              <select
                v-model="memberData.member_type"
                type="text"
                id="rank"
                placeholder="กรุณาใส่ตำแหน่งนักเรียน"
              >
                <option value="" disabled>กรุณาเลือกห้อง</option>
                <option value="leader">leader</option>
                <option value="member">member</option>
                <option value="ast">ast</option>
              </select>
            </div>
            <div class="col-md-12"><label for="address">ที่อยู่</label></div>
            <div class="col-md-12">
              <input
                v-model="memberData.address"
                type="text"
                id="address"
                placeholder="กรุณาใส่ที่อยู่"
              />
            </div>
          </div>
          <div class="button-add-data-main">
            <button
              type="button"
              class="button-add-data btn btn-success"
              @click="createMember"
              v-if="isEdit"
            >
              เพิ่มข้อมูลนักเรียน
            </button>
            <button
              type="button"
              class="button-add-data btn btn-dark"
              @click="editMember"
              v-if="isEdit"
            >
              แก้ไขข้อมูลนักเรียน
            </button>
            <button
              type="button"
              class="button-add-data btn btn-success"
              @click="createMember"
              v-else
            >
              เพิ่มข้อมูลนักเรียน
            </button>
          </div>
        </div>
      </div>

      <div class="head-profile-student">
        <table>
          <tr class="head-table-content">
            <th>ห้อง</th>
            <th>เลขที่</th>
            <th>เลขประจำตัว</th>
            <th>ชื่อ - นามสกุล</th>
            <th>ตำเเหน่งในห้อง</th>
            <th>ที่อยู่</th>
            <th>แก้ไข/ลบ</th>
          </tr>
          <tr v-for="(item, i) in getdataStudent" :key="i">
            <td>{{ item.room }}</td>
            <td>{{ item.rank }}</td>
            <td>{{ item.id }}</td>
            <td>{{ item.first_name }} {{ item.last_name }}</td>
            <td>{{ item.member_type }}</td>
            <td>{{ item.address }}</td>
            <td class="button-edit">
              <button
                type="button"
                class="btn btn-dark"
                @click="editDataStudent(item)"
              >
                แก้ไข
              </button>
              <button
                type="button"
                class="btn btn-danger"
                @click="deleteProfile(item.id)"
                v-if="item.member_type !== 'leader'"
              >
                ลบ
              </button>
            </td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      getdataStudent: [],
      isEdit: false,
      errors:[],
      memberData: {
        first_name: "",
        last_name: "",
        member_type: "",
        rank: "",
        address: "",
        room: "",
      },
    };
  },
  mounted() {
    this.getData();
  },
  methods: {
    async getData() {
      const response = await axios.get(
        "https://647efbeec246f166da8fd1bd.mockapi.io/api/student/member"
      );
      this.getdataStudent = response.data;
      // console.log("getdataStudent", this.getdataStudent[0]);
    },
    async deleteProfile(id) {
      const student = this.getdataStudent.find((item) => item.id === id);
      if (student.member_type === "leader") {
        alert('Cannot delete student with a "leader" role.');
        return;
      }
      const res = await axios.delete(
        `https://647efbeec246f166da8fd1bd.mockapi.io/api/student/member/${id}`
      );
      this.getData();
    },
    async createMember() {
      if (!this.validateForm()) {
    alert("คุณใส่ข้อมูลไม่ครบ");
    return;
  }
      const res = await axios.post(
        "https://647efbeec246f166da8fd1bd.mockapi.io/api/student/member",
        this.memberData
        
      );
      this.getData();
  this.resetForm();
      this.memberData = {
        first_name: "",
        last_name: "",
        member_type: "",
        rank: "",
        address: "",
        room: "",
      }
      this.getData();
    },
    async editMember() {
      if (!this.validateForm()) {
    alert("คุณใส่ข้อมูลไม่ครบ");
    return;
  }

      let payload = { ...this.memberData };
      payload.id = Number(payload.id);
      const res = await axios.put(
        `https://647efbeec246f166da8fd1bd.mockapi.io/api/student/member/${this.memberData.id} `,
        payload
      );
      this.memberData = {
        first_name: "",
        last_name: "",
        member_type: "",
        rank: "",
        address: "",
        room: "",
      }
      this.getData();
      this.resetForm();
    },
    validateForm() {
  const { room, rank, first_name, last_name } = this.memberData;
  return room && rank && first_name && last_name;
},

resetForm() {
  this.memberData = {
    first_name: "",
    last_name: "",
    member_type: "",
    rank: "",
    address: "",
    room: "",
  };
},
    editDataStudent(item) {
      this.memberData = { ...item };
      this.isEdit = true;
    },
    isLetter(e) {
      let char = String.fromCharCode(e.keyCode); // Get the character
      if (/^[A-Za-z]+$/.test(char)) return true; // Match with regex
      else e.preventDefault(); // If not match, don't add to input text
    },
    // checkForm:function(e) {
    //   if(this.memberData) return true;
    //   this.errors = [];
    //   if(!this.memberData.first_name) this.errors.push("Name required.");
    //   if(!this.memberData.last_name) this.errors.push("lastname required.");
    //   if(!this.memberData.member_type) this.errors.push("rank required.");
    //   if(!this.memberData.rank) this.errors.push("Number required.");
    //   if(!this.memberData.address) this.errors.push("Address required.");
    //   if(!this.memberData.room) this.errors.push("Room required.");
      
    //   e.preventDefault();
    // },
  },
  computed: {
    isLeader() {
      return this.memberData.member_type === "leader";
    },
  },
};
</script>

<style lang="scss">
@media only screen and (min-width: 768px) and (max-width: 1024px) {
  .input-data-content-main {
    width: 60% !important;
  }
  .head-profile-student {
    table {
      .button-edit {
        button {
          width: 40px !important;
          font-size: 9px;
        }
      }
      th {
        font-size: 14px !important;
      }
      td {
        font-size: 12px;
      }
    }
  }
}
@media only screen and (max-width: 600px) {
  .head-content {
    justify-content: center;
  }
  .input-data-content-main {
    width: 100% !important;
    button {
      width: 40% !important;
      font-size: 12px;
      margin: 1rem 0.5rem 0 !important;
    }
  }
  .head-profile-student {
    table {
      .button-edit {
        button {
          width: 40px !important;
          font-size: 8px;
        }
      }
      th {
        font-size: 12px !important;
      }
      td {
        font-size: 10px;
      }
    }
  }
}
p,
ul {
  margin: 0;
}
.input-data-content-main {
  padding: 1rem;
  width: 40%;
  margin: auto;
  border: 1px solid black;
  background: azure;
  .head-input-data {
    text-align: center;
  }
  input {
    width: 100%;
    border: 1px solid;
  }
  select {
    width: 100%;
  }
  .button-add-data-main {
    text-align: center;
    button {
      margin: 1rem 1rem 0;
    }
  }
}
.head-content {
  display: flex;
  align-items: center;
  margin: 1rem 0;
  h2 {
    margin-left: 1rem;
  }
  img {
    width: 55px;
    height: auto;
  }
}
.head-profile-student {
  margin: 1rem 0;
}
table {
  background: azure;
  border: 1px solid black;
  text-align: center;
  border-collapse: collapse;
  table-layout: fixed;

  th {
    font-size: 18px;
    border: 1px solid black;
  }
  td {
    border: 1px solid black;
  }

  .head-table-content {
    background: aquamarine;
  }
  width: 100%;
  .button-edit {
    button {
      margin: 0 0.25rem;
      width: 55px;
    }
  }
}
</style>
