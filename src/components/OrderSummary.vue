<template>
    <div class="box mb-4">
        <h3 class="is-size-4 mb-6">Заказ #{{ order.id }}</h3>
        <h4 class="is-size-4 mb-6">Сумма: ${{ order.paid_amount }}</h4>
        <h4 class="is-size-4 mb-6">Статус:
            <span v-if="order.status === 'in_progress'">В обработке</span>
            <span v-else>Заказ выполнен</span>
        </h4>

        <table class="table is-fullwidth">
            <thead>
                <tr>
                    <th>Товар</th>
                    <th>Цена</th>
                    <th>Количество</th>
                    <th>Сумма</th>
                </tr>
            </thead>

            <tbody>
                <tr
                    v-for="item in order.items"
                    v-bind:key="item.product.id"
                >
                    <td>{{ item.product.title }}</td>
                    <td>${{ item.product.get_price }}</td>
                    <td>{{ item.quantity }}</td>
                    <td>${{ getItemTotal(item).toFixed(2) }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: 'OrderSummary',
    props: {
        order: Object
    },
    methods: {
        getItemTotal(item) {
            return item.quantity * item.product.get_price
        },
        orderTotalLength(order) {
            return order.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        },
    }
}
</script>