<template>
  <div>
    <div class="datatable_row" id="frame_datatable">
      <div id="data_datatable">
        <div class="panel edit-panel">
          <div class="panel-heading">
            <a @click="goToList" class="small-hide-back hide-back"><span class="fa fa-angle-left"></span></a>{{isCreate ? '创建人员' : '修改人员'}}<a class="small-hide medium-hide button secondary small radius margin-l10 hide-back"><el-button @click="goToList" :disabled="userStatusSave"><span class="fa fa-angle-left"></span> 返回</el-button></a>
            <div class="absolute">
              <el-button type="primary"
                @click="saveUser"
                :loading="userStatusSave"
              >保存</el-button>
            </div>
          </div>
          <div class="panel-body">
            <div class="panel-tag">
              <div class="swiper-container" id="panel-tag">
                <div class="swiper-wrapper">
                  <div class="swiper-slide swiper-slide-active" id="wdgz">
                    <a class="active">基本信息</a>
                  </div>
                </div>
                <!-- Add Scroll Bar -->
                <div class="swiper-scrollbar">
                  <div class="swiper-scrollbar-drag"></div>
                </div>
              </div>
            </div>
            <div class="modcon clearfix">
              <div id="data_wdgz_con_1" style="display: block;">
                <div class="row">
                  <div class="large-12 medium-16 small-24 columns">
                    <div class="row margin-b10">
                      <div class="large-6  small-24 medium-6 columns"><label class="right small-no-float">状态：</label></div>
                      <div class="large-18 small-24 medium-18 columns">
                        <!--<el-radio class="radio" v-model="user.userStatus" label="0">禁用</el-radio>
                        <el-radio class="radio" v-model="user.userStatus" label="1">启用</el-radio>-->
                        <el-switch
                          v-model="userStatus"
                          on-text="启用"
                          off-text="禁用">
                        </el-switch>
                        <!-- <el-radio class="radio" v-model="user.userStatus" label="2">未激活</el-radio> -->
                      </div>
                    </div>
                    <div class="row margin-b10">
                      <div class="large-6 small-24 medium-6 columns"><label class="right small-no-float">头像：</label></div>
                      <div class="large-18 small-24 medium-18 columns">
                        <div class="transverse clearfix" style="line-height: 80px;">
                          <div class="img">
                            <a><img src="~assets/default/images/av1.jpg" alt="头像"></a>
                          </div>
                          <a class="font-s14 margin-l10 h-underline" data-reveal-id="avatar"><span class="fa fa-upload margin-r4"></span>修改头像</a>
                        </div>
                      </div>
                    </div>
                    <div class="row margin-b10">
                      <div class="large-6 small-24 medium-6 columns"><label class="right small-no-float">姓名：</label></div>
                      <div class="large-18 small-24 medium-18 columns">
                        <!-- <input type="text" name="name" value="林中有"> -->
                        <el-input placeholder="请输入姓名" v-model="user.userName"></el-input>
                      </div>
                    </div>
                    <div class="row margin-b10">
                      <div class="large-6  small-24 medium-6 columns"><label class="right small-no-float">登录名：</label></div>
                      <div class="large-18 small-24 medium-18 columns">
                        <el-input placeholder="请输入内容" v-model="user.loginName"></el-input>
                      </div>
                    </div>
                    <!-- <div class="row margin-b10">
                      <div class="large-6  small-24 medium-6 columns"><label class="right small-no-float">密码：</label></div>
                      <div class="large-18 small-24 medium-18 columns">
                        <el-input placeholder="请输入内容" v-model="user.password"></el-input>
                      </div>
                    </div> -->
                    <div class="row margin-b10">
                      <div class="large-6  small-24 medium-6 columns"><label class="right small-no-float">性别：</label></div>
                      <div class="large-18 small-24 medium-18 columns">
                        <el-radio class="radio" v-model="user.sex" label="1">男</el-radio>
                        <el-radio class="radio" v-model="user.sex" label="2">女</el-radio>
                      </div>
                    </div>
                    <div class="row margin-b10">
                      <div class="large-6  small-24 medium-6 columns"><label class="right small-no-float">手机号码: </label></div>
                      <div class="large-18 small-24 medium-18 columns">
                        <el-input placeholder="请输入手机号码" v-model="user.cellphone"></el-input>
                      </div>
                    </div>
                    <div class="row margin-b10">
                      <div class="large-6  small-24 medium-6 columns"><label class="right small-no-float">邮箱:</label></div>
                      <div class="large-18 small-24 medium-18 columns">
                        <el-input placeholder="请输入邮箱" v-model="user.email"></el-input>
                      </div>
                    </div>
                    <div class="row margin-b10">
                      <div class="large-6  small-24 medium-6 columns"><label class="right small-no-float">角色:</label></div>
                      <div class="large-18 small-24 medium-18 columns">
                        <page-select
                          v-model="userRoles"
                          multiple
                          restfulUrl="/upms/roles"
                          restfulListQuery="roleType=0"
                          :getItemTitle="item => { return item.roleName }"
                        >
                        </page-select>
                      </div>
                    </div>
                    <div class="row">
                      <div class="large-6 small-24 medium-6 columns"><label class="right small-no-float">所在部门:</label></div>
                      <div class="large-18 small-24 medium-18 columns">
                        <label class="inline-block"><span class="fa fa-circle font-info"></span>{{dept.roleName || currDept.roleName}}</label><a @click="userMovePanelShow = true" class="font-s14 margin-l10 h-underline" data-button-id="b16"><span class="fa fa-edit margin-r4"></span>编辑</a></div>
                    </div>
                  </div>
                </div>  
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <modal title="选择部门" size="tiny" class="modal"
      v-model="userMovePanelShow"
      :close-on-click-modal="!userStatusSave"
      :close-on-press-escape="!userStatusSave"
      :show-close="!userStatusSave"
    >
      <div>
        <div style="margin-bottom: 5px;">
          <span style="margin-right: 5px;">移动人员</span>
          <el-tag v-show="user.userName"
            class="user-tag"
            type="primary"
          >{{user.userName}}</el-tag>
          <span style="margin-right: 5px;">到</span>
          <el-tag
            :type="userMoveToDept.roleName ? 'gray' : ''"
            :closable="!!userMoveToDept.roleName"
            @close="userMoveToDept = {}"
          >{{userMoveToDept.roleName || '根部门'}}</el-tag>
        </div>
        <tree class="modal_wrap"
          :data="deptTree"
          :getNodeTitle="getDeptTitle"
          :activeNode="userMoveToDept"
          @loadChildrenNodes="onLoadChildrenNodes"
          @changeActiveNode="onChangeMoveActiveNode"
        >
        </tree>
      </div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="userMovePanelShow = false">取 消</el-button>
        <el-button type="primary" 
          @click="moveUsersToDept" 
          :disabled="!userMoveToDept"
        >确 定</el-button>
      </span>
    </modal>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import Tree from 'components/Tree'
