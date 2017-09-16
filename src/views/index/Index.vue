<template>
  <div>
	<div class="main-dashboard" id="frame_main_container">
      <div class="row margin-t">
        <div class="large-18 small-24 columns">
          <div class="row padding-l10 padding-r10">
            <div class="large-6 small-24 columns">
              <div class="panel-1">
                <div class="site-panel">
                  <div class="site-logo"><img src="http://okgo.top/errorImg.png?imageMogr2/thumbnail/!100x100r/gravity/Center/crop/100x100/quality/90!" title="站点图标" alt="站点图标"></div>                  
                  <div class="site-info">
                    <div class="site-title">小渔垂钓</div>
                    <a class=" font-alert margin-l6"><span class="fa fa-exclamation-circle has-tip  tip-right radius"  title="未上线" data-tooltip aria-haspopup="true"></span></a>
                    <div class="site-dn"><a href="www.baidu.com">www.baidu.com</a></div>
                    <div class="site-versition">
                      <span class="font-gray margin-r10">XXX免费版</span><a class="p-color">升级</a>
                    </div>
                  </div>
                  <div class="counter-info">
                    <ul class="large-up-3 medium-up-3 small-up-3 clearfix">
                      <li class="column">
                        <a href="" title="用户人数">
                          <div><span class="fa fa-user"></span></div>
                          26k+
                          <div>+14</div>
                        </a>
                      </li>
                      <li class="column">
                        <a href="" title="网站排行">
                          <div><span class="fa fa-paper-plane-o"></span></div>
                          1095
                          <div>+56</div>
                        </a>
                      </li>
                      <li class="column">
                        <a href="" title="浏览人数">
                          <div><span class="fa fa-eye"></span></div>
                          26k+
                          <div>+38</div>
                        </a>
                      </li>
                    </ul>
                  </div>
                </div>
              </div>
            </div>
            <div class="large-18 small-24 columns">
              <div class="portlet">
                <div class="portlet-title">
                  <div class="caption">
                    访问统计
                  </div>
                </div>
                <div class="portlet-body">
                  <div id="flot-chart" style="height:260px"></div>
                </div>
              </div>                    
            </div>
        
          </div>
          <div class="row margin-t">
            <div class="large-6 columns"></div>
            <div class="large-18 columns"></div>
          </div>
        </div>
        <div class="large-6 small-24 columns">fsa</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
}
</script>

<style>
</style>
