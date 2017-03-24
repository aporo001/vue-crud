---

มาลองเล่น Vuejs กันเถอะๆ มาเล่นกันเถอะ
บนความนี้จะเป็นสอนการใช้ Vuejs เบื้องต้นตั้งแต่การติดตั้งไปจนถึงการเรียกใช้ api
ทุกวันนี้เทคโนโลยีของ Web ก็เปลี่ยนแปลงค่อนข้างไว เทรนในการทำเว็ปยุคนี้ก็จะเน้นไปทางการทำ Web ที่เป็น Single-Page Application จึงทำให้มีการพัฒนา Framework ที่จำมาช่วยให้การพัฒนา Web แบบ Single-Page Application ง่ายขึ้น สำหรับ Framework นี้เด่นๆในตอนนี้เลยในความคิดของผมก็จะมี 3 ตัว ได้แก่ Reactjs, Vuejs และ Angularjs โดยทั้ง 3 ตัวนี้ก็จะมีข้อดีข้อเสียที่แตกต่างกัน ซึ่งบนความนี้จะเป็นมาแนะนำเจ้า Vuejs ครับ


---

เริ่มต้นที่การติดตั้งก่อนเลยครับ
ติดตั้ง Vue-cli เจ้าตัวนี้จะมาช่วยเราในการสร้าง project

npm instll -g vue-cli
2. การสร้าง project โดย Vue-cli จะมา template มาให้เราเลือก 5 แบบ ได้แก่ browserify, browserify-simple, simple, webpack และ webpack-simple ในที่นี้จะเลือกใช้เป็น webpack
vue init webpack vue-crud
พอรันเสร็จก็จะมีการให้ตั้งค่าดังรูปข้างล่าง
3. เข้าไปให้ folder ของ project แล้วทำให้โหลด dependency ของ project ก่อน
cd vue-crud
npm install
เป็นอันเสร็จสิ้นขั้นตอนการติดตั้งครับ ~


---

เริ่มต้นด้วยการสร้าง Template
โครงสร้างของ projectเริ่มต้นด้วยการ run คำสั่ง
npm run dev
เมื่อ run คำสั่งเสร็จ browser จะเด้งหน้า web ของเราขึ้นมา
เข้าไปเพิ่ม css และ js ของ bootstap ที่ไฟล์ index.html ใน folder ของ project



สร้างไฟล์ Home.vue และ AddItem.vue ใน folder components






จาก 2 ไฟล์ด้านบนจะเห็นว่า จะต้องมี Tag <template></template> ครอบ html ของเราเสมอ
ต่อไปก็เข้าไปเพิ่ม Route ในไฟล์ router/index.js



เพิ่มเสร็จกด save จะได้ตามรูปด้านล่าง


---

สร้าง api
ทำ template เสร็จแล้วก็มาเริ่ม call api กันแต่ก่อนอื่นมาสร้าง api กันก่อนโดยใช้ json-server ขั้นตอนแรกก็ต้อง add package ของ json-server เข้ามาใน project ก่อน
npm install --save-dev json-server
หลังจากลงเสร็จก็สร้าง folder ชื่อว่า api ขึ้นมาใน project ของเรา
หลังจากสร้าง folder api แล้วก็เข้าไปที่ไฟล์ package.json แล้วเพิ่ม
"api": "json-server --watch api/db.json --port 5000"
เข้าไปตามภาพ
package.jsonหลังจากเพิ่มเสร็จก็เปิดหน้าจอ terminal หน้าใหม่ขึ้นมาแล้วเข้าไปที่ path ของ project แล้วรันคำสั่ง
npm run api
จะเห็นว่ามีการสร้างไฟล์ db.json ขึ้นมาใน folder api
ให้เข้าไปแก้ไฟล์ ตามด้านล่าง



หลังจากแก้เสร็จให้ลองรัน http://localhost:5000/photos ตอนนี้เราก็ได้ api ที่สามารถเรียกใช้ได้แล้ว มาเขียน code เรียกใช้ api กัน


---

เรียกใช้ Api
ก่อนอื่นต้องติดตั้ง package สำหรับเรียก api ก่อน
npm install --save vue-resource
หลังจากติดตั้งเสร็จให้เข้าไปที่ไฟล์ src/main.js เพื่อทำการเรียกใช้ vue-resource ใน project เราตามภาพด้านล่าง



