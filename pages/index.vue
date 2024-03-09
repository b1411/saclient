<template>
    <form class="mx-auto px-5 max-w-4xl w-full mt-8" @submit.prevent="submitForm">
        <FormBlock title="Заполните свои данные">
            <template v-slot:inputs>
                <input type="text" required placeholder="Введите имя" name="name"
                    class="input input-bordered input-primary" v-model="name" />
                <input type="email" required placeholder="Введите email" name="email" v-model="email"
                    class="input input-bordered input-primary" />
                <input type="text" required placeholder="Введите телефон" v-model="phone" name="phone"
                    class="input input-bordered input-primary" />
            </template>
        </FormBlock>
        <FormBlock title="Планируете ли вы поступать в вузы США?" class="mt-8">

            <template v-slot:inputs>
                <label class="label cursor-pointer">
                    <span class="label-text">Да, в ближайшем будущем</span>
                    <input type="radio" name="plan" v-model="plan" value="Да, в ближайшем будущем" required
                        class="radio radio-primary" />
                </label>
                <label class="label cursor-pointer">
                    <span class="label-text">Да, но через несколько лет</span>
                    <input type="radio" name="plan" v-model="plan" value="Да, но через несколько лет"
                        class="radio radio-primary" />
                </label>
                <label class="label cursor-pointer">

                    <span class="label-text">Еще не решил(а)</span>
                    <input type="radio" name="plan" v-model="plan" value="Еще не решил(а)"
                        class="radio radio-primary" />
                </label>
                <label class="label cursor-pointer">
                    <span class="label-text">Нет, это не входит в мои планы</span>
                    <input type="radio" name="plan" v-model="plan" value="Нет" class="radio radio-primary" />
                </label>
            </template>
        </FormBlock>
        <FormBlock title="Какие образовательные программы вас наиболее интересуют?" class="mt-8">

            <template v-slot:inputs>
                <label class="label cursor-pointer">
                    <span class="label-text">Гуманитарные науки</span>
                    <input type="radio" name="programs" v-model="program" value="Гуманитарные науки"
                        class="radio radio-primary" />
                </label>
                <label class="label cursor-pointer">
                    <span class="label-text">Технические науки</span>
                    <input type="radio" name="programs" v-model="program" value="Технические науки"
                        class="radio radio-primary" />
                </label>
                <label class="label cursor-pointer">
                    <span class="label-text">Естественные науки</span>
                    <input type="radio" name="programs" v-model="program" value="Естественные науки"
                        class="radio radio-primary" />
                </label>
                <label class="label cursor-pointer">
                    <span class="label-text">Бизнес и менеджмент</span>
                    <input type="radio" name="programs" v-model="program" value="Бизнес и менеджмент"
                        class="radio radio-primary" />
                </label>
                <input type="text" placeholder="Другое" @focus="resetProgram" :required="requiredAnotherProgram"
                    v-model="anotherProgram" name="programs" class="input input-bordered input-primary" />
            </template>
        </FormBlock>
        <FormBlock title="Какие факторы важны для вас при выборе университета в США? (Выберите несколько вариантов)"
            class="mt-8">

            <template v-slot:inputs>
                <label class="label cursor-pointer">
                    <span class="label-text">Репутация университета</span>
                    <input type="checkbox" name="factors" value="Репутация университета"
                        class="checkbox checkbox-primary" v-model="factor" />
                </label>
                <label class="label cursor-pointer">
                    <span class="label-text">Стоимость обучения</span>
                    <input type="checkbox" name="factors" value="Стоимость обучения" class="checkbox checkbox-primary"
                        v-model="factor" />
                </label>
                <label class="label cursor-pointer">
                    <span class="label-text">Расположение университета</span>
                    <input type="checkbox" name="factors" value="Расположение университета"
                        class="checkbox checkbox-primary" v-model="factor" />
                </label>
                <label class="label cursor-pointer">
                    <span class="label-text">Наличие стипендий и финансовой поддержки</span>
                    <input type="checkbox" name="factors" value="Наличие стипендий и финансовой поддержки"
                        class="checkbox checkbox-primary text-pretty" v-model="factor" />
                </label>
                <label class="label cursor-pointer">
                    <span class="label-text
                        ">Специфика программы обучения</span>
                    <input type="checkbox" name="factors" value="Специфика программы обучения"
                        class="checkbox checkbox-primary" v-model="factor" />
                </label>
                <input type="text" placeholder="Другое" @focus="resetFactor" v-model="anotherFactor"
                    :required="requiredAnotherFactor" name="other" class="input input-bordered input-primary" />
            </template>
        </FormBlock>
        <FormBlock title="Как вы видите кампус своей мечты:" class="mt-8">

            <template v-slot:inputs>
                <input type="text" required placeholder="Введите текст" name="campus" v-model="campus"
                    class="input input-bordered input-primary" />
            </template>
        </FormBlock>
        <FormBlock title="Загрузите свое фото" class="mt-8">

            <template v-slot:inputs>
                <input @change="setUserImage" type="file" required accept="image/*" name="photo"
                    class="file-input file-input-bordered file-input-primary" />
            </template>
        </FormBlock>
        <FormBlock class="mt-8">
            <template v-slot:inputs>
                <label class="label cursor-pointer">
                    <span class="label-text">Удалить фон с вашего изображения</span>
                    <input type="checkbox" v-model="rembg" class="checkbox checkbox-primary" />
                </label>
            </template>
        </FormBlock>
        <div class="w-full my-8 flex flex-row justify-center">
            <button type="submit" class="btn btn-primary px-20">Отправить</button>
        </div>
        <img v-if="processedImage" :src="pathProcessedImage" alt="processed image" class="w-1/2 mx-auto" />
    </form>
