<template>
  <div class="col-12">
    <div class="header">
      <div class="header-body">
        <div class="row align-items-center">
          <div class="col">
            <h6 class="header-pretitle">
              Server Tools
            </h6>

            <h1 class="header-title">
              Tools
            </h1>
          </div>
        </div>
        <div class="row align-items-center">
          <div class="col">

            <ul class="nav nav-tabs nav-overflow header-tabs">
              <li class="nav-item">
                <router-link to="/admin/tools" active-class="" exact-active-class="active" class="nav-link">
                  <i class="fe fe-download-cloud mr-2"></i>
                  Manual Backup
                </router-link>
              </li>

              <li class="nav-item">
                <router-link to="/admin/tools/client/assets" active-class="" exact-active-class="active" class="nav-link">
                  <span class="icon mr-3"><i class="fa fa-th"></i></span>
                  Client Assets
                </router-link>
              </li>

            </ul>
          </div>
        </div>
      </div>
    </div>

    <div class="row">
      <div class="col-lg-12">
        <router-view></router-view>
      </div>
    </div>

  </div>
</template>
