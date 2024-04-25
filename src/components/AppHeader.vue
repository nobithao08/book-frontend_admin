<template>
    <div class="navbar navbar-expand navbar-dark bg-dark row">
        <div class="col-sm-2"><a href="/" class="navbar-brand px-3 fw-bold">QUẢN LÝ SÁCH</a></div>
        <div class="col-sm">
            <ul v-if="!getUserName" class="nav d-flex justify-content-end">
                <li class="nav-item">
                    <router-link :to="{ name: 'register' }" class="nav-link text-white">
                        ĐĂNG KÝ
                    </router-link>
                </li>
                <li class="nav-item">
                    <router-link :to="{ name: 'login' }" class="nav-link text-white">
                        ĐĂNG NHẬP
                    </router-link>
                </li>
            </ul>
            <ul v-else class="nav d-flex justify-content-end">
                <li class="nav-item">
                    <router-link :to="{ name: 'homeBook' }" class="nav-link text-white">
                        SÁCH
                    </router-link>
                </li>
                <li class="nav-item">
                    <router-link :to="{ name: 'nv-info', params: { MSNV: getUserName } }" class="nav-link text-white">
                        TÀI KHOẢN
                    </router-link>
                </li>
                <li class="nav-item">
                    <router-link :to="{ name: 'nxbview' }" class="nav-link text-white">
                        NHÀ XUẤT BẢN
                    </router-link>
                </li>
                <li class="nav-item">
                    <a href="#" class="nav-link text-white" @click="logout">ĐĂNG XUẤT</a>
                </li>
            </ul>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return{
            userName: '',
        }
    },
    computed: {
        getUserName() {
            this.userName = sessionStorage.getItem('userName');
            return this.userName;
        }
    },
    methods: {
        logout() {
            if (confirm('Bạn muốn đăng xuất?')) {
                sessionStorage.setItem("userName","");
                this.userName = "";
                window.location.reload();
            }
        }
    },
    mounted(){
        if(!this.userName)
        {
            this.$router.push({name: "homeBook"});
        }
    }
};
</script>