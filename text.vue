<template>
<el-container class="home_container">
    
    
    <!--头部区-->
  
  
  
  
  <el-header style="height:100px">
      <diV class="header_title">
        <img src="../img/drop.png"  id="home_header_logo">
        <span>污水处理及沼能分析利用大数据分析平台</span>
      </diV>
      <div>
        <img >
        <el-select class="header_select"  placeholder="用户操作"   >
            <el-popover
            placement="right"
            width="400"
            trigger="click"
            v-model="cvisible">
            <!--修改密码弹出内容-->
                <div class="ChangePasswordForm">
                    <h2>修改密码</h2>
                    <el-form :model="ruleForm" status-icon :rules="rules"  ref="ruleFormRef" label-width="100px">
                    <el-form-item label="旧密码">
                    <el-input v-model="put" ></el-input>
                    </el-form-item>
                    <el-form-item label="新密码" prop="pass">
                        <el-input v-model="ruleForm.pass" ></el-input>
                    </el-form-item>
                    <el-form-item label="确认新密码" prop="checkPass">
                        <el-input  v-model="ruleForm.checkPass" ></el-input>
                    </el-form-item>
                        <el-button style="float:right" type="primary" plain @click="change1">修改</el-button>
                        <el-button style="float:right" type="primary" plain @click="cvisible = false">取消</el-button>
                    </el-form>

                    <el-form style="text-align:right">
                    </el-form>
                </div> 
            <el-option slot="reference" value="1">修改密码</el-option>
            </el-popover>
            
            <el-popover
              placement="top"
              width="160"
              v-model="evisible">
              <p>确定退出？</p>
              <div style="text-align: right; margin: 0">
                          <el-button  @click="exit()">确定</el-button>
                <el-button  @click="evisible = false">取消</el-button>
      
              </div>
              <el-option slot="reference" value="2">退出</el-option>
            </el-popover>
        </el-select>
      </div>
  </el-header>
  <!--页面主体区-->
  <el-container>
    <!--侧边栏-->
    <el-aside width="300px">
        <el-menu router="index">
            <el-submenu index="1">
                <template slot="title">
                <i class="el-icon-camera"></i>
                <span>视频管理</span>
                </template>
                <el-menu-item-group>
                <el-menu-item index="video">视频查看</el-menu-item>
                </el-menu-item-group>
            </el-submenu>
            <el-submenu index="2">
                <template slot="title">
                <i class="el-icon-setting"></i>
                <span>设备管理</span>
                </template>
                <el-menu-item-group>
                <el-menu-item index="equipment">设备信息</el-menu-item>
                </el-menu-item-group>
            </el-submenu>
            <el-submenu index="3">
                <template slot="title">
                <i class="el-icon-s-data"></i>
                <span>数据查看</span>
                </template>
                <el-menu-item-group>
                <el-menu-item index="report">数据展示</el-menu-item>
                </el-menu-item-group>
            </el-submenu>
    </el-menu>
    </el-aside>
    <!--右侧内容主体-->
    <el-main>
        <div>
          <router-view></router-view>
        </div>        
    </el-main>
  </el-container>
</el-container>
</template>

<script>
import axios from "axios";
export default {
    data(){
      var validatePass = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入密码'));
        } else {
          if (this.ruleForm.checkPass !== '') {
            this.$refs.ruleForm.validateField('checkPass');
          }
          callback();
        }
      };
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value !== this.ruleForm.pass) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
        return {
            cvisible:false,
            evisible:false,
            put:'',
            ruleForm: {
            pass: '',
            checkPass: '',
            },
            rules: {
            pass: [
                { validator: validatePass, trigger: 'blur'}
            ],
            checkPass: [
                { validator: validatePass2, trigger: 'blur' }
            ]
            }
        }
    },
    methods:{
      change:function(){
        axios.put('http://101.133.174.112:8888/swg/user/modify',{
          "uid": 1,
          "password": "111111",
          "opassword": "123456"
        })
        .then(function(response){
          console.log(response)
        })
        .catch(function(error){
          console.log(error)
        })
      },
      exit:function(){
        console.log('11111111')
        window.sessionStorage.clear()
        this.$router.push('/login')
      } ,
      change1:function(){
         console.log('11111111')
        this.$router.push('/login')
      }
    }
}
</script>

<style>
.home_container{
    height: 100%;
}
.el-header{
    background-color:skyblue;
    position: relative;
    height: 75px;
    color: aliceblue;
}
.header_title{
    float: left;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
    font-size: 35px;
}
#home_header_logo{
    position: relative;
    width: 45px;
    height: 45px;
   right: 15px;

}
.header_select{
    left: 80%;
    padding-top: 40px;
}
.el-aside{
    background-color:white;
}
.el-main{
    background-color:white;
}
h2{
    text-align: center;
}
.ChangePasswordForm{
    width: 400px;
    height: 300px;
} 

</style>