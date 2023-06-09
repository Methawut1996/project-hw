<template>
  <div class="main-body">
    <div class="container">
      <div class="head-content">
        <img src="../assets/img/user.png" alt="" />
        <h2>รายชื่อนักเรียน</h2>
      </div>

      <div class="head-profile-student">
        <ul>
          <li>ห้อง</li>
          <li>เลขที่</li>
          <li>เลขประจำตัว</li>
          <li>ชื่อ - นามสกุล</li>
          <li>ตำเเหน่งในห้อง</li>
          <li>ที่อยู่</li>
          <li>แก้ไข</li>
        </ul>
      </div>
      <div class="data-profile-student">
        <ul>
          <li>1</li>
          <li>1</li>
          <li>1409901217742</li>
          <li>มะลิ จำปาทอง</li>
          <li>-</li>
          <li>228 หมู่ 1 อ.ทรายทอง จ.ปัตานี</li>
          <li class="edit-button">
            <button type="button" class="button-edit btn btn-dark">
              แก้ไข
            </button>
            <button type="button" class="button-dalete btn btn-danger">
              ลบ
            </button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss">
@media only screen and (max-width: 600px) {
}
p,
ul {
  margin: 0;
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
  background: aliceblue;
  border-radius: 10px;
  ul {
    width: 100%;
    list-style-type: none;
    display: flex;
    padding: 0.5rem 1rem;
    align-items: center;
    li {
      font-size: 17px;
      width: 14.5%;
    }
  }
}
.data-profile-student {
  ul {
    width: 100%;
    list-style-type: none;
    display: flex;
    padding: 0.5rem 1rem;
    align-items: center;
    li {
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      width: 14.2%;
    }
  }
  .edit-button {
    display: flex;
    button {
      margin: 0 0.25rem;
      width: 60px;
    }
  }
}
</style>