</template>

<script setup>
let plan = ref(null)
let name = ref('')
let email = ref('')
let phone = ref('')
let program = ref('')
let factor = ref([])
let anotherProgram = ref('')
let requiredAnotherProgram = ref(false)
let requiredAnotherFactor = ref(false)
let anotherFactor = ref('')
let userImage = ref(null)
let campus = ref('')
let rembg = ref(false);
let processedImage = ref(null);

let pathProcessedImage = computed(() => (`https://https://rakhmat.ninja/${processedImage.value}`))

function setUserImage(e) {
    const file = e.target.files[0]
    if (file.size > 2 * 1024 * 1024) {
        useNuxtApp().$toast.dark("Размер файла не должен превышать 2 МБ", {
            type: "error"
        })
        userImage.value = null
        e.target.value = null
        return
    }
    userImage.value = file
}

watchEffect(() => {
    if (!program.value) {
        requiredAnotherProgram.value = true
    } else {
        requiredAnotherProgram.value = false
    }
    if (factor.value.length === 0) {
        requiredAnotherFactor.value = true
    } else {
        requiredAnotherFactor.value = false
    }
})

function resetProgram() {
    program.value = ''
}

function resetFactor() {
    factor.value = []
}

async function submitForm() {
    let formData = new FormData()
    formData.append('name', name.value)
    formData.append('email', email.value)
    formData.append('phone', phone.value)
    formData.append('studyPlan', plan.value)
    formData.append('studyProgram', !!!program.value ? anotherProgram.value : program.value)
    formData.append('factors', factor ? factor.value.join(', ') : anotherFactor.value)
    formData.append('campus', campus.value)
    formData.append('photo', userImage.value, userImage.value?.name)
    formData.append('rembg', rembg.value)

    let res = null;
    try {
        res = await fetch('https://rakhmat.ninja/user', {
            method: "post",
            body: formData,
        });
        let data = await res.json();
        if (res.status === 200) {
            processedImage.value = data.filepath
            useNuxtApp().$toast.dark(data.message, {
                type: "success"
            })
        } else {
            useNuxtApp().$toast.dark(data.message, {
                type: "error"
            })
        }
    }
    catch (e) {
        useNuxtApp().$toast.dark('Ошибка отправки формы', {
            type: "error",
        })
    }
}

</script>