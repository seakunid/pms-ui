<template>
    <div class="layout">
        <div class="layout__left">
            <SideBar/>
        </div>
        <div class="layout__right">
            <div class="content">
                <h4>Netflix Packets</h4>
                <div class="alert alert-success" role="alert" v-if="showAlert">
                    Status Packet Berhasil di Update
                </div>
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
</template>

<script>
import axios from 'axios'
import SideBar from '~/components/mollecules/SideBar'
export default {
    components: {
        SideBar,
    },
    data() {
        return {
            packets: [],
            showAlert: false
        }
    },
    beforeMount() {
        this.checkLoggedUser()
    },
    mounted() {
        this.getDataPackets()
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
            axios.get('https://seakun-packet-api-v1.herokuapp.com/netflix')
            .then(res => {
                this.packets = res.data
            })
            .catch(err => {
                console.log(err);
            })
        },
        changeStatusPacket(packet) {
            let payload = {
                active: !packet.active
            }
            axios.patch(`https://seakun-packet-api-v1.herokuapp.com/netflix/${packet.id}`, payload)
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
        width: 26%;
    }
    &__right {
        width: 74%;
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

</style>