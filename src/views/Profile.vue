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
            <div class="col-md-12"><label>ห้อง </label></div>
            <div class="col-md-12">
              <input v-model="memberData.room" type="text" id="" />
            </div>
            <div class="col-md-12"><label>เลขที่</label></div>
            <div class="col-md-12">
              <input v-model="memberData.rank" type="text" id="" />
            </div>
            <div class="col-md-12"><label>ชื่อ </label></div>
            <div class="col-md-12">
              <input v-model="memberData.first_name" type="text" id="" />
            </div>
            <div class="col-md-12"><label>นามสกุล</label></div>
            <div class="col-md-12">
              <input v-model="memberData.last_name" type="text" id="" />
            </div>
            <div class="col-md-12"><label>ตำเเหน่งในห้อง</label></div>
            <div class="col-md-12">
              <input v-model="memberData.member_type" type="text" id="" />
            </div>
            <div class="col-md-12"><label for="address">ที่อยู่</label></div>
            <div class="col-md-12">
              <input v-model="memberData.address" type="text" id="address" />
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
      console.log("getdataStudent", this.getdataStudent[0]);
    },
    async deleteProfile(id) {
      const res = await axios.delete(
        `https://647efbeec246f166da8fd1bd.mockapi.io/api/student/member/${id}`
      );
      this.getData();
    },
    async createMember() {
      const res = await axios.post(
        "https://647efbeec246f166da8fd1bd.mockapi.io/api/student/member",
        this.memberData
      );
      this.getData();
    },
    async editMember() {
      let payload = { ...this.memberData };
      payload.id = Number(payload.id);
      const res = await axios.put(
        `https://647efbeec246f166da8fd1bd.mockapi.io/api/student/member/${this.memberData.id} `,
        payload
      );
      this.getData();
    },
    editDataStudent(item) {
      this.memberData = { ...item };
      this.isEdit = true;

      // console.log("itemdata",item )
    },
  },
};
</script>

<style lang="scss">
@media only screen and (max-width: 600px) {
  .head-content{
    justify-content: center;
  } 
  .input-data-content-main{
    width: 100% !important;
    button{
      width: 40% !important;
      font-size: 12px;
      margin: 1rem 0.5rem 0 !important;
      }
  }
  .head-profile-student{
    table{
    .button-edit {
      button{
        width: 40px !important;
        font-size: 8px;
      }
    }
    th{
      font-size: 12px !important;
    }
    td{
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
  .head-input-data{
    text-align: center;
  }
  input {
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
