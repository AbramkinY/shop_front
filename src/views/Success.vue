<template>
    <div class="page-success">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Спасибо за заказа!</h1>

                <p>Закза оформлен! Наш менеджер скоро свяжется с вами!</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Success',
    mounted() {
        document.title = 'Success'
    },
}
</script>