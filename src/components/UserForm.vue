<template>
    <form class="form">
        <div class="form__wrapper">
            <h1 class="my-5">Профиль сотрудника</h1>

            <v-row class="mt-3">
                <v-col cols="12" sm="4">Имя</v-col>

                <v-col cols="12" sm="8">
                    <v-text-field
                        v-model="employee.name"
                    />
                </v-col>
            </v-row>

            <v-row>
                <v-col cols="12" sm="4">Фамилия</v-col>

                <v-col cols="12" sm="8">
                    <v-text-field
                        v-model="employee.surname"
                    />
                </v-col>
            </v-row>

            <v-row>
                <v-col cols="12" sm="4">Отделение</v-col>
                <v-col cols="12" sm="8">
                    <UiComplexSelect
                        :item-name="'отделение'"
                        :url="'department'"
                        :value="employee.department ? employee.department : null"
                        @saveSelectItem="(value) => saveSelectItem('department', value)"
                    />
                </v-col>
            </v-row>

            <v-row>
                <v-col cols="12" sm="4">Филлиал</v-col>
                <v-col cols="12" sm="8">
                    <UiComplexSelect
                        :item-name="'филиал'"
                        :url="'address'"
                        :value="employee.address ? employee.address : null"
                        @saveSelectItem="(value) => saveSelectItem('address', value)"
                    />
                </v-col>
            </v-row>
        </div>
    </form>
</template>

<script>
import UiComplexSelect from './ui/UiComplexSelect.vue'

export default {
    name: 'UserForm',

    components: {
        UiComplexSelect
    },


    data: () => ({
        employee: {
            name: null,
            surname: null,
            department: null,
            address: null
        }
    }),

    methods: {
        // сохраняем выбранные данные
        saveSelectItem (item_for_select, value) {
            this.employee[item_for_select] = value
        }
    }
}
</script>

<style scoped>
.form {
    display: flex;
}
.form__wrapper {
    width: 1000px;
    margin-right: auto;
    margin-left: auto;
    padding:10px;
}
</style>