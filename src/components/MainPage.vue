<template>
  <div class="container mt-5">
    <div class="d-flex">
      <input v-model.trim="filter" class="form-control w-auto" placeholder="Найти по ФИО"/>
      <div class="flex-grow-1"></div>
      <button class="btn btn-outline-success" @click="modalAdd = true">Добавить</button>
    </div>
    <div class="my-2">
      <table class="w-100 table table-striped sortable">
        <thead>
          <th class="name-column bg-success text-light p-1" @click="sort('name')">Название<img class="sort-arrow p-1" :class="{'d-none': sortColumn !== 'name', 'sort-asc': sortAsc}" src="@/assets/sort.png"/></th>
          <th class="director-column bg-success text-light p-1" @click="sort('director')">ФИО директора<img class="sort-arrow p-1" :class="{'d-none': sortColumn !== 'director', 'sort-asc': sortAsc}" src="@/assets/sort.png"/></th>
          <th class="phone-column bg-success text-light p-1" @click="sort('phone')">Номер телефона<img class="sort-arrow p-1" :class="{'d-none': sortColumn !== 'phone', 'sort-asc': sortAsc}" src="@/assets/sort.png"/></th>
          <th class="delete-column bg-success text-light p-1"></th>
        </thead>
        <tbody>
          <template v-if="view.length > 0">
            <template v-for="(item) of view" :key="item">
              <tr>
                <td>{{ item.name }}</td>
                <td>{{ item.director }}</td>
                <td>{{ item.phone }}</td>
                <td><div class="d-flex"><button class="btn p-0 m-0 mx-auto" @click="delOrg(item.id)" title="Удалить"><img src="@/assets/delete.png"/></button></div></td>
              </tr>
            </template>
          </template>
        </tbody>
      </table>
      <div class="float-end">
        <button class="btn btn-page border-0" :class="{disabled: !canPrev}" @click="page--; viewList()"><img src="@/assets/page-prev.png"/></button>
        <span>Страница {{ page }}</span>
        <button class="btn btn-page border-0" :class="{disabled: !canNext}" @click="page++; viewList()"><img src="@/assets/page-next.png"/></button>
      </div>
    </div>
  </div>
  <template v-if="modalAdd === true">
    <ModalAdd @cancel="modalAdd = false" @add-org="addOrg"></ModalAdd>
  </template>
</template>