import TreeMenu from 'components/TreeMenu'
import Modal from 'components/Modal'
import Dropdown from 'components/Dropdown'
import DropdownContent from 'components/DropdownContent'
import PageSelect from 'components/PageSelect'

export default {
  components: {
    PageSelect,
    Tree,
    TreeMenu,
    Modal,
    Dropdown,
    DropdownContent
  },
  computed: {
    ...mapState({
      catalogTree: state => state.dept.catalogTree
    })
  },
  data () {
    return {
      userMoveToDept: {},
      userMovePanelShow: false,
      dept: {},
      user: {
        name: '',
        sex: '1',
        userStatus: '1',
        cellphone: '',
        email: '',
        deptId: '',
        password: ''
      },
      userRoles: [],
      oldUserRolesJson: ''
    }
  },
  watch: {
    userMessage (nv, ov) {
      this.$message({
        type: nv.type,
        message: nv.msg
      })
      if (nv.type === 'success') {
        this.goToList()
      }
    }
  },
  created () {
  },
  methods: {
    getDeptTitle (node) {
      return node.roleName
    },
    goToList () {
      let to
      if (this.currDept.id) {
        to = {
          name: 'dept',
          params: { deptId: this.currDept.id }
        }
      } else {
        to = { path: '/dept' }
      }
      this.$router.push(to)
    },
    saveUser () {
      if (this.isCreate) {
        if (this.userRoles && this.userRoles.length > 0) {
          this.user.userRoles = this.userRoles
        }
        if (!this.user.deptRole) {
          this.user.deptRole = this.currDept.id
        }
        this.$store.dispatch(types.user.A_CREATE, this.user)
      } else {
        if (this.oldUserRolesJson !== JSON.stringify(this.userRoles)) {
          this.user.userRoles = this.userRoles.length === 0 ? '' : this.userRoles
        }
        this.$store.dispatch(types.user.A_SAVE_CHANGE, {
          oldVal: this.currUser,
          newVal: this.user
        })
      }
    },
    moveUsersToDept () {
      if (this.userMoveToDept.id !== this.currDept.id) {
        this.user.deptRole = this.userMoveToDept.id ? this.userMoveToDept.id : ''
        this.dept.roleName = this.userMoveToDept.id ? this.userMoveToDept.roleName : '根部门'
      }
      this.userMovePanelShow = false
    },
    onLoadChildrenNodes (dept) {
      this.$store.dispatch(types.dept.A_LOAD_TREE, dept)
    },
    onChangeMoveActiveNode (dept) {
      if (dept.id !== this.currDept.id) {
        this.userMoveToDept = dept
      }
    }
  }
}
</script>

<style>
</style>
