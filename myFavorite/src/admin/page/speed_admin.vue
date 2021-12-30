<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-setting"></i> 管理</el-breadcrumb-item>
                <el-breadcrumb-item>会议室列表</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div>
            <el-table :data="data" border style="width: 90%" ref="multipleTable" >
                <el-table-column label="用户" prop="role" width="80px" ></el-table-column>
                <el-table-column label="会议室" prop="usrname" width="100px" ></el-table-column>
                <el-table-column label="会议室地址" prop="usripaddress" width="150px" ></el-table-column>
                <el-table-column label="带宽(/MB)" prop="bandwidth" width="120px" ></el-table-column>
                <el-table-column label="抖动(/ms)" prop="jitter" width="120px" ></el-table-column>
                <el-table-column label="延迟(/ms)" prop="delay" width="120px" ></el-table-column>
                <el-table-column label="创建时间" prop="ctime" width="250px" ></el-table-column>
                <el-table-column label="操作" width="120px" >
                    <template slot-scope="scope" width="100px">
                        <el-button type="text" @click="gotourl(scope.row)" >进入</el-button>
                        <!-- <el-button type="text" @click="openedit(scope.row)" >修改</el-button>
                        <el-button type="text" @click="del_fav(scope.row)" >删除</el-button> -->
                    </template>
                </el-table-column>
            </el-table>
            <!-- <el-button type="primary" @click="addspeed()" align="center">添加新记录</el-button> -->
        </div>

        <div>
            <el-table :data="data" border style="width: 90%" ref="multipleTable" >
                <el-table-column label="可用带宽" prop="role" width="80px" ></el-table-column>
                <el-table-column label="STUN/TURN" prop="usrname" width="100px" ></el-table-column>
                <el-table-column label="加密方式" prop="usripaddress" width="150px" ></el-table-column>
                <el-table-column label="IP地址" prop="bandwidth" width="120px" ></el-table-column>
                <el-table-column label="分辨率" prop="jitter" width="120px" ></el-table-column>
                <el-table-column label="传输数据" prop="delay" width="120px" ></el-table-column>
                <el-table-column label="创建时间" prop="ctime" width="250px" ></el-table-column>
                <el-table-column label="操作" width="120px" >
                    <template slot-scope="scope" width="100px">
                        <el-button type="text" @click="gotourl(scope.row)" >进入</el-button>
                        <!-- <el-button type="text" @click="openedit(scope.row)" >修改</el-button>
                        <el-button type="text" @click="del_fav(scope.row)" >删除</el-button> -->
                    </template>
                </el-table-column>
            </el-table>
            <!-- <el-button type="primary" @click="addspeed()" align="center">添加新记录</el-button> -->
        </div>

        <!-- <el-dialog
            width="30%"
            title="修改"
            :visible.sync="dialogFormVisibleed">
            <div>
                <el-form :model="form" :rules="rules" ref="form" label-width="150px">
                    <el-form-item label="网站名称" prop="wname">
                        <el-input v-model="form.wname" placeholder="请输入网站名称"></el-input>
                    </el-form-item>
                    <el-form-item label="网站地址" prop="password2">
                        <el-input v-model="form.wurl" placeholder="请输入网站地址"></el-input>
                    </el-form-item>
                    <el-form-item label="权限" prop="type">
                        <el-select v-model="form.type" placeholder="请设置权限">
                            <el-option
                                v-for="item in options"
                                :key="item.value"
                                :label="item.label"
                                :value="item.value">
                            </el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item style="text-align: center" >
                        <el-button @click="dialogFormVisibleed = false" >取消</el-button>
                        <el-button type="primary" @click="submit('form')">修改</el-button>
                    </el-form-item>
                </el-form>
            </div>
        </el-dialog> -->
        <!-- <el-dialog
            width="30%"
            title="添加收藏"
            :visible.sync="dialogFormVisibleed1">
            <div class="form-box">
                <el-form :model="form" :rules="rules" ref="form" label-width="150px">
                    <el-form-item label="网站名称" prop="wname">
                        <el-input v-model="form.wname" placeholder="请输入网站名称"></el-input>
                    </el-form-item>
                    <el-form-item label="网站地址" prop="wurl">
                        <el-input v-model="form.wurl" placeholder="请输入网站地址"></el-input>
                    </el-form-item>
                    <el-form-item label="权限" prop="type">
                        <el-select v-model="form.type" placeholder="请设置权限">
                            <el-option
                                v-for="item in options"
                                :key="item.value"
                                :label="item.label"
                                :value="item.value">
                            </el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item style="text-align: center" >
                        <el-button @click="dialogFormVisibleed1 = false" >取消</el-button>
                        <el-button type="primary" @click="addnew(form)">添加</el-button>
                    </el-form-item>
                </el-form>
            </div>
        </el-dialog> 
    </div> -->
    </div>
</template>

<script>
    import main from "../../main";
    export default {
        data: function(){
            return {
                data:[],
                // options:[{value: 0, label: "公开"},{value: 1, label: "私有"}],
                // dialogFormVisibleed:false,
                // dialogFormVisibleed1:false,
                form:{
                    role:'',
                    usrname:'',
                    usripaddress:'',
                    bandwidth:'',
                    jitter:'',
                    delay:'',
                    ctime:''
                }
            }
        },
        created(){
            if(localStorage.getItem('username')===""){
                this.$router.replace('/login')
            }else{this.init();}
        },
        methods:{
            init(){
                this.$http.post(main.url+"/speed/list",
                    // {'role': localStorage.getItem('role')},
                    {
                        headers: {'Content-Type':'application/x-www-form-urlencoded'},
                        emulateJSON: true
                    }).then(
                    success=>{this.data=success.data;}
                );
            }   
        },
            gotourl(row){ //进入指定的网站
                    this.$http.post(main.url+"/favorite/count",
                        {'id': row.id},
                        {
                            headers: {'Content-Type':'application/x-www-form-urlencoded'},
                            emulateJSON: true
                        }).then(
                        success=> {
                            window.open(row.wurl, "_blank");
                            this.init();
                        }
                    )
                }
    }
</script>
