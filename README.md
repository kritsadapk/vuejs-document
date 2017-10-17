# vue-js-สรุปแบบ-กากๆ 
Hello.vue 
> โครงสร้าง Vue

``` bash
<template>
	<div>
	</div>
</template>

<style scope>
</style>

<script>
	Export default{
		Props:[‘name’]
}
</script>
```
> Directive 

```
v-model เรียกใช้ชื่อตัวแปรต่างๆ  เช่นเอาไปใช้กับ input 
v-text, v-html การแสดงผล text / html ออกบนหน้าจอ หรือจะใช้ {{ //variable }} ก็ได้
v-bind  [หน้าถัดไป]
v-on [หน้าถัดไป]
v-if เช็คเงือนไขเช่น <p v-if=”x=0”></p> 
v-else 
v-else-if 

<p v-if=”x>0”></p>
<p v-else-if=”x<0”></p>
<p v-else></p>

v-show เช็คเงือนไขเช่น <p v-show=”x=0”></p> จะสร้าง element รอไว้ก่อน แต่ hidden ไว้เท่านั้น (display none)
v-for การวนลูปออกมา เช่น วน tr วน li วน element เป็นต้น
 ```

> ตัวอย่าง การใช้ for 

 ```
<templet>
  <p v-for="x in list" :key="x"> {{x}} </p>
</templet>
<script>
export default {
 data() {
   return {
     list: [1,2,3,4,5]
   }
 }
}
</script>

... ต่อ
v-on:click=”xxx”  @click=””
event ต่างๆ สามารถใส่เข้าไปได้เช่น click, blur, change, etc.

v-bind ตัวอย่างง่ายๆ <img :src=”link”> เมื่อใดที่ตัวแปร link เปลี่ยน รูปก็จะเปลี่ยนทันที

```

> Cycel 

```
components: {
   //คือการ เอา component จากที่อื่นมาใช้งาน โดย Import มาไว้แล้วเรียกใช้ที่นี่
}, 
 
methods: {
  // คือการเรียกใช้งานฟังก์ชั่นต่างๆ เช่นคลิ๊กแล้วให้ทำอะไร รวมไปถึงการหา service
},

props: [] คือ Pasting Data การรับค่า property จากแม่ หรือจากหน้าที่เรียกใช้ได้ เช่น 

::ตัวอย่าง PROPS::

|app.vue (หน้านี้จะเรียกใช้ hello.vue)|
    <template>
      <div>
        <hello message=”name”></hello>  
         //ถ้าใส่แบบนี้  <hello :message=”name”></hello> คือ v-bind สามารับค่า input หรือการกระทำใดๆ ส่งต่อไปอีกหน้าได้เลย เรียกสั้นๆว่า Dynamic Props
      </div>
    </template>
    
    Import Hello from ‘./components/Hello’
    <script>
      Components: { Hello },
    </script>

|hello.vue|
    <template>
      <div>
        Hi, {{message}}
      </div>
    </template>
    
    <script>
        export default {
         props: ['message']  //ตัวแปรนี้รับค่ามากจากแม่จ้า
        }
    </script>

- นอกจากจะส่ง prop เข้าไปในตัวแปรใดๆ แล้ว เราสามารถส่งเป็น obj ได้อีกด้วยนะ
- เมื่อเรารับค่ามาแล้ว เก็บไว้ใน props เราก็สามารถนำค่านั้นๆ มา compute ก่อนที่จะ render ได้อีกนะโดยเพิ่ม 
    Computed: {
        
    }
- Props สามารถนำไปใช้ Validate data ได้ด้วย
    props: {
      name: {
        required: true
      }
    }

--- 
computed: { 
  //การที่เราประกาศตัวแปรบางอย่าง ที่มัน depend ตามค่านั้น ๆ เช่นนำ object มา filter ก่อนจะส่งไปแสดงผล
},

data () { 
 return {
   //object ของข้อมูลเริ่มต้นที่จะเอามาใช้งาน
 }
}

Watch: {
// ไว้สำหรับตรวจดูการเปลี่ยนแปลงในตัวแปรหนึ่งๆ 
   var1(value_in_var1) {
      //จะทำอะไรกับค่าในตัวแปร var1  
   }
} 

การใช้ .sync  //กำลังหาข้อมูล

```
> nextTick

```


Vue nextTick :: 0.47.09 VDO
สำคัญมากๆ  
คำสั่งนี้คล้ายๆ กับ settimeOut เช่น เอาฟังก์ชั่นเรียกทีหลังจากฟังชั่นปัจจุบัน
this.rating = 5  //เมื่อ rating = 5 แล้วค่อยไปเรียกฟังก์ชั่น rating()
// DOM ยังไม่ได้อัพเดต 
this.$nextTick(() => {
  $(this.$refs.rating).rating() //DOM อัพเดต  
}


```

