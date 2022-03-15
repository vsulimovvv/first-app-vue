<template>
  <div>
    <p v-if="isLoading">Загрузка</p>
    <Tabs :tabs="tabs" @onClickTab="clickTab" />

    <AppInput @change="filterValue = $event" />

    <div class="buttons">
      <AppButton buttonName="По возрастанию (A-Z)" @click="sortPosts('increase')" />
      <AppButton buttonName="По убыванию (Z-A)" @click="sortPosts('decrease')" />
    </div>

    <!-- Сделать пагинацию -->
    <div class="pagination">
      <ul class="pagination__list">
        <li class="pagintaion__item" v-for="(page, index) in countPage" :key="index">
          <a href="#" class="pagination__link" @click="changePage(index)">{{ index + 1 }}</a>
        </li>
      </ul>
    </div>

    <p>{{ currentPage }}</p>

    <CardsList
      v-if="!isLoading && activeTab === 0"
      :typeName="tabs[activeTab].value"
      :list="paginationPosts"
      @deletePost="deletePost"
    />

    <CardsList
      v-if="!isLoading && activeTab === 1"
      :typeName="tabs[activeTab].value"
      :list="usersList"
    />
  </div>
</template>

<script>
import CardsList from "@/components/Cards/CardsList";
import Tabs from "@/components/Tabs/Tabs";
import AppInput from "@/components/base/AppInput";
import AppButton from "@/components/base/AppButton";

export default {
  name: "Posts",
  components: {
    CardsList,
    Tabs,
    AppInput,
    AppButton,
  },

  // todo Сделать пост

  data() {
    return {
      isLoading: true,
      postsList: [],
      usersList: [],
      tabs: [
        { value: "post", name: "Посты" },
        { value: "user", name: "Пользователи" },
      ],
      activeTab: 0,
      filterValue: "",
      currentPage: 1,
      numberPosts: 20,
    };
  },

  created() {
    setTimeout(() => {
      this.fetchData();
      this.fetchUsers();
      this.isLoading = false;
    }, 1000);
  },

  methods: {
    fetchData() {
      fetch("https://jsonplaceholder.typicode.com/posts")
        .then((response) => response.json())
        .then((data) => {
          data.map((el) => {
            el.title = String(el?.title ?? "");
            el.id = Number(el?.id ?? 0);
            el.userId = Number(el?.userId ?? 0);
            el.body = String(el?.body ?? "");

            return el;
          });

          return data;
        })
        .then((validData) => (this.postsList = validData));
    },

    fetchUsers() {
      fetch("https://jsonplaceholder.typicode.com/users")
        .then((response) => response.json())
        .then((data) => {
          data.map((el) => {
            el.id = Number(el?.id ?? 0);
            el.name = String(el?.name ?? "");
            el.username = String(el?.username ?? "");
            el.email = String(el?.email ?? "");
            el.address = {
              street: String(el?.street ?? ""),
              city: String(el?.city ?? ""),
              zipcode: String(el?.zipcode ?? ""),
              suite: String(el?.suite ?? ""),
            };
            el.phone = String(el?.phone ?? "");
            el.website = String(el?.website ?? "");
            el.company = {
              name: String(el?.name ?? ""),
              catchPhrase: String(el?.catchPhrase ?? ""),
              bs: String(el?.bs ?? ""),
            };

            delete el.address.geo;
            return el;
          });

          return data;
        })
        .then((validData) => (this.usersList = validData));
    },

    clickTab(id = 0) {
      if (id !== this.activeTab) {
        this.activeTab = id;
      }
    },

    sortPosts(sortType) {
      this.postsList.sort((a, b) => {
        if (sortType === "increase") {
          if (a.title > b.title) {
            return 1;
          }
          if (a.title < b.title) {
            return -1;
          }
        }

        if (sortType === "decrease") {
          if (a.title > b.title) {
            return -1;
          }

          if (a.title < b.title) {
            return 1;
          }
        }

        return 0;
      });
    },

    deletePost(postId) {
      let idx = this.postsList.findIndex((post) => post.id === postId);

      if (idx !== -1) {
        this.postsList.splice(idx, 1);
      }
    },

    changePage(index) {
      this.currentPage = index;
    },
  },

  computed: {
    filteredPost() {
      return this.postsList.filter((post) => post.title.includes(this.filterValue));
    },

    paginationPosts() {
      console.log("this.currentPage", this.currentPage);
      let startPage = this.currentPage * this.numberPosts;
      let endPage = 20;
      console.log("start", startPage);

      endPage === 0 ? (endPage = this.numberPosts) : this.currentPage + this.numberPosts;

      console.log("end", startPage);

      return this.filteredPost.slice(startPage, endPage);
    },

    countPage() {
      return this.postsList.length / this.numberPosts;
    },
  },
};
</script>

<style lang="scss" scoped>
ul {
  list-style-type: none;
}
p {
  font-size: 30px;
}

.buttons {
  display: flex;
  align-items: center;
  gap: 10px;
}

.pagination {
  &__list {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  &__link {
    display: block;
    padding: 10px;
    text-decoration: none;
    border: 1px solid #333;
  }
}
</style>
