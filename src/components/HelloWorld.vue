<template>
  <div class="hello">
    <div class="search-member">
      <div class="search-content">
        <input type="text" placeholder="请输入搜索内容" v-model="list">
        <div class="content" ref="listContent">
          <ul>
            <li class="content-list" v-for="(item, index) in searchData" :key="index" @click="addMember(index, item.name, item.url)">{{item.name}}--{{item.url}}</li>
          </ul>
        </div>
      </div>
    </div>
    <div class="add-display">
      <ul>
        <li v-for="(item, index) in member" :key="index">{{item.name}}<span class="cls" @click="deletes(index)">X</span></li>
      </ul>
      <button class="add-member" @click="lastmember()">add</button>
    </div>
    <div class="right-content">
      <ul>
        <li v-for="(item, index) in members" :key="index" class="lastshow">{{item.name}}-{{item.url}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import BScroll from 'better-scroll'
export default {
  name: 'HelloWorld',
  data () {
    return {
      listArr: [],
      list:'',
      member: [],
      members: []
    }
  },
  methods: {
    // show () {
    //   console.log('111')
    // },
    deletes (index) {
      // console.log(index)
      // this.member.remove(index)
      this.member.splice(index,1)
    },
    lastmember () {
      this.members = this.member
    },
    addMember (index, name, url) {
      console.log(this.member)
      // if (this.member.indexOf(index) < 0) {}
      // if (JSON.stringify(this.member)){}
      console.log(JSON.stringify(this.member))
      var obj = {
        name: name,
        url: url
        }
        if (JSON.stringify(this.member).indexOf(JSON.stringify(obj)) === -1) {
          this.member.push(obj)
        }
        // console.log(this.member)

    },
    _initScroll () {
      this.listScroll = new BScroll(this.$refs.listContent, {
        scrollbar: {
          fade: true,
          interactive: true
        }
      })
    }
  },
  created () {
    axios.get('../../static/data.json')
    .then(data => {
      this.listArr = data.data.links
      // console.log(this.listArr)
        this.$nextTick(() => {
          this._initScroll()
      })
    })
    .catch((err) => {
      console.log('err')
    })
  },
  computed: {
    // filterList: function () {
    //   return this.listArr.filter((item) => {
    //     return item.name.match(this.list)
    //   })
    // }
     searchData: function() {
      var search = this.list
      var searchVal = ''//搜索后的数据
      if (search) {
        searchVal = this.listArr.filter(function(product) {
          return Object.keys(product).some(function(key) {
            //搜索所有的内容
            return String(product[key]).indexOf(search) > -1
            //只搜索问题内容（某一个key）.toLowerCase()
            return String(product['questions']).indexOf(search) > -1
          })
        })
        return searchVal
      }
    }
  }
}
</script>

<style scoped>
.hello{
position: relative;
width: 100%;
height: 100%;
}
.search-member{
  overflow: hidden;
  max-width: 600px;
  margin: auto;
  border: 1px solid #d8d8d8;
  border-radius: 10px;
  padding: 20px;
}
.search-content{
  width: 500px;
  margin: auto;
}
input{
  width: 500px;
  height: 40px;
  border-radius: 5px;
  border: 1px solid blueviolet;
  outline: none;
  padding: 0 5px;
}
.content{
  position: relative;
  width: 500px;
  height: 200px;
  margin: 20px auto;
  overflow: hidden;
}
.content-list{
  height: 30px;
  line-height: 30px;
  border: 1px solid #333;
  padding-left:20px;
  color: orange;
  margin: 10px 0;
  cursor: pointer;
}
.add-member{
  position: absolute;
  bottom: 0;
  left: 200px;
  width: 100px;
  height: 40px;
  background: gold;
  color: #fff;
  outline: none;
  border-radius: 25px;
  cursor: pointer;
}
.add-display{
  width: 500px;
  height: 200px;
  border: 1px solid gray;
  border-radius: 10px;
  margin: 10px auto;
  position: relative;
}
.add-display li {
  position: relative;
  float: left;
  width: 80px;
  line-height: 30px;
  font-size: 14px;
  background: #333;
  color: #fff;
  border-radius: 5px;
  text-align: center;
  margin: 20px;
  cursor: pointer;
}
.cls{
  display: inline-block;
  width: 25px;
  position: absolute;
  font-size: 16px;
  right: -5px;
  top: -7px;
  color: #333;
}
.cls:hover{
  color: #fff;
}
.right-content{
  position: absolute;
  right: 0;
  top: 100px;
  width: 200px;
  height: 400px;
  background: gray;
  color: #fff;
  line-height: 40px;
  text-indent: 10px;
  font-size: 10px;
}
</style>
