<template>
    <div class="col-md-5">
            <SearchBook v-model="searchText" />
    </div>
    <div class="row">
        
        <div class="mt-3 col-md-6">
            <h3 class="text-dark fw-bold">
                SÁCH
            </h3>
            <BookList v-if="filteredBooksCount > 0" :books="filteredBooks" v-model:activeIndex="activeIndex"
                @click="retrieveNxb(activeBook.maNxb)" />
            <p v-else>Không có sách nào.</p>


        <!---->
            <div class="mt-3 row agign-items-center">
                <button class="btn col btn btn-sm btn-primary me-2" @click="refreshList()">
                    <i class="fas fa-redo"></i> Làm mới
                </button>

                <button class="btn col btn btn-sm btn-success me-2" @click="goToAddBook">
                    <i class="fas fa-plus"></i> Thêm sách
                </button>

                <button class="btn col btn btn-sm btn-danger" @click="removeAllBooks">
                    <i class="fas fa-trash"></i> Xóa tất cả
                </button>
            </div>
        </div>

        <!--  -->
        <div class="mt-3 col-md-5 ">
            <div v-if="activeBook != null" class="p-3  border rounded" style="background: #CFE5E4;">
                <h4 class="text-dark fw-bold">
                    THÔNG TIN SÁCH
                    <i class="fas fa-book"></i>
                </h4>
                <BookCard :book="activeBook" :tenNXB="tenNxb" />
                <router-link :to="{
                    name: 'book-edit',
                    params: { id: activeBook._id }
                }">
                    <span class="mt-2">
                        <button class="btn btn-sm btn-outline-danger">
                            <i class="fas fa-edit"></i> Hiệu chỉnh
                        </button>
                    </span>
                </router-link>
            </div>
        </div>
    </div>
</template>
<script>

import BookList from "@/components/BookList.vue";
import BookCard from "@/components/BookCard.vue";
import BookService from "@/services/book.service";
import SearchBook from "@/components/InputSearch.vue";
import NxbService from "@/services/nxb.service";
export default {
    components: {
        SearchBook,
        BookList,
        BookCard,
    },
    data() {
        return {
            books: [],
            activeIndex: -1,
            searchText: "",
            nxb: [],
            tenNxb: "",
            userName: "",
        };
    },
    watch: {
        searchText() {
            this.activeIndex = -1;
        },
    },
    computed: {
        bookStrings() {
            return this.books.map((book) => {
                const { tenSach } = book;
                return [this.removeAccents(tenSach)].join().toLowerCase();
            });
        },
        filteredBooks() {
            if (!this.searchText) {
                return this.books;
            }
            return this.books.filter((_book, index) => {
                return this.bookStrings[index].includes(this.searchText)
            });


        },
        activeBook() {
            if (this.activeIndex < 0) {
                return null;
            }
            else {
                return this.filteredBooks[this.activeIndex];
            }

        },

        filteredBooksCount() {
            return this.filteredBooks.length;

        },
        getUserName() {
            this.userName = sessionStorage.getItem('userName');
            return this.userName;
        }
    },
    methods: {
        removeAccents(string){
            return string.normalize('NFD')
                .replace(/[\u0300-\u036f]/g, '')
                .replace(/đ/g, 'd').replace(/Đ/g, 'D')
        },
        async retrieveNxb(id) {
            try {
                this.nxb = await NxbService.get(id);
            } catch (error) {
                console.log(error);
            }
            this.tenNxb = this.nxb.tenNxb;
        },
        async retrieveBooks() {
            try {
                this.books = await BookService.getAll();
            } catch (error) {
                console.log(error);
            }
        },
        async removeAllBooks() {
            if (confirm("Bạn muốn xóa tất cả sách?")) {
                try {
                    await BookService.deleteAll();
                    this.refreshList();
                } catch (error) {
                    console.log(error);
                }
            }
        },

        refreshList() {
            this.retrieveBooks();
            this.activeIndex = -1;
        },

        goToAddBook() {
            this.$router.push({ name: "book-add" });
        },
    },
    mounted() {
        if (localStorage.getItem('reloaded')) {
            localStorage.removeItem('reloaded');
        } else {
            localStorage.setItem('reloaded', '1');
            location.reload();
        }
        if (!this.getUserName) {
            this.$router.replace({ name: "login" });
        }
        this.refreshList();
    },
};
</script>