> Lifecycle 

```
beforeCreate: {  
  //SSR = Service side rendering // ส่วนนี้ควรดึงค่าจาก api เรียกแรกสุด ไม่ค่อยได้ใช้ รันก่อนสร้างทุกๆอย่าง
} 

created () { 
// หลังจากที่ component โดยสร้างเสร็จแล้ว จะ call ตัวนี้ แต่จะยังไม่ mount ลง html ดังนั้นแล้ว ตรงนี้ควรที่จะดึง data จาก api //SSR = Service side rendering // ส่วนนี้ควรดึงค่าจาก api 
// ไม่สามารถใช่ $el , $refs ได้ 
}  

mounted () { 
// component เข้าไปใน DOM แล้ว ตรงนี้สามารถเรียกทำงานกับ DOM ได้ สามารถใช้ jquery เอาค่าจาก DOM ได้ สามารถใช้ $el, $refs ได้
$el คือค่าที่อยู่ Template ไม่เชื่อ ลอง console.log(this.$el) ดูครับ 
$refs คืออะไร ?? มันก็เหมือนการตั้ง id ให้ element อ้างถึง DOM 
      <p ref=”text”></p>
***ถ้าใช้ jquery แล้วประกาศใช้หา DOM ใน mounted แล้วพัง ลองใส่ this.$nextTick(()=>{ /*CODE*/}) เข้าไป
}

beforeUpdate () { 
 เปลี่ยนค่า data ก่อนที่จะ update โดยที่จะไม่เกิด infinite loop
}  

updated () { 
 ไม่ควรเขียนคำสั่งเปลี่ยนค่าตัวแปร
ควรใช้ทำงานเกี่ยวกับ DOM เช่น เรียกใช้ jquery เช่น เพิ่ม class เหมือนกันกับ mounted 
}  

beforeDestroy () { 
เรียกใช้ก่อนที่จะ destroy เช่นก่อนที่จะเปลี่ยนหน้า...ก็จะทำตรงนี้ก่อน  หรือลบค่าต่างๆ ก่อน
}  

destroy () { 
เมื่อลบทุกอย่างเสร็จก็จะมาทำฟังก์ชั่นนี้
}  
Ref:: https://vuejs.org/v2/api/#mounted 


```

> Tips[1] Add jQuery into the Project

```
หากต้องการใช้ jQuery ในโปรเจค 
1. npm i jquery –save
2. เปิดไฟล์ build/webpack.base.conf.js 
3. แก้ไฟล์นี้ โดยเพิ่ม 
    Const webpack = require(‘webpack’) 
	//ลงมาล่าสุดหลัง vue:{}
	plugins: [
  new webpack.ProvidePlugin({
    ‘window.$’ : ‘jquery’,
     ‘$’ : ‘jquery’,
     ‘jQuery’’ : ‘jquery’,
  })
]
4. ไปแก้ในไฟล์ .eslintrc.js 
เพิ่ม  ,globals :{ 
   		$ : true
}
Script loader :: look on vdo 2 : 1.25.00


```

> Router

```

npm i vue-rounter 
import VueRouter from ‘vue-router’ 

Vue.use(VueRouter) 
หากเราลง Library ใหม่ของ Vue ต้องใช้ Vue.use เพื่อให้ Vue หากันจนเจอ...

... ตัวอย่าง code

const router = new VueRouter({
  routes: [
            mode: ‘history’, //ใส่ไว้เพื่อไม่ให้มี http://loooo/#/home 
  	{path :’/’, component: App}   //import component ของแต่ละอันมาด้วยนะ
              {path : ‘*’ redirect “/”}
  ]
})
---
อ่อ สร้างโฉ้โล้ได้ด้วยนะ – สร้าง name ไว้สำหรับอ้างถึงได้ด้วย โดยใส่ name: 'Name' เวลาอ้างก็ใส่ 
<router-link tag="a" :to="{name: 'History'}">History</router-link>

--- 

สร้างลิงค์ยังไงงะ
ใน App.vue 
    <router-link to: '/'> Home </router-link> 
    <router-link to:”/about”> About </router-link>
สามารถเปลี่ยน tag ได้นะ 
    <router-link to:”/about” tag=”button”> About </router-link>
อ่อ แล้วกดให้มา Render ละ 
	<router-view></router-view> 

TIPS : https://github.com/vuejs-templates/pwa PWA 


```
 


 
