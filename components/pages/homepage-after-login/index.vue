<template>
  <div>
    <div class="layout">
      <div class="layout__left">
        <SideBar/>
      </div>
      <div class="layout__right">
        <div class="content">
          <div style="padding: 0px 20px">
            <h3 class="title">
              Hello,
            </h3>
            <div class="links">
              <a
                target="_blank"
                rel="noopener noreferrer"
                class="button--green"
              >
                Silahkan pilih menu yang ada disamping
              </a>
            </div>
          </div>
        </div> 
      </div>
    </div>
  </div>
</template>

<script>
import SideBar from '~/components/mollecules/SideBar'
export default {
    components: {
        SideBar,
    }
}
</script>

<style lang="scss" scoped>
.layout {
  display: flex;
  justify-content: space-around;
  &__left {
    width: 26%;
  }
  &__right {
    width: 74%;
  }
}

.content {
  padding: 16px;
}

.title {
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
@media (max-width: 800px) {
  .layout {
    &__left {
      width: 38%;
    }
  }
  .title {
    font-size: 40px;
  }
  .content {
    padding: 51px;
  }
}
</style>
