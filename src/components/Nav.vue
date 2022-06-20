<template>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
            <div class="container px-4 px-lg-5">
                <a class="navbar-brand" href="/">Магазин Электросамокатов</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation"><span class="navbar-toggler-icon"></span></button>
                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav me-auto mb-2 mb-lg-0 ms-lg-4">
                        <li>
                            <a class="navbar nav-link" href="">Каталог</a>
                        </li>
                        <li class="nav-item"><a class="nav-link" href="">Регистрация</a></li>
                        <li class="nav-item"><a class="nav-link" href="">Войти</a></li>
                    </ul>
                    <form class="d-flex">
                        <a class="btn btn-outline-dark mt-auto" href="">Корзина <span class="badge bg-dark text-white ms-1 rounded-pill"></span></a>
                    </form>
                </div>
            </div>
    </nav>
</template>

<script>
    export default {
        name: "Nav",
    }

</script>

<style>

</style>