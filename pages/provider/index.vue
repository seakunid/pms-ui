<template>
    <div class="layout">
        <div class="layout__left">
            <SideBar/>
        </div>
        <div class="layout__right">
            <div class="content">
                <h4 style="font-weight: 800 !important">{{ provider.toUpperCase() }} PAKET</h4>
                <div class="alert alert-success" role="alert" v-if="showAlert">
                    Status Packet Berhasil di Update
                </div>
                <Loading
                  :isShow="showLoading"
                />
                <div v-if="!showLoading">
                  <div class="content__packet" v-for="(packet, index) in packets" :key="index">
                      <div class="detail">
                          <div>Nama Paket: {{packet.name}}</div>
                          <div>ID Paket: {{packet.id}}</div>
                          <div class="detail-status">
                              Status:
                              <span>Tidak Aktif</span>
                              <div class="custom-control custom-switch">
                                  <input @click="changeStatusPacket(packet)" type="checkbox" class="custom-control-input" :id="`customSwitch${packet.id}`" :checked="packet.active">
                                  <label class="custom-control-label" :for="`customSwitch${packet.id}`"></label>
                              </div>
                              <span>Aktif</span>
                          </div>
                      </div>
                  </div>
                </div>
            </div> 
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import SideBar from '~/components/mollecules/SideBar'
import Loading from '~/components/mollecules/Loading'
export default {
    components: {
        SideBar,
        Loading
    },
    data() {
        return {
            packets: [],
            showAlert: false,
            provider: '',
            showLoading: false
        }
    },
    beforeMount() {
        this.checkLoggedUser()
    },
    mounted() {
        this.provider = this.$route.query.name
        this.getDataPackets()
    },
    watch: {
      $route(to) {
        const { name } = to.query
        this.provider = name
        this.getDataPackets()
      }
    },
    methods: {
        checkLoggedUser() {
            const expired = localStorage.getItem('expired') && localStorage.getItem('expired')
            if (expired) {
                Date.now() >= expired && (window.location = '/')
            } else {
                window.location = '/'
            }
        },
        getDataPackets() {
            this.showLoading = true
            axios.get(`https://seakun-packet-api.herokuapp.com/${this.provider.toLowerCase()}`)
            .then(res => {
                this.showLoading = false
                this.packets = res.data
            })
            .catch(err => {
                this.showLoading = false
                console.log(err);
            })
        },
        changeStatusPacket(packet) {
            let payload = {
                active: !packet.active
            }
            axios.patch(`https://seakun-packet-api.herokuapp.com/${this.provider.toLowerCase()}/${packet.id}`, payload)
            .then(res => {
                console.log(res.data);
                console.log('Paket berhasil di update');
                this.showAlert = true
                this.getDataPackets()
                setTimeout(() => {
                    this.showAlert = false
                }, 5000);
            })
            .catch(err => {
                console.log(err);
            })
        }
    }
}
</script>

<style lang="scss" scoped>
.layout {
    display: flex;
    &__left {
        width: 20%;
    }
    &__right {
        width: 78%;
    }
}

.content {
    padding: 16px;
    margin: 16px;
    &__packet {
        padding: 16px;
        border: 1px solid #dddddd;
        margin-bottom: 10px;
        .detail {
            &-status {
                display: flex;
                span {
                    margin: 0px 10px;
                }
            }
        }
    }
}

@media (max-width: 800px) {
  .layout {
    &__left {
      width: 39%;
    }
  }
}

</style>