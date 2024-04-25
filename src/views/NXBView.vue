<template>
    <h2 class="text-center text-dark">THÔNG TIN NHÀ XUẤT BẢN</h2>
    <div class="border border-2 border-dark rounded p-2">
        <table class="table">
            <thead>
                <tr>
                    <th>Mã NXB</th>
                    <th>Tên NXB</th>
                    <th>Địa chỉ</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="nxb in nxb" :key="nxb._id">
                    <td>{{ nxb._id }}</td>
                    <td>{{ nxb.tenNxb }}</td>
                    <td>{{ nxb.diaChi }}</td>
                    <td>
                        <router-link :to="{name: 'nxb-edit', params: { id: nxb._id }}">
                            <button class="btn btn-sm btn-outline-primary me-2">Sửa</button>
                        </router-link>
                        <button class="btn btn-sm btn-outline-danger" @click=deleteNXB(nxb._id)>Xóa</button>
                    </td>

                </tr>
            </tbody>
            <router-link :to="{
                name: 'nxb-add'
            }">
                <button class="btn btn-sm btn-outline-primary">Thêm NXB</button>
            </router-link>
        </table>
    </div>
</template>
<script>
import NxbService from "@/services/nxb.service.js";
export default {
    data() {
        return {
            nxb: []
        }
    },
    methods: {
        async deleteNXB(id){
            if(confirm("Bạn có chắc chắn muốn xóa nhà xuất bản " + id + " không?"))
            {
                try{
                    await NxbService.delete(id);
                    alert("Xóa nhà xuất bản thành công");
                }
                catch (error) {
                    console.log(error);
                }
            }
        },
        async retrieveNxb() {
            try {
                this.nxb = await NxbService.getAll();
            } catch (error) {
                console.log(error);
            }
        },
    },
    created() {
        this.retrieveNxb();
    }

}
</script>