<script>
import ModalAdd from "@/components/ModalAdd";
export default {
  name: "MainPage",
  components: {
    ModalAdd
  },
  data() {
    return {
      data: [
        {
          id: 1,
          name: 'ООО "Вектор"1',
          director: 'Иванов И.И.1',
          phone: '+7 000 123 45 67'
        },
        {
          id: 2,
          name: 'ИП Сидоров С.С.1',
          director: 'Сидоров С.С.1',
          phone: '+7 000 56 78 99'
        },
        {
          id: 3,
          name: 'ООО "ГрафЭлит"1',
          director: 'Морозова А.С.1',
          phone: '+7 000 56 73 89'
        },
        {
          id: 4,
          name: 'ЗАО "Новая Птица"1',
          director: 'Агапов А.Н.1',
          phone: '+7 000 53 23 83'
        },
        {
          id: 5,
          name: 'ИП Смирнов М.Я.1',
          director: 'Смирнов М.Я.1',
          phone: '+7 000 93 20 13'
        },
        {
          id: 6,
          name: 'ООО "Вектор"2',
          director: 'Иванов И.И.2',
          phone: '+7 000 123 45 67'
        },
        {
          id: 7,
          name: 'ИП Сидоров С.С.2',
          director: 'Сидоров С.С.2',
          phone: '+7 000 56 78 99'
        },
        {
          id: 8,
          name: 'ООО "ГрафЭлит"2',
          director: 'Морозова А.С.2',
          phone: '+7 000 56 73 89'
        },
        {
          id: 9,
          name: 'ЗАО "Новая Птица"2',
          director: 'Агапов А.Н.2',
          phone: '+7 000 53 23 83'
        },
        {
          id: 10,
          name: 'ИП Смирнов М.Я.2',
          director: 'Смирнов М.Я.2',
          phone: '+7 000 93 20 13'
        },
        {
          id: 11,
          name: 'ООО "Вектор"3',
          director: 'Иванов И.И.3',
          phone: '+7 000 123 45 67'
        },
        {
          id: 12,
          name: 'ИП Сидоров С.С.3',
          director: 'Сидоров С.С.3',
          phone: '+7 000 56 78 99'
        },
        {
          id: 13,
          name: 'ООО "ГрафЭлит"3',
          director: 'Морозова А.С.3',
          phone: '+7 000 56 73 89'
        },
        {
          id: 14,
          name: 'ЗАО "Новая Птица"3',
          director: 'Агапов А.Н.3',
          phone: '+7 000 53 23 83'
        },
        {
          id: 15,
          name: 'ИП Смирнов М.Я.3',
          director: 'Смирнов М.Я.3',
          phone: '+7 000 93 20 13'
        },
        {
          id: 16,
          name: 'ИП Смирнов М.Я.4',
          director: 'Смирнов М.Я.4',
          phone: '+7 000 93 20 13'
        },
        {
          id: 17,
          name: 'ООО "Вектор"4',
          director: 'Иванов И.И.4',
          phone: '+7 000 123 45 67'
        },
        {
          id: 18,
          name: 'ИП Сидоров С.С.4',
          director: 'Сидоров С.С.4',
          phone: '+7 000 56 78 99'
        },
        {
          id: 19,
          name: 'ООО "ГрафЭлит"4',
          director: 'Морозова А.С.4',
          phone: '+7 000 56 73 89'
        },
        {
          id: 20,
          name: 'ЗАО "Новая Птица"4',
          director: 'Агапов А.Н.4',
          phone: '+7 000 53 23 83'
        },
        {
          id: 21,
          name: 'ИП Смирнов М.Я.5',
          director: 'Смирнов М.Я.5',
          phone: '+7 000 93 20 13'
        },
        {
          id: 22,
          name: 'ИП Смирнов М.Я.6',
          director: 'Смирнов М.Я.6',
          phone: '+7 000 93 20 13'
        },
        {
          id: 23,
          name: 'ООО "Вектор"5',
          director: 'Иванов И.И.5',
          phone: '+7 000 123 45 67'
        },
        {
          id: 24,
          name: 'ИП Сидоров С.С.5',
          director: 'Сидоров С.С.5',
          phone: '+7 000 56 78 99'
        },
        {
          id: 25,
          name: 'ООО "ГрафЭлит"5',
          director: 'Морозова А.С.5',
          phone: '+7 000 56 73 89'
        },
        {
          id: 26,
          name: 'ЗАО "Новая Птица"5',
          director: 'Агапов А.Н.5',
          phone: '+7 000 53 23 83'
        },
        {
          id: 27,
          name: 'ИП Смирнов М.Я.7',
          director: 'Смирнов М.Я.7',
          phone: '+7 000 93 20 13'
        },
      ],
      view: [],
      idInc: 28,
      page: 1,
      limit: 7,
      sortColumn: 'name',
      sortAsc: true,
      filter: '',
      modalAdd: false,
    }
  },
  methods: {
    sort(sortColumn, change = true) {
      if (sortColumn === this.sortColumn) {
        if (change === true) this.sortAsc = !this.sortAsc;
      } else {
        this.sortAsc = true;
      }
      this.sortColumn = sortColumn;
      this.data.sort(function (a, b) {
        return a[sortColumn].localeCompare(b[sortColumn]);
      })
      if (this.sortAsc === false) {
        this.data.reverse();
      }
      this.viewList();
    },
    filterList(sortWord) {
      this.view = this.data.filter((item) => item['director'].toLowerCase().includes(sortWord.toLowerCase()));
    },
    viewList() {
      let start = this.limit * (this.page - 1);
      if (this.filter.length > 0) {
        this.filterList(this.filter);
        this.view = this.view.slice(start, start + this.limit);
      } else {
        this.view = this.data.slice(start, start + this.limit);
      }
    },
    addOrg(name, phone, director) {
      this.data.push({
        id: this.idInc++,
        name,
        phone,
        director,
      });
      this.sort(this.sortColumn, false);
      this.modalAdd = false;
    },
    delOrg(id) {
      let index = this.data.indexOf(this.data.find((item) => item.id === id));
      if (index >= 0) {
        this.data.splice(index, 1);
      }
      this.sort(this.sortColumn, false);
    }
  },
  watch: {
    filter(newWord, oldWord) {
      if (newWord !== oldWord) {
        this.page = 1;
        this.filterList(newWord);
        this.viewList();
      }
    }
  },
  computed: {
    canPrev() {
      return this.page > 1;
    },
    canNext() {
      if (this.filter.length > 0) {
        return this.data.filter((item) => item['director'].includes(this.filter)).length > this.limit * (this.page - 1) + this.view.length;
      } else {
        return this.data.length > this.limit * (this.page - 1) + this.view.length;
      }
    }
  },
  mounted() {
    this.sort('name', false);
  }
}
</script>

<style scoped>
.name-column {
  width: 30%;
  user-select: none;
}
.director-column {
  width: 30%;
  user-select: none;
}
.phone-column {
  width: 30%;
  user-select: none;
}
.delete-column {
  width: 40px;
  user-select: none;
}
.sortable th {
  cursor: pointer;
}
.sort-arrow {
  width: 25px;
  margin: 0;
  padding: 0;
}
.sort-asc {
  transform: rotate(180deg);
}
.btn-page img {
  width: 30px;
}
* {
  font-family: 'Cascadia Mono', 'Consolas', sans-serif;
}
</style>