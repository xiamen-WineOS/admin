<template>
  <div>
    <div class="datatable_row" id="frame_datatable">
      <div id="data_datatable">
        <data-table
          ref="users"
          store-module="user"
          v-model="selectedUsers"
          :list-url-params="listUrlParams"
          :button-add="false"
          :button-edit="false"
          :button-search="false"
        >
          <template slot="title">
            <a class="fa-back"><span class="fa fa-angle-left"></span></a>2222<a class="margin-l10" @click="deptEditPanelShow = true"><span class="fa fa-cog"></span></a>
          </template>
          <template slot="button">
            <el-dropdown split-button type="success" trigger="click"
              
              @command="userCommand"
              class="show-for-large"
            >
              添加文章
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item command="invite">邀请成员</el-dropdown-item>
              </el-dropdown-menu>
            </el-dropdown>
            <el-button class="show-for-large" type="success" @click="deptCreatePanelShow = true"><span class="fa fa-plus"></span> 创建子栏目</el-button>
            <a @click="menuInMobileShow = true" class="addEvery hide-for-large" data-dropdown="addEvery" aria-controls="addEvery" aria-expanded="false"><span>&#43</span></a>
            <modal v-model="menuInMobileShow"
              type="dialog"
              size="full"
              custom-class="modal-bottom"
            >
              <ul style="margin: 0;">
                <li><a>添加成员</a></li>
                <li><a>邀请成员</a></li>
                <li><a>创建子部门</a></li>
              </ul>
            </modal>
          </template>
          <template slot="search">
            <el-tooltip class="cue show-for-large" content="请输入..：" placement="bottom" effect="light">
              关键词<span>?</span>
            </el-tooltip>
            <el-input placeholder="请输入作者..." v-model="search.userName" :style="{ width: fullContent ? '100%' : '300px'}" class="d-search" accesskey="s">
              <el-button slot="append" icon="search" :loading="userStatusFetch"></el-button>
            </el-input>
          </template>
          <data-table-button slot="leftButton" class="hide-for-small-only" @click="userMovePanelShow = true">
            移动到...
          </data-table-button>
          <data-table-button slot="leftButton" class="hide-for-small-only" type="dropdown">
            启用<span class="fa fa-caret-down margin-l6 large-margin-r0"></span>
            <ul slot="content">
              <el-dropdown-item>启用</el-dropdown-item>
              <el-dropdown-item>禁用</el-dropdown-item>
            </ul>
          </data-table-button>
          <data-table-button slot="leftButton" class="hide-for-small-only" type="dropdown">
            更多操作<span class="fa fa-caret-down margin-l6 large-margin-r0"></span>
            <div slot="content">
              <el-dropdown-item>111启用</el-dropdown-item>
              <el-dropdown-item>222禁用</el-dropdown-item>
            </div>
          </data-table-button>
          <el-dropdown slot="rightButton" trigger="click" class="show-for-large" style="padding-top: 8px;">
            <a class="Tablestate icon-angle-down" data-dropdown="sorting" aria-controls="sorting" aria-expanded="false">全部人员(10)</a>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item>全部成员(10)</el-dropdown-item>
              <el-dropdown-item>启用(4)</el-dropdown-item>
              <el-dropdown-item>禁用(0)</el-dropdown-item>
              <el-dropdown-item disabled>已禁用(6)</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
          <template slot="head">
            <th class="large-padding-l0">姓名</th>
            <th class="hide-for-small-only hide-for-medium-only">员工编号</th>
            <th class="hide-for-small-only hide-for-medium-only">手机号码</th>
            <th class="hide-for-small-only hide-for-medium-only">邮箱</th>
            <th class="hide-for-small-only hide-for-medium-only">创建时间</th>
            <th class="hide-for-small-only hide-for-medium-only">状态</th>
            <!-- <th class="hide-for-small-only hide-for-medium-only">角色</th> -->
            <th class="hide-for-small-only hide-for-medium-only">操作</th>
          </template>
          <template scope="{ item, editPanel }">
            <td class="large-padding-l0">
              <div class="img small">
                <a title="头像"><img src="~assets/default/images/av1.jpg" alt=""></a>
              </div>
              <div class="des">
                <a class="h-underline" :title="item.userName">{{item.userName}}</a> <span class="font-gray">登录名：{{item.loginName}}</span>
              </div>
            </td>
            <td data-colname="员工编号：" class="n-view">{{item.id}}</td>
            <td data-colname="手机号码：" class="author">{{item.cellphone}}</td>
            <td data-colname="邮箱：" class="n-view">{{item.email}}</td>
            <td data-colname="创建时间：" class="n-view">{{item.createTime | dateFormat }}</td>
            <td data-colname="状态：">
              <template v-if="item.userStatus === '1'">
                <span class="fa fa-circle font-success margin-r4"></span>启用
              </template>
              <template v-else-if="item.userStatus === '0'">
                <span class="fa fa-circle font-gray margin-r4"></span>禁用
              </template>
            </td>
            <!-- <td data-colname="角色：" class="n-view">管理员</td> -->
            <td data-colname="操作">
              <div class="fa-tools">
                <a  title="编辑" class="hide-for-medium-only"><span class="fa fa-edit"></span></a>
                <a title="删除" class="item-delete"><span class="fa fa-trash-o"></span></a>
             </div>
            </td>
          </template>
        </data-table>
        <modal title="选择部门" class="modal" :size="fullContent ? 'large' : 'tiny'"
          v-model="userMovePanelShow"
          :close-on-click-modal="!userStatusSave"
          :close-on-press-escape="!userStatusSave"
          :show-close="!userStatusSave"
        >
          <div>
            <div>
              <span style="margin-right: 5px;">移动人员</span>
              <el-tag v-for="user in moveUsers"
                :key="user.id"
                class="user-tag"
                type="primary"
                :closable="true"
                @close="moveSelectedUser(user)"
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
             
              :loading="userStatusSave"
              :disabled="selectedUsers.length === 0"
            >确 定</el-button>
          </span>
        </modal>
        <modal title="邀请成员加入" class="modal"
          v-model="userInvitePanelShow"
          :size="fullContent ? 'large' : 'tiny'"
        >
          <el-tabs style="width: 100%">
            <el-tab-pane label="邮箱邀请" class="fix-box-sizing">
              <div class="row">
                <div class="small-24 column">
                  <el-input
                    type="textarea"
                    :autosize="{ minRows: 2, maxRows: 4}"
                    placeholder="如要添加多个，请换行"
                    v-model="inviteUsers">
                  </el-input>
                </div>
              </div>
              <div class="row">
                <div class="small-24 column">
                  <label>已成功邀请 <em>14</em>人,以下6个账号无效：</label>
                  <ul class="invalid-mail">
                    <li><span>abc@qq.com</span></li>
                    <li><span>abcfasdf@qq.com</span></li>
                    <li><span>abcasdf@qq.com</span></li>
                    <li><span>1234abc@qq.com</span></li>
                    <li><span>abfasdc@qq.com</span></li>
                    <li><span>ab12445c@qq.com</span></li>
                  </ul>
                </div>
              </div>
            </el-tab-pane>
            <el-tab-pane label="微信邀请" class="fix-box-sizing">
              <div class="row">
                <div class="small-24 columns">
                	<div class="el-input">
                		<input type="text" value="这边放大家加入平台的链接" class="el-input__inner">
                	</div>
                </div>
              </div>
              <div class="row margin-t">
                <div class="small-10 medium-4 large-6 columns">
                  <div class="qr-box">
                    <img src="/static/amc/images/qr.png" alt="二维码">
                  </div>
                </div>
                <div class="small-14 medium-20 large-18 columns">
                  <p>扫描二维码</p><p>邀请微信好友</p>
                </div>
              </div>
              <div class="row margin-t">
                <div class="small-24 columns middle">
                  <label class="font-s12 font-gray text-center">成员加入项目后，为了防止链接泄露可以选择 <a class="el-button el-button--danger el-button--mini">关闭链接</a></label>
                </div>
              </div>
            </el-tab-pane>
          </el-tabs>
          <span slot="footer" class="dialog-footer">
            <el-button type="primary" 
              @click="userInvitePanelShow = false"
              :disabled="inviteUsers.length === 0"
            >邀 请</el-button>
          </span>
        </modal>
      </div>
    </div>
    <div v-clickoutside="closeUserInfo" class="opp_result" id="frame_opp_result" :class="userInfoShow ? 'open' : ''">
      <div class="modal-title">详细资料
        <div class="modal-tools float-right">
          <div class="simple-tools">
            <a class="button split"><span class="fa fa-edit"></span>修改</a>
            <a class="button split hide-for-small-only hide-for-medium-only"><span class="fa fa-lock"></span>禁用</a>
            <!-- <ul id="m1" class="f-dropdown select-content" data-dropdown-content="" style="display: none">
              <li class="hide-for-large"><a>禁用</a></li>
              <li><a href="#">移除关注</a></li>
              <li><a href="#">移动</a></li>
              <li><a href="#">删除</a></li>
            </ul> -->
            <!-- <a class="button" href="#" title="更多" data-dropdown="m1" aria-controls="m1" aria-expanded="false">更多<span class="fa fa-angle-down margin-l6"></span></a> -->
            <el-dropdown trigger="click" class="like-button">
              <a class="button" title="更多" data-dropdown="m1" aria-controls="m1" aria-expanded="false">更多<span class="fa fa-angle-down margin-l6"></span></a>
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item class="hide-for-large">禁用</el-dropdown-item>
                <el-dropdown-item>移动</el-dropdown-item>
                <el-dropdown-item>删除</el-dropdown-item>
              </el-dropdown-menu>
            </el-dropdown>
          </div>
        </div>
      </div>
      <div class="modal_wrap">
        <div class="transverse">
          <div class="img">
            <a title="文章图片"><img src="~assets/default/images/av1.jpg" alt=""></a>
          </div>
          <div class="content">
            <div class="title"><a href="#">{{userInfo.userName}}</a><span class="sex women"></span></div>
            <div class="description clearfix">登录名：{{userInfo.loginName}}</div>
            <label class="label rt10">启用</label>
          </div>
        </div>
        <div class="merber-info margin-t">
          <div class="row">
            <div class="large-6 columns"><label>员工编号:</label></div>
            <div class="large-18 columns"><label class="font-gray">{{userInfo.id}}</label></div>
          </div>
          <div class="row">
            <div class="large-6 columns"><label>手机:</label></div>
            <div class="large-18 columns"><label class="font-gray">{{userInfo.cellphone}}</label></div>
          </div>
          <div class="row">
            <div class="large-6 columns"><label>部门:</label></div>
            <div class="large-18 columns"><label class="font-gray">{{currDept.roleName}}</label></div>
          </div>
          <div class="row">
            <div class="large-6 columns"><label>标签:</label></div>
            <div class="large-18 columns"><label><label class="label">java</label></label></div>
          </div>
        </div>
      </div>
      <a @click="closeUserInfo" class="close-opp-result">&#215;</a>
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
import DataTable from 'components/DataTable'
import DataTableButton from 'components/DataTableButton'

export default {
  components: {
    Tree,
    TreeMenu,
    Modal,
    Dropdown,
    DropdownContent,
    DataTable,
    DataTableButton
  },
  computed: {
    ...mapState({
    })
  },
  data () {
    return {
      listUrlParams: {},
      selectedUsers: [],
      userMovePanelShow: false,
      userInvitePanelShow: false,
      menuInMobileShow: false,
      userInfoShow: false,
      userInfo: {},
      userMoveToDept: {},
      inviteUsers: ''
    }
  },
  created () {
  },
  watch: {
  },
  methods: {
    closeUserInfo () {
      this.userInfoShow = false
    }
  }
}
</script>

<style>
</style>
