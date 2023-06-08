<template>
    <div class="v-form">
        <!-- форма ввода/редактирования данных -->
        <form class="form" v-on:submit.prevent="formSubmit"> 
            <h3 class="form-header">Заполните форму</h3>

            <div class="form-item">
                <label for="name">Имя</label>
                <input 
                class="from-input" 
                id="name" 
                type="text" 
                placeholder="Введите имя..." 
                autocomplete="off"
                v-model="user.name">
            </div>
            
            <div class="form-item">
                <label for="surname">Фамилия</label>
                <input 
                class="from-input" 
                id="surname" 
                type="text" 
                placeholder="Введите фамилию..."
                autocomplete="off"
                v-model="user.surname"
            >
            </div>

            <div class="form-item">
                <label for="age">Возраст</label>
                <input 
                class="from-input" 
                id="age" 
                type="number" 
                min="18"
                placeholder="Введите возраст..." 
                autocomplete="off"
                v-model="user.age">
            </div>

            <div class="form-item">
                <label for="work">Стаж работы</label>
                <input 
                    class="from-input" 
                    id="work" 
                    type="number" 
                    min="1" 
                    placeholder="Введите стаж работы..."
                    autocomplete="off"
                    v-model="user.work">
            </div>
            
            <div class="form-item">            
                <label for="address">Адрес</label>
                <input 
                class="from-input" 
                id="address" 
                type="text" 
                placeholder="Введите адрес..."
                autocomplete="off"
                v-model="user.address">
            </div>
            

           <input
            class="form-btn" 
            type="submit"
            value="Готово">
        </form>

        <!-- список(таблица) с введенными сотрудниками с возможностью редактирования-->
        <table class="table">
            <caption class="table-header">Список сотрудников</caption>
            <thead>            
                <tr>
                    <th class="header-row">Имя</th>
                    <th class="header-row">Фамилия</th>
                    <th class="header-row">Возраст</th>
                    <th class="header-row">Стаж работы в годах</th>
                    <th class="header-row">Адрес</th>
                    <th class="header-row">Редактировать</th>
                </tr> 
            </thead>
            <tbody>
                <tr>
                    <td 
                        class="emptyTable" 
                        colspan="6"
                        v-if="emptyTable"
                        >Таблица пуста
                    </td>
                </tr>
                <tr 
                    v-for="(u,i) in users" 
                    :key="i"
                    :u="u">
                    <td class="row">{{ u.name }}</td>
                    <td class="row">{{ u.surname }}</td>
                    <td class="row">{{ u.age }}</td>
                    <td class="row">{{ u.work }}</td>
                    <td class="row">{{ u.address}}</td>
                    <th class="edit-row">

                        <button 
                            class="update-btn"
                            value="update" 
                            @click="update(i)">
                            Изменить
                        </button>

                        <button 
                            class="delete-btn"
                            @click="deleteItem(i)"
                            >Удалить
                        </button>
                    </th>
                </tr>
            </tbody>
        </table>        
    </div>
</template>

<script setup>
    import { ref} from 'vue';
    // реактивный объект с данными одного сотрудника
    const user = ref({
        name: '',
        surname: '',
        age:'',
        work: '',
        address: ''
    })
    // реактивный список всех сотрудников
    const users = ref([])

    // переменная для надписи, если таблица пуста
    const emptyTable = ref(true)

    // функция для заполнения списка всех сотрудников для последубщего рендринга, и очистка полей инпутов
    function formSubmit(){
        emptyTable.value = false
        users.value.push(user.value)
        user.value = {
            name: '',
            surname: '',
            age:'',
            work: '',
            address: ''
        }
    }

    // функционал кнопки удаления
    const deleteItem = (i) => users.value.splice(i, 1)

    // функционал кнопки изменений
    function update(i){
        user.value = users.value[i]
        users.value.splice(i,1)
    }    

</script>


<!-- стили -->
<style>
.v-form{
    width: 90%;
    margin: auto;
}
.form {
    display: flex;
    max-width: 200px;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin: 10px auto;
}
.from-input{
    padding: 2px 5px;
    margin-top: 5px;
    width: 200px;
}
.form-header{
    text-align: center;
}
.form-item{
    margin: 10px auto;
    display: flex;
    flex-direction: column;
}
.form-btn{
    margin-top: 20px;
    padding: 3px;
    height: 24px;
    width: 200px;
}
.table-item{
    border: 1px solid black;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}
.table{
    border: 2px solid gray;
    width: 90%;
    margin: auto;
    margin-bottom: 30px;
    border-collapse: collapse;
}
.header-row{
    border: 1px solid gray;
    padding: 5px;
    text-align: center;
    font-size: 0.9rem;
}
.row{
    border: 1px solid gray;
    padding: 5px;
    text-align: center;
}
.edit-row{
    padding: 0;
}
.emptyTable{
    text-align: center;
    padding: 30px;
}
.table-header{
    font-weight: 700;
    padding: 20px;
    font-size: 1.2rem;
}
.delete-btn{
    background-color: rgb(230, 114, 114);
    border: 1px solid gray;
    border-radius: 3px;
    width: 50%;
}
.update-btn{
    border: 1px solid gray;
    border-radius: 3px;
    width: 50%;
}
</style>