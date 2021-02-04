<template>
    <div align="left" class="sidebar">
        <ul class="list-group">
            <li v-for="(provider, id) in providers" :key="id" class="list-group-item" style="padding: 0px!important">
                <p class="label" @click="$router.push(`/provider?name=${provider.name.toLowerCase()}`)">{{ provider.name }}</p>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data() {
        return {
            providers: [
                {
                    name: 'Netflix'
                },
                {
                    name: 'Spotify'
                },
                {
                    name: 'Gramedia'
                },
                {
                    name: 'Youtube'
                },
                {
                    name: 'Microsoft365'
                },
                {
                    name: 'Canva'
                }
            ]
        }
    }
}
</script>

<style lang="scss" scoped>
.sidebar {
    margin-top: 52px;
    margin-left: 52px;
    .child-list {
        border: unset;
        padding: 9px 0px 9px 0px;
        &:hover {
            opacity: .7;
            background-color: #e5dafc;
            cursor: pointer;
        }
    }
    .list-group-item {
        cursor: pointer;
    }
    .label {
        padding: 16px;
        margin-bottom: 0px;
        &:hover {
            opacity: .7;
            background-color: #e5dafc;
            cursor: pointer;
        }
    }
    &__child {
        padding: 0px;
    }
    a {
        color: black;
        text-decoration: unset;
        padding: 0px 16px 0px 32px;
        &:hover {
            opacity: .7;
        }
    }
}
@media (max-width: 800px) {
  .sidebar {
      margin-left: 0px !important;
      padding-left: 16px;
  }
}
</style>
