<template>
  <div :class="pathClass">
    <div class="frame_con" id="frame_con_article">
      <div class="product-navbar classify-navbar" :class="{ 'toggle': treeToggle, 'dept-navbar-open': treeToggleInMobile }">
        <div class="tree-header">栏目管理</div>
        <div class="btn-switch">
          <span @click="treeToggle = !treeToggle" class="fa fa-angle-double-right"></span>
          <span @click="goToIndex" class="fa fa-angle-left"></span>
        </div>
        <tree class="accordion-list" 
          ref="dept"
          :data="deptTree"
          :getNodeTitle="getDeptTitle"
          :activeNode="currDept"
          @loadChildrenNodes="onLoadChildrenNodes"
          @changeActiveNode="onChangeActiveNode"
          @clickNode="onClickDept"
        >
          <tree-menu :full="fullContent">
            <ul id="contextmenu" class="f-dropdown" data-dropdown-content aria-hidden="true" tabindex="-1">
              <li class="el-dropdown-menu__item"><el-button type="text" @click="deptEditPanelShow = true">设置部门</el-button></li>
              <li class="el-dropdown-menu__item"><el-button type="text" @click="deptCreatePanelShow = true">添加子部门</el-button></li>
              <li class="el-dropdown-menu__item"><el-button type="text" @click="showDeptMovePanel(true)">移动到...</el-button></li>
              <li class="el-dropdown-menu__item"><el-button type="text" :disabled="true">上移</el-button></li>
              <li class="el-dropdown-menu__item"><el-button type="text" :disabled="true">下移</el-button></li>
              <li class="el-dropdown-menu__item"><el-button type="text" @click="deleteDept">删除</el-button></li>
            </ul>
          </tree-menu>
        </tree>
        <modal title="修改部门" v-if="dept" v-model="deptEditPanelShow" :size="fullContent ? 'large' : 'tiny'">
          <div class="row">
            <div class="large-6 small-24 medium-6 columns"><label>部门名称：</label></div>
            <div class="large-18 small-24 medium-18 columns">
              <el-input placeholder="请输入部门名称" v-model="dept.roleName"></el-input>
            </div>
          </div>
          <div class="row">
            <div class="large-6 small-24 medium-6 columns"><label class="font-gray">部门ID：</label></div>
            <div class="large-18 small-24 medium-18 columns">
              <label class="font-gray"><span>{{dept.id}}</span></label>
            </div>

          </div>
          <span slot="footer" class="dialog-footer">
            <el-button @click="deptEditPanelShow = false">取 消</el-button>
            <el-button type="primary"
              @click="saveDept"
              @keyup.enter="saveDept"
              :loading="deptSave"
            >确 定</el-button>
          </span>
        </modal>
        <modal title="创建子部门" v-if="dept" v-model="deptCreatePanelShow" :size="fullContent ? 'large' : 'tiny'"
          :close-on-click-modal="!deptSave"
          :close-on-press-escape="!deptSave"
          :show-close="!deptSave"
        >
          <div class="row">
            <div class="large-6 small-24 medium-6 columns"><label>部门名称：</label></div>
            <div class="large-18 small-24 medium-18 columns">
              <el-input placeholder="请输入部门名称" v-model="dept.roleName"></el-input>
            </div>
          </div>
          <span slot="footer" class="dialog-footer">
            <el-button @click="deptCreatePanelShow = false">取 消</el-button>
            <el-button type="primary" 
              @click="saveSubDept" 
              @keyup.enter="saveSubDept" 
              :loading="deptSave"
            >确 定</el-button>
          </span>
        </modal>
        <modal title="移动部门" class="modal" :size="fullContent ? 'large' : 'tiny'"
          v-model="deptMovePanelShow"
          :close-on-click-modal="!deptSave"
          :close-on-press-escape="!deptSave"
          :show-close="!deptSave"
        >
          <div>
            <div style="padding-bottom: 5px;">
              <span style="margin-right: 3px;">移动</span>
              <el-tag type="gray">{{currDept.roleName}}</el-tag>
              <span style="margin: 0 3px 0 3px;">到</span>
              <el-tag
                :type="deptMoveTo.roleName ? 'primary' : ''"
                :closable="!!deptMoveTo.roleName"
                @close="deptMoveTo = {}"
              >{{deptMoveTo.roleName || '根部门'}}</el-tag>
            </div>
            <tree class="modal_wrap"
              :data="deptTree"
              :getNodeTitle="getDeptTitle"
              :activeNode="deptMoveTo"
              @loadChildrenNodes="onLoadChildrenNodes"
              @changeActiveNode="onChangeMoveActiveNode"
            >
            </tree>
          </div>
          <span slot="footer" class="dialog-footer">
            <el-button @click="deptMovePanelShow = false">取 消</el-button>
            <el-button type="primary" 
              @click="moveDept" 
              :loading="deptSave"
            >确 定</el-button>
          </span>
        </modal>
      </div>
      <div class="product-con contents" :class="{ 'toggle': treeToggle || userListToggleInMobile }">
        <router-view id="mod_datatable"></router-view>
        <div class="btn-switch" @click="treeToggle = !treeToggle">
          <span class="fa fa-angle-double-left"></span><!-- <span class="fa fa-angle-left"></span> -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import Tree from 'components/Tree'
import TreeMenu from 'components/TreeMenu'
import Modal from 'components/Modal'
import Dropdown from 'components/Dropdown'
import DropdownContent from 'components/DropdownContent'

export default {
  components: {
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
      dept: {},
      notFirst: false,
      treeToggle: false,
      treeToggleInMobile: false,
      userListToggleInMobile: false,
      deptMovePanelShow: false,
      deptMoveTo: {},
      deptCreateDisabled: true
    }
  },
  watch: {
  },
  created () {
  },
  methods: {
  }
}
</script>

<style>
</style>