จากนั้นไปที่ไฟล์ src/components/Home.vue ทำการ set ค่าเริ่มต้นให้ local state ที่จะเอามารับค่าที่ได้จาก api
export default {
  name: 'Home',
  data: function () {
    return {
      list: []
    }
  },
  ...
}
สร้าง method สำหรับเรียก api มา set ค่าให้กับ list และเรียกใช้ที่ created
const API_URL = 'http://localhost:5000/photos'
export default {
...,
  methods: {
    ...
    ,
    loadDate: function () {
      this.$http.get(API_URL)
        .then(response => {
          console.log(response)
          this.list = response.body
        }, () => {
          this.list = []
        })
    }
  },
  created: function () {
    this.loadDate()
  }
}
created เป็น event ของ Vue ที่จะทำหลังจากการมีการสร้าง component สำหรับลำดับการทำงานของ lifecycle ของ Vuejs สามารถเข้าไปดูเพิ่มเติมได้ที่นี่
หลังจากเพิ่ม method loadData เสร็จก็ไปแก้ในส่วนของ template ให้นำข้อมูลที่ได้ไปใช้
<template>
  <div>
    ...
    <div class="col-sm-6 col-md-4" v-for="item in list">
      <div class="thumbnail">
        <img :src="item.url" :alt="item.title">
        <div class="caption">
          <h3>{{ item.title }}</h3>
        </div>
      </div>
    </div>
</div>
</template>
หลังจากเสร็จจะได้ไฟล์ src/components/Home.vue ตามด้านล่าง



เสร็จแล้วลองดูว่ารันได้หรือเปล่า http://localhost:8080/#/
เป็นอันเสร็จสำหรับหน้า Home ที่ดึง api photo มา show ต่อไปลองทำหน้า create photo กันดูบ้างไปที่ไฟล์ src/components/FormItem.vue set ค่าเริ่มต้นและสร้าง method สำหรับรับการ submit form
<script>
  const API_URL = 'http://localhost:5000/photos'
  export default {
    name: 'FormItem',
    data: function () {
      return {
        id: null,
        title: null,
        url: null,
        thumbnailUrl: null
      }
    },
    methods: {
      onSubmit: function () {
        this.$http.post(API_URL,
          {title: this.title, url: this.url, thumbnailUrl:    this.thumbnailUrl}
        ).then(response => {
          this.backToHome()
        }, response => {
          this.backToHome()
        })
    },
    backToHome: function () {
      this.$router.push({name: 'Home'})
    }
  }
}
</script>
แก้ไข template ให้สามารถทำงานกับ funtion ด้านบน
<template>
<form class="form-horizontal" v-on:submit.prevent="onSubmit">
<fieldset>
<!-- Form Name -->
<legend>Photos</legend>
<!-- Text input-->
<div class="form-group">
<label class="col-md-4 control-label" for="title">Title</label>
<div class="col-md-4">
<input id="title" name="title" type="text" placeholder="title" class="form-control input-md" required="" v-model="title">
</div>
</div>
<!-- Text input-->
<div class="form-group">
<label class="col-md-4 control-label" for="thumbnailUrl">Thumbnail URL</label>
<div class="col-md-4">
<input id="thumbnailUrl" name="thumbnailUrl" type="text" placeholder="thumbnail url" class="form-control input-md" required="" v-model="thumbnailUrl">
</div>
</div>
<!-- Text input-->
<div class="form-group">
<label class="col-md-4 control-label" for="url">URL</label>
<div class="col-md-4">
<input id="url" name="url" type="text" placeholder="url" class="form-control input-md" v-model="url">
</div>
</div>
<!-- Button (Double) -->
<div class="form-group">
<label class="col-md-4 control-label" for="save"></label>
<div class="col-md-8">
<button id="save" name="save" class="btn btn-success">Save</button>
<button id="cancel" name="cancel" class="btn btn-danger" @click="backToHome">Cancel</button>
</div>
</div>
</fieldset>
</form>
</template>
หลังจากทำเสร็จแล้วจะได้ตามด้านล่าง



เสร็จแล้วครับสำหรับการทำ form ที่ทำงานกับ api จะเห็นว่า syntax ต่างๆของ vuejs ค่อนข้างเข้าใจง่าย ตรงไปตรงมา คนที่เคยเขียน javascript มาก่อนก็จะสามารถเริ่มใช้งานได้ไม่ยากครับ
จบแล้วครับ สำหรับบนความนี้หวังว่าจะเป็นประโยชน์ให้คนที่เข้ามาอ่าน สามารถเริ่มต้นการใช้งาน Vuejs ได้ง่ายขึ้น ขอบคุณครับ~~
Github: link
credit: https://vuejs.org/