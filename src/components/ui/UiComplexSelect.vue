<template>
    
    <v-dialog
        v-model="dialog"
        max-width="1000px"
        height="500px"
        scrollable
    >
        <!-- При дорабатывании компонента необходимо заложить здесь слот для вывода результата -->
        <template v-slot:activator="{ props }">
            <p v-if="select_result">
                {{ prepareSelectResultTitle }}
            </p>

            <v-btn
                color="primary"
                v-bind="props"
            >
                {{ prepareBtnTitle }}
            </v-btn>
        </template>

        <v-card>
            <v-card-text>
                <v-card-title>
                    Выберите {{ itemName }} 
                </v-card-title>

                <v-text-field
                    v-model="filter_text"
                    dense
                    label="Поиск"
                />

                <v-radio-group
                    v-if="filteredItems.length > 0"
                    v-model="select_result"
                >
                    <v-radio
                        v-for="(item, index) in filteredItems"
                        :key="index"
                        :label="item.title"
                        :value="item.value"
                    > 
                    </v-radio>
                </v-radio-group>

                <p v-else-if="filter_text && (filteredItems.length === 0)">
                    Варианты, включающие "{{ filter_text }}" не найдены. Попробуйте изменить параметры поиска.
                </p>

                <p v-else>
                    Данные не найдены.
                </p>
            </v-card-text>

            <v-card-actions>
                <div class="btn__wrapper">
                    <v-btn
                        color="success"
                        @click="saveAndClose"
                    >
                        Выбрать
                    </v-btn>

                    <v-btn
                        color="primary"
                        @click="clearAndClose"
                    >
                        Очистить
                    </v-btn>
                </div>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>
import departmentJSON from './../../mocks/departmens.json';
import addressJSON from './../../mocks/address.json';

export default {

  name: 'UiComplexSelect',

  props: {
    itemName: {
        type: String,
        default () {
            return 'нужное значение'
        }
    },

    url: {
        type: String,
        required: true
    },

    value: {
        type: [String, Number],
        default () {
            return null
        }
    }
  },

  created () {
    this.prepareDataForSelect()

    this.prepareSelectedValue()
  },

  computed: {
    // фильтруем список по заданному параметру
    filteredItems () {
        if (this.filter_text) {
            return this.select_items.filter((item) => {
                return item.title.toLowerCase().includes(this.filter_text.toLowerCase())
            })
        } else { 
            return this.select_items
        }
    },

    // возвращаем название кнопки, открывающем всплывающее окно
    prepareBtnTitle () {
        return (this.select_result) ? 'Изменить ' + this.itemName : 'Выбрать ' + this.itemName
    },

    // возвращаем название выбранного значения
    prepareSelectResultTitle () {
        let  selected_item  = this.select_items.find((item) => {
            return item.value === this.select_result
        })

        return selected_item.title
    }
  },

  data: () => ({
    dialog: false,
    
    select_result: null,

    filter_text: null,

    select_items: []
  }),

  methods: {
    // подготавливаем данные для выбора
    prepareDataForSelect () {
        switch (this.url) {
            case 'department':
                this.select_items = departmentJSON
                break
            case 'address':
                this.select_items = addressJSON
                break
            default:
                this.select_items = []
                break
        }

        // здесь предполагается запрос к серверу. 
        // используем this.url, переданный из родителького компонента

        // очень грубый вариант:
        // включаем лоадер
        // this.$axios.$get(this.url)
        //  .then((result) => { this.select_items = result.data })
        //  .catch((e) => { console.log(e) })
        //  .finally(() => { выключаем лоадер })
    },

    // если родительский компонент передал сохранённое ранее значение  - отображаем его
    prepareSelectedValue () {
        this.select_result = (this.value) ? this.value : null
    },

    // отдаем результат в родительский компонент и закрываем всплывающее окно
    saveAndClose () {
        this.saveResult()
        
        this.dialog = false
    },

    // передаем результат в родительский компонент
    saveResult () {
        this.$emit('saveSelectItem', this.select_result)
    },

    // очищаем выбранное и закрываем всплывающее окно
    clearAndClose () {
        this.clearResult()

        this.dialog = false
    },

    // очищаем выбранные данные
    clearResult () {
        this.select_result = null
    }
  }
}
</script>

<style scoped>
.btn__wrapper {
    width: 100%;
    display: flex;
    justify-content: flex-end;
}
</style